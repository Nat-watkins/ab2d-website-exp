---
layout: api-docs
page_title: "API Documentation"
seo_title: "API Documentation | AB2D Medicare Enrollee Claims Data"
description: "Access comprehensive API documentation for AB2D, including instructions on how to access sandbox and production claims data."
permalink: /api-documentation
in-page-nav: true
---

# {{ page.page_title }}

The AB2D API provides Medicare Parts A and B claims data to active, stand-alone Prescription Drug Plan (PDP) sponsors. It uses the [FHIR Bulk Data Export](https://hl7.org/fhir/uv/bulkdata/) standard to deliver [ExplanationOfBenefit](https://hl7.org/fhir/R4/explanationofbenefit.html) resources in NDJSON format.

{% capture versionAlert %}
AB2D offers two API versions. Version 2 (FHIR R4) is recommended for all new integrations. Version 1 (FHIR STU3) is still supported but does not include the <code>_until</code> parameter.
{% endcapture %}
{% include alert.html variant="info" text=versionAlert classNames="measure-6" %}

## Getting started

Follow these steps to start using the AB2D API.

<ol class="usa-process-list margin-top-1">
  <li class="usa-process-list__item">
    <p class="usa-process-list__heading">Review the available data</p>
    <p>
      Explore the <a href="{{ '/ab2d-data#data-dictionary' | relative_url }}">Data Dictionary</a> and <a href="{{ '/ab2d-data#sample-files' | relative_url }}">sample files</a> to understand what claims data AB2D provides and how it is structured.
    </p>
  </li>
  <li class="usa-process-list__item">
    <p class="usa-process-list__heading">Set up your environment</p>
    <p>
      Install <a href="{{ '/setup-instructions' | relative_url }}">curl and jq</a> or another HTTP client. You can also use the <a href="https://sandbox.ab2d.cms.gov/swagger-ui/index.html?urls.primaryName=V2%20-%20FHIR%20R4" target="_blank" rel="noopener">AB2D Swagger UI</a> to interact with the API in your browser.
    </p>
  </li>
  <li class="usa-process-list__item">
    <p class="usa-process-list__heading">Get a bearer token</p>
    <p>
      <a href="{{ '/get-a-bearer-token' | relative_url }}">Authenticate with your credentials</a> to receive a bearer token. You need a valid token for every API request.
    </p>
  </li>
  <li class="usa-process-list__item">
    <p class="usa-process-list__heading">Access sandbox data</p>
    <p>
      The sandbox is open to everyone. <a href="{{ '/access-sandbox-data' | relative_url }}">Try the API with synthetic data</a> to learn the workflow before requesting production access.
    </p>
  </li>
  <li class="usa-process-list__item">
    <p class="usa-process-list__heading">Get production access</p>
    <p>
      PDP sponsors must complete <a href="{{ '/production-access' | relative_url }}">attestation and onboarding</a> to access their enrollees' real claims data.
    </p>
  </li>
</ol>

## Sandbox vs. production

Both environments use the same endpoints and workflow. The key differences are credentials and data.

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
      <th scope="row">Access</th>
      <td>Open to everyone</td>
      <td>PDP sponsors with <a href="{{ '/production-access' | relative_url }}">production access</a></td>
    </tr>
    <tr>
      <th scope="row">URL</th>
      <td>sandbox.ab2d.cms.gov</td>
      <td>api.ab2d.cms.gov</td>
    </tr>
    <tr>
      <th scope="row">Identity provider</th>
      <td>test.idp.idm.cms.gov</td>
      <td>idm.cms.gov</td>
    </tr>
    <tr>
      <th scope="row">Data</th>
      <td>Synthetic claims data</td>
      <td>Real Medicare enrollee claims data</td>
    </tr>
    <tr>
      <th scope="row">IP restrictions</th>
      <td>None</td>
      <td>Static IP allowlist required</td>
    </tr>
  </tbody>
</table>

## API workflow

The AB2D API uses an asynchronous bulk data export pattern. Each export follows 4 steps:

<table class="usa-table usa-table--stacked usa-table--borderless">
  <caption class="usa-sr-only">API workflow steps</caption>
  <thead>
    <tr>
      <th scope="col">Step</th>
      <th scope="col">What you do</th>
      <th scope="col">Endpoint</th>
      <th scope="col">Duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th data-label="Step" scope="row">1. Get a bearer token</th>
      <td data-label="What you do">Authenticate with your credentials to get a token</td>
      <td data-label="Endpoint"><a href="{{ '/get-a-bearer-token' | relative_url }}">Authentication guide</a></td>
      <td data-label="Duration">Seconds (expires after 30 minutes)</td>
    </tr>
    <tr>
      <th data-label="Step" scope="row">2. Start an export job</th>
      <td data-label="What you do">Request an export and save the job ID from the response</td>
      <td data-label="Endpoint"><code>GET /api/v2/fhir/Patient/$export</code></td>
      <td data-label="Duration">Seconds</td>
    </tr>
    <tr>
      <th data-label="Step" scope="row">3. Check job status</th>
      <td data-label="What you do">Poll until the job completes (200 response)</td>
      <td data-label="Endpoint"><code>GET /api/v2/fhir/Job/{jobId}/$status</code></td>
      <td data-label="Duration">Minutes to hours</td>
    </tr>
    <tr>
      <th data-label="Step" scope="row">4. Download files</th>
      <td data-label="What you do">Download NDJSON files from the URLs in the completed response</td>
      <td data-label="Endpoint"><code>GET /api/v2/fhir/Job/{jobId}/file/{fileName}</code></td>
      <td data-label="Duration">Minutes to hours</td>
    </tr>
  </tbody>
</table>

You can also cancel an in-progress job with `DELETE /api/v2/fhir/Job/{jobId}/$status` or check the server's capabilities with `GET /api/v2/fhir/metadata`.

Use [query parameters]({{ '/query-parameters-v2' | relative_url }}) like `_since` and `_until` to filter the claims data returned by an export job.

## JSON resources

AB2D delivers data in NDJSON (Newline Delimited JSON) format using the FHIR ExplanationOfBenefit resource type.

- [Intro to JSON format](http://json.org/)
- [NDJSON specification](https://github.com/ndjson/ndjson-spec)
- [JSON format viewer/validator](https://jsonlint.com/)

## Glossary

<div class="padding-top-4"></div>

{% capture a1AccordionContent %}
  APIs allow software systems and applications to communicate with each other. APIs follow unique definitions and protocols. The AB2D API is publicly available and follows the <a href="https://hl7.org/fhir/uv/bulkdata/">FHIR Bulk Data Export</a> standard.
{% endcapture %}

{% capture a2AccordionContent %}
  A basic encoding that converts your credentials from plain text to a standard ASCII format for safe transmission. You encode your client ID and password in Base64 format when requesting a bearer token.
{% endcapture %}

{% capture a3AccordionContent %}
  <p>
    An <a href="https://www.okta.com/identity-101/why-your-company-needs-an-identity-provider/" target="_blank" rel="noopener">IdP</a> is a service that stores, verifies, and manages user identities. AB2D uses <a href="https://support.okta.com/help/s/article/What-is-Okta?language=en_US" target="_blank" rel="noopener">Okta</a> as its IdP. You authenticate with your Okta credentials to get a bearer token and access the API.
  </p>
{% endcapture %}

{% capture a4AccordionContent %}
<p>
  A bearer token (also called an access token or <a href="https://jwt.io/introduction/" target="_blank" rel="noopener">JSON web token</a>) authorizes your API requests through <a href="https://oauth.net/2/" target="_blank" rel="noopener">OAuth 2.0</a>. You need a valid bearer token for every request to the sandbox or production environment.
</p>
<p>
  Get a bearer token by authenticating with your sandbox or <a href="{{ '/production-access' | relative_url }}">production</a> credentials. Tokens expire after 30 minutes.
</p>
{% endcapture %}

{% include accordion.html id="a1" heading="Application Programming Interface (API)" expanded=true bordered=false accordionContent=a1AccordionContent %}

{% include accordion.html id="a2" heading="Base64" expanded=false bordered=false accordionContent=a2AccordionContent %}

{% include accordion.html id="a3" heading="Identity Provider (IdP) and Okta" expanded=false bordered=false accordionContent=a3AccordionContent %}

{% include accordion.html id="a4" heading="Bearer token" expanded=false bordered=false accordionContent=a4AccordionContent %}

## Next step

Ready to try the API? Start by [getting a bearer token]({{ '/get-a-bearer-token' | relative_url }}).

{% include feedback-form.html id="f42c76e3" %}
