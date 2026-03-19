---
layout: api-docs
page_title: "Sandbox Guide"
seo_title: "Sandbox Guide | AB2D Medicare API"
description: "Connect to the AB2D API and access test Medicare Parts A & B claims data in the sandbox environment."
permalink: /sandbox-guide
redirect_from: [/access-sandbox-data]
in-page-nav: true
---

# {{ page.page_title }}

The sandbox environment (sandbox.ab2d.cms.gov) is open to anyone who wants to try the API. You need a [bearer token]({{ '/authentication' | relative_url }}) before you begin.

{% capture sandboxAlert %}
The sandbox contains synthetic data only. No real patient information is used in the sandbox environment.
{% endcapture %}
{% include alert.html variant="info" text=sandboxAlert classNames="measure-6" %}

{% capture versionAlertHeading %}
  <p class="usa-alert__heading text-bold">
    AB2D recommends using v2 of the API
  </p>
{% endcapture %}
{% capture versionAlert %}
    <p>
        Version 2 follows the <a href="https://hl7.org/fhir/R4/" target="_blank" rel="noopener">FHIR R4 standard</a> and includes the <code>_until</code> parameter. Version 1 follows the <a href="https://hl7.org/fhir/STU3/explanationofbenefit.html" target="_blank" rel="noopener">FHIR STU3</a> standard.
    </p>
    <p>
        <a href="https://github.com/CMSgov/ab2d-pdp-documentation/raw/main/AB2D%20STU3-R4%20Migration%20Guide%20Final.xlsx" target="_blank" rel="noopener">Learn more about migrating from v1 to v2</a>.
    </p>
{% endcapture %}
{% include alert.html variant="info" text=versionAlert heading=versionAlertHeading classNames="measure-6" %}

## Sandbox vs. production

<table class="usa-table usa-table--borderless">
  <caption class="usa-sr-only">Comparison of sandbox and production environments</caption>
  <thead>
    <tr>
      <th scope="col"></th>
      <th scope="col">Sandbox</th>
      <th scope="col">Production</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">URL</th>
      <td>sandbox.ab2d.cms.gov</td>
      <td>api.ab2d.cms.gov</td>
    </tr>
    <tr>
      <th scope="row">Access</th>
      <td>Open to everyone</td>
      <td>PDP sponsors with <a href="{{ '/production-access' | relative_url }}">production access</a></td>
    </tr>
    <tr>
      <th scope="row">Data</th>
      <td>Synthetic claims data</td>
      <td>Real Medicare enrollee data</td>
    </tr>
    <tr>
      <th scope="row">IP restrictions</th>
      <td>None</td>
      <td>Static IP allowlist required</td>
    </tr>
  </tbody>
</table>

## Step 1: Start an export job

Start an export job to request claims data. The API returns a `202 Accepted` response with a job ID in the `content-location` header.

{% capture curlSnippet %}{% raw %}
RESP2=$(curl -i "https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/\$export?_type=ExplanationOfBenefit" \
  -H "Accept: application/fhir+json" \
  -H "Authorization: Bearer ${TOKEN}")
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

Expected response headers:

{% capture curlSnippet %}{% raw %}
HTTP/2 202
content-location: https://sandbox.ab2d.cms.gov/api/v2/fhir/Job/2356b9af-9257-41f4-9d82-4e27542ff1be/$status
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" %}

Extract and save the job ID for later steps:

{% capture curlSnippet %}{% raw %}
JOB_ID=$(echo $RESP2 | grep content-location | sed 's%^.*Job/\([^/]*\).*$%\1%')
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

You can use [query parameters]({{ '/api-reference' | relative_url }}) like `_since` and `_until` to filter the claims data returned. If a job takes more than 30 hours, it will time out — use parameters to reduce the data scope.

## Step 2: Check job status

Poll the status endpoint until the job completes. A `202` response means the job is still in progress. A `200` response means it is complete and files are ready.

{% capture curlSnippet %}{% raw %}
curl -sw '%{http_code}' -o status.json "https://sandbox.ab2d.cms.gov/api/v2/fhir/Job/${JOB_ID}/\$status"  \
  -H "Accept: application/fhir+json" \
  -H "Authorization: Bearer ${TOKEN}" | {
       read STATUS
       echo "Status: " $STATUS
  }
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

