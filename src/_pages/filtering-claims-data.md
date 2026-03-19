---
layout: api-docs
page_title: "Filtering Claims Data"
seo_title: "Filter Claims Data With Parameters | AB2D Medicare API"
description: "Filter and refine AB2D's Medicare claims data using HTTP query parameters to reduce duplication and speed up job times."
permalink: /filtering-claims-data
redirect_from:
  - /filter-claims-data-v1
  - /filter-claims-data-v2
in-page-nav: true
---

# {{ page.page_title }}

[HTTP query parameters]({{ '/api-reference' | relative_url }}) can help you efficiently maximize the value of the AB2D API. The default behavior of parameters supports the incremental export model and varies depending on your API version. You can use parameters while starting a job request in the sandbox or production environment.

Learn how to access [sandbox data]({{ '/sandbox-guide' | relative_url }}) or [production claims data]({{ '/production-guide' | relative_url }}).

{% capture versionAlertHeading %}
  <p class="usa-alert__heading text-bold">
    AB2D recommends using v2 of the API
  </p>
{% endcapture %}
{% capture versionAlert %}
    <p>
        Version 2 implements the <a href="https://hl7.org/fhir/uv/bulkdata/" target="_blank" rel="noopener">Bulk Data Access Implementation Guide v2.0.0</a>. The _until parameter is only available with v2. Version 1 implements the <a href="https://hl7.org/fhir/uv/bulkdata/STU1.0.1/" target="_blank" rel="noopener">Bulk Data Access Implementation Guide v1.0.1</a>.
    </p>
    <p>
        <a href="https://github.com/CMSgov/ab2d-pdp-documentation/raw/main/AB2D%20STU3-R4%20Migration%20Guide%20Final.xlsx" target="_blank" rel="noopener">Learn more about migrating from v1 to v2</a>.
    </p>
{% endcapture %}
{% include alert.html variant="info" text=versionAlert heading=versionAlertHeading classNames="measure-6" %}

## Filtering with v2 (recommended)

### The _since and _until parameters