{% capture statusAlert %}
Avoid checking status too frequently. If you make too many requests, you may receive a <code>429 Too Many Requests</code> response with a <code>Retry-After</code> header. Wait before making another request.
{% endcapture %}
{% include alert.html variant="info" text=statusAlert classNames="measure-6" %}

When the job is complete, the response contains download URLs for the export files. Extract the file name:

{% capture curlSnippet %}{% raw %}
FILE=$(cat status.json | jq -r ".output[0].url" | sed 's%^.*file/\(.*$\)%\1%')
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

## Step 3: Download your files

Download the claims data files using the job ID and file name:

{% capture curlSnippet %}{% raw %}
RESP4=$(curl "https://sandbox.ab2d.cms.gov/api/v2/fhir/Job/${JOB_ID}/file/${FILE}" \
  -H "Accept: application/fhir+ndjson" \
  -H "Accept-Encoding: gzip" \
  -H "Authorization: Bearer ${TOKEN}")
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

{% capture downloadAlert %}
Files and job IDs expire after 72 hours. Download your files promptly. You can request compressed files with the <code>Accept-Encoding: gzip</code> header to speed up downloads.
{% endcapture %}
{% include alert.html variant="info" text=downloadAlert classNames="measure-6" %}

### Working with downloaded files

The data is in NDJSON format, with one JSON object per line. Files for even a small contract can be large (100 enrollees produces over 25MB). To preview the data:

{% capture curlSnippet %}{% raw %}
echo $RESP4 | sed 1q | jq
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

This extracts and formats the first record. Use a code editor that supports JSON folding (like VS Code or IntelliJ) for exploring larger files.

## Step 4: Cancel a job (optional)

You can cancel an in-progress job. Completed jobs cannot be canceled.

{% capture curlSnippet %}{% raw %}
curl -X DELETE "https://sandbox.ab2d.cms.gov/api/v2/fhir/Job/${JOB_ID}/\$status" \
  -H "Authorization: Bearer ${TOKEN}"
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

## Other endpoints

**FHIR metadata**: Retrieve the server's [CapabilityStatement](https://hl7.org/fhir/R4/capabilitystatement.html) resource:

{% capture curlSnippet %}{% raw %}
curl "https://sandbox.ab2d.cms.gov/api/v2/fhir/metadata" \
  -H "Accept: application/fhir+json"
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

## Using Swagger UI

You can also interact with the API through the [AB2D Swagger UI](https://sandbox.ab2d.cms.gov/swagger-ui/index.html?urls.primaryName=V2%20-%20FHIR%20R4) in your browser.

### Authorize your token

1. Open the Swagger UI and select **v2 - FHIR R4** from the *Select a definition* dropdown.
2. Select the **Authorize** button.
3. Enter your token in the format `Bearer {your_token}`.
4. Select **Authorize**, then **Close**.

### Start an export job

1. Expand the **Export** command `/api/v2/fhir/Patient/$export`.
2. Select **Try it out**.
3. Optionally add dates in [ISO datetime format](https://en.wikipedia.org/wiki/ISO_8601) for the `_since` and `_until` [parameters]({{ '/api-reference' | relative_url }}).
4. Select **Execute**. Copy the job ID from the `content-location` in the response header.

### Check status and download

1. Expand the **Status** command and enter your job ID. Select **Execute**.
2. A `202` response means the job is in progress — re-execute periodically. A `200` response returns file URLs.
3. Expand the **Download** command. Enter the job ID and file name from the status response.
4. Select **Execute**, then **Download file**. The file is in [NDJSON format](https://github.com/ndjson/ndjson-spec).

## Troubleshooting

Visit the [Troubleshooting Guide]({{ '/troubleshooting-guide' | relative_url }}) for help with HTTP response codes and common issues. If you need additional assistance, email [ab2d@cms.hhs.gov](mailto:ab2d@cms.hhs.gov) with:

- Your operating system
- The HTTP response code you received
- A description of the issue and which step you are on
- Any relevant logs

## Next step

After successfully using the sandbox, follow the steps for [production access]({{ '/production-access' | relative_url }}) to get your enrollees' real claims data.

{% include feedback-form.html id="71834290" %}