The [_since and _until parameters]({{ '/api-reference' | relative_url }}#the-since-and-until-parameters) filter for claims data last updated since or until a specified date. These can be used while starting a job to speed up download times and reduce duplication. Visit <a href="{{ '/get-started' | relative_url }}#api-workflow">Get Started</a> to learn more about the expected workflow for the AB2D API.

The following examples use the _since and _until parameters separately and together. Note the ISO8601 dates include characters that can't appear in URLs. [Learn more about percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding). There are unencoded and percent-encoded examples. Only the encoded versions will work, but the unencoded examples show how the URL is formed before encoding.

#### When only the _since parameter is specified

##### Unencoded:

`https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/$export?_since=2023-02-13T00:00:00.000-05:00`

##### Percent-encoded:

`https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/$export?_since%3D2023-02-13T00%3A00%3A00.000-05%3A00`

##### curl command

{% capture curlSnippet %}{% raw %}
curl -i "https://api.ab2d.cms.gov/api/v2/fhir/Patient/\$export?_since%3D2023-02-13T00%3A00%3A00.000-05%3A00" \
-H "Accept: application/json" \
-H "Accept: application/fhir+json" \
-H "Prefer: respond-async" \
-H "Authorization: Bearer ${bearer_token}"
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

#### When only the _until parameter is specified

##### Unencoded

`https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/$export?_until=2024-06-15T00:00:00.000-05:00`

##### Percent-encoded

`https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/$export?_until%3D2024-06-15T00%3A00%3A00.000-05%3A00`

##### curl command

{% capture curlSnippet %}{% raw %}
curl -i "https://api.ab2d.cms.gov/api/v2/fhir/Patient/\$export?_until%3D2024-06-15T00%3A00%3A00.000-05%3A00" \-H "accept: application/json" \
-H "Accept: application/fhir+json" \
-H "Prefer: respond-async" \
-H "Authorization: Bearer ${bearer_token}"
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

#### When both the _since and _until parameters are specified

##### Unencoded

`https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/$export?_since=2023-06-15T00:00:00.000-05:00&_until=2024-06-15T00:00:00.000-05:00`

##### Percent-encoded

`https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/$export?_since%3D2023-06-15T00%3A00%3A00.000-05%3A00%26_until%3D2024-06-15T00%3A00%3A00.000-05%3A00`

##### curl command

{% capture curlSnippet %}{% raw %}
curl -i "https://api.ab2d.cms.gov/api/v2/fhir/Patient/\$export?_since%3D2023-06-15T00%3A00%3A00.000-05%3A00%26_until%3D2024-06-15T00%3A00%3A00.000-05%3A00" \-H "accept: application/json" \
-H "Accept: application/fhir+json" \
-H "Prefer: respond-async" \
-H "Authorization: Bearer ${bearer_token}"
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

### Incremental export model (v2)

One recommended way to use AB2D is to periodically export any data that has been updated since your last job. The AB2D team encourages bi-weekly exports to stay updated on new claims.

Incremental exports occur by default when you start a job, even without using parameters, so no further action is required. This is because _since defaults to the datetime of your last successful export, and _until defaults to the current datetime.

#### Example scenario

This is a scenario demonstrating how the default capability works for _since and _until:

1. You start your first job (JOB ID: ABC).
2. Once the job is complete, you download the files.
3. You wait until the next bi-weekly update (the recommended frequency to run jobs).
4. You start a new job (JOB ID: DEF).
5. You forget to download the files until after they've expired (72 hours).
6. You start another job (JOB ID: GHI). The _since value will default to when you started job ABC, not job DEF. This is because no files were downloaded with job DEF, so it wasn't completed successfully.
7. Once the job is complete, download the files.
8. Repeat these steps.

In this usage model, AB2D automatically populates the _since value with the datetime of your last successfully completed export. For Job GHI, this is the date job ABC was created. Job DEF isn't considered a successfully completed export since its files weren't downloaded.

## Filtering with v1

{% capture v1Alert %}
Version 1 uses FHIR STU3. We recommend using v2 (FHIR R4) for new integrations. The _until parameter is only available with v2. <a href="https://github.com/CMSgov/ab2d-pdp-documentation/raw/main/AB2D%20STU3-R4%20Migration%20Guide%20Final.xlsx" target="_blank" rel="noopener">Learn more about migrating from v1 to v2</a>.
{% endcapture %}
{% include alert.html variant="warning" text=v1Alert classNames="measure-6" %}

### The \_since parameter (v1)

<p> The _since parameter allows users to filter claims data returned by date, which reduces duplication and speeds up job times. It follows the <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank" rel="noopener">ISO datetime format</a> (yyyy-mm-dd'T'hh:mm:ss[+|-]hh:mm). The time zone must be specified using + or - followed by hh:mm.</p>

The \_since parameter allows users to pull data that has been last updated since a specified date. The earliest possible date is January 1, 2020 (2020-01-01T00:00:00-05:00) or your organization's attestation date, whichever is later. If no \_since date is specified, it will default to your earliest possible date. It is highly recommended and considered best practice to use the \_since parameter on v1 API calls.

### Valid and invalid \_since parameter values

<table class="usa-table usa-table--stacked usa-table--borderless">
  <caption class="usa-sr-only">Valid and invalid \_since parameter values</caption>
    <thead>
        <tr>
            <th scope="col">_since</th>
            <th scope="col">Is it valid?</th>
            <th scope="col">Why?</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td data-label="_since">2019-12-30T00:00:00.000+00:00</td>
            <td data-label="Is it valid?">No</td>
            <td data-label="Why?">The datetime is before January 1, 2020. The job will still run, but the value will be replaced with your earliest possible date.</td>
        </tr>
        <tr>
            <td data-label="_since">2020-10-10T00:00:00.000</td>
            <td data-label="Is it valid?">No</td>
            <td data-label="Why?">A timezone must be provided with the datetime.</td>
        </tr>
        <tr>
            <td data-label="_since">2020-10-10T16:00:00.000+10:00</td>
            <td data-label="Is it valid?">Yes</td>
            <td data-label="Why?">The datetime is after January 1, 2020 and follows the ISO format.</td>
        </tr>
        <tr>
            <td data-label="_since">2020-10-10T16:00:00.000-10:00</td>
            <td data-label="Is it valid?">Yes</td>
            <td data-label="Why?">The datetime is after January 1, 2020 and follows the ISO format.</td>
        </tr>
    </tbody>
</table>

### Why the \_since parameter is important

In this scenario, your organization attests on March 1, 2020. Your organization runs its first job on November 1, 2020 and subsequently decides to pull data once a month. Your organization runs its second job on December 1, 2020.

#### Without the \_since parameter

1. The first job takes 4 hours to export all data available to you added between March 1, 2020 and November 1, 2020.
2. The second job takes 4 and ½ hours to export all data available to you added between March 1, 2020 and December 1, 2020.

Without the \_since parameter, the second job will pull all data from March 1st–December 1, 2020. Most of the data pulled is duplicate data. The only new data pulled is from the past month. Using the \_since parameter can prevent this.

#### With the \_since parameter

With the \_since parameter manually set to November 1, 2020 on the second job:

1. The first job takes 4 hours to export all data available to you between March 1, 2020 and November 1, 2020.
2. The second job takes ½ hour to only export claims data available to you between November 1, 2020 and December 1, 2020.

With the \_since parameter, the second job will only pull data from November 1–December 1. Only pulling data for the past month will be quicker and easier. Though a smaller number of claims may be pulled, a few may still be duplicates of already pulled claims.

### Incremental export model overview (v1)

One recommended way to use AB2D is to periodically export any data that has been updated since your last job. We encourage bi-weekly exports to stay updated on new claims data. By using the \_since parameter, you can filter for claims data last updated since your last job export. This way, you can run manual incremental exports.

#### Example scenario

1. Start a job (JOB ID: ABC), successfully download the files, and save the datetime the job started. The datetime ABC starts will be the \_since parameter value for your next job.
2. Wait until the next bi-weekly update (the recommended frequency to run jobs).
3. Start a job (JOB ID: DEF), manually entering the \_since parameter with the datetime ABC started. The datetime DEF starts will be the \_since parameter value for your next job.
4. Repeat steps 2 and 3 indefinitely.

In this usage model, an organization only pulls the last 2 weeks of claims data delivered to the AB2D API. Please note there may be duplicates returned even when using the \_since parameter. AB2D errs on the side of potentially providing a claim twice rather than potentially never providing a claim.

#### Example export workflow for parameters (v1)

The \_since parameter can be used while starting an export job. Visit <a href="{{ '/get-started' | relative_url }}#api-workflow">Get Started</a> to learn more about the expected workflow for the AB2D API. Review both unencoded and percent-encoded examples of the \_since parameter. Note that ISO8601 dates include characters that can't appear in URLs.
[Learn more about percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding). Only the encoded versions will work, but the unencoded examples will show how the URL is formed before encoding.

##### Unencoded

`https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/$export?_since=2023-02-13T00:00:00.000-05:00`

##### Percent-encoded

`https://sandbox.ab2d.cms.gov/api/v2/fhir/Patient/$export?_since%3D2023-02-13T00%3A00%3A00.000-05%3A00`

##### curl command

{% capture curlSnippet %}{% raw %}
curl -i "https://api.ab2d.cms.gov/api/v1/fhir/Patient/\$export?_since%3D2023-02-13T00%3A00%3A00.000-05%3A00" \
-H "Accept: application/json" \
-H "Accept: application/fhir+json" \
-H "Prefer: respond-async" \
-H "Authorization: Bearer ${bearer_token}"
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

## Next step

Learn more about the [Data Dictionary]({{ '/data-dictionary' | relative_url }}) including how claim versions, statuses, and identifiers work.

{% include feedback-form.html id="85f95067" %}
