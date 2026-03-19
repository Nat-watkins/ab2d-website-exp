---
layout: api-docs
page_title: "Production Access"
seo_title: "Production Access to Claims Data for PDP Sponsors | AB2D API"
description: "Prescription Drug Plan sponsors can access enrollees' Parts A & B Medicare claims data with production access to the AB2D API."
permalink: /production-access
in-page-nav: true
in-page-nav-levels: "h2"
show-side-nav: false
---

# {{ page.page_title }}

Only active, stand-alone Prescription Drug Plan (PDP) sponsors can access enrollee claims data in the production environment.

{% capture eligibilityAlert %}
PACE and MA-PD organizations are not eligible for AB2D production access.
{% endcapture %}
{% include alert.html variant="info" text=eligibilityAlert classNames="measure-6" %}

Before you begin, make sure you have tried the API in the [sandbox environment]({{ '/access-sandbox-data' | relative_url }}) and are familiar with the [API workflow]({{ '/api-documentation' | relative_url }}#api-workflow).

<ol class="usa-process-list margin-top-4">
  <li class="usa-process-list__item">
    <h2 class="usa-process-list__heading margin-bottom-2">Attest to AB2D data protocols</h2>
      <span class="usa-tag bg-accent-cool-darker">Completed by Attestor</span>
      <p>
          A current CEO, CFO, or COO ("Attestor") from your organization must agree to the Claims Data Usage Protocols. These protocols include <a href="https://www.federalregister.gov/documents/2019/04/16/2019-06822/medicare-and-medicaid-programs-policy-and-technical-changes-to-the-medicare-advantage-medicare#page-15745" target="_blank" rel="noopener">legal limitations on data use and disclosure</a>.
      </p>
      <p>
          Log in to the <a href="https://hpms.cms.gov/app/ng/cda/attestations" target="_blank" rel="noopener">Health Plan Management System (HPMS)</a> and select <i>Claims Data Attestation</i> under <i>Contract Management</i>. We encourage you to designate multiple Attestors to prevent gaps in access if one leaves your organization.
      </p>
      <div class="padding-top-4 usa-accordion usa-accordion--multiselectable" data-allow-multiple>
        <h3 class="usa-accordion__heading">
          <button
            type="button"
            class="usa-accordion__button"
            aria-expanded="false"
            aria-controls="m-a1"
            data-tealium="accordion">
            How do I complete attestation?
          </button>
        </h3>
        <div id="m-a1" class="usa-accordion__content usa-prose">
          <p>
            After logging in to <a href="https://hpms.cms.gov/app/ng/cda/attestations" target="_blank" rel="noopener">HPMS</a> and selecting <i>Claims Data Attestation</i> under <i>Contract Management</i>:
          </p>
          <ol>
            <li>Choose single, multiple, or all contracts in the <i>Contracts Without Attestation</i> window.</li>
            <li>Select <i>Attest</i>.</li>
            <li>Agree to the <i>Claims Data Usage Protocols</i>.</li>
          </ol>
          <p>
            To add multiple Attestors, follow the same steps and select <i>Re-attest</i> during step 2.
          </p>
        </div>
        <h3 class="usa-accordion__heading">
          <button
            type="button"
            class="usa-accordion__button"
            aria-expanded="false"
            aria-controls="m-a2"
            data-tealium="accordion">
            What are the requirements to be an Attestor?
          </button>
        </h3>
        <div id="m-a2" class="usa-accordion__content usa-prose">
          <ul>
            <li>Be part of an active, stand-alone PDP organization (PACE and MA-PD are ineligible)</li>
            <li>Hold a current CEO, CFO, or COO role within the organization</li>
            <li>Attest to each contract that will connect to AB2D</li>
          </ul>
        </div>
        <h3 class="usa-accordion__heading">
          <button
            type="button"
            class="usa-accordion__button"
            aria-expanded="false"
            aria-controls="m-a3"
            data-tealium="accordion">
            How does attestation affect your claims data?
          </button>
        </h3>
        <div id="m-a3" class="usa-accordion__content usa-prose">
          <ul>
            <li>
              You can retrieve claims data for active plan enrollees from the date of attestation onwards. Claims data prior to that date will not be provided.
            </li>
            <li>
              Your organization must have an active Attestor at all times. Access to data is suspended during periods without an active Attestor and restored only when another CEO, CFO, or COO completes attestation.
            </li>
          </ul>
        </div>
        <h3 class="usa-accordion__heading">
          <button
            type="button"
            class="usa-accordion__button"
            aria-expanded="false"
            aria-controls="m-a4"
            data-tealium="accordion">
            When does attestation take effect?
          </button>
        </h3>
        <div id="m-a4" class="usa-accordion__content usa-prose">
          <p>
            After attestation, your organization must complete all remaining steps: choosing an ADOS, testing in the sandbox, and receiving production credentials. Once complete, you will have access to claims data starting from the attestation date. Claims data prior to that date will not be available.
          </p>
        </div>
      </div>
  </li>
  <li class="usa-process-list__item">
    <h2 class="usa-process-list__heading margin-bottom-2">Choose an AB2D Data Operations Specialist</h2>
    <span class="usa-tag bg-accent-cool-darker">Completed by Attestor</span>
    <p>
      After attestation, the Attestor receives an email with instructions to assign an AB2D Data Operations Specialist (ADOS). The ADOS is your organization's primary technical point of contact.
    </p>
    <p><b>ADOS requirements:</b></p>
    <ul>
      <li>Employee or vendor with authority to access and view your organization's enrollee data</li>
      <li>Technical expertise to connect to and retrieve data from the sandbox and production environments</li>
      <li>Ability to provide static IP address(es) and/or CIDR ranges for every system accessing the API</li>
    </ul>
  </li>
  <li class="usa-process-list__item">
    <h2 class="usa-process-list__heading margin-bottom-2">Retrieve sandbox data</h2>
    <span class="usa-tag">Completed by AB2D Data Ops Specialist</span>
    <h3 class="font-sans-sm">Verify sandbox data retrieval</h3>
      <p>
        The ADOS receives an email with instructions on next steps. They must send the AB2D team the job ID from a successful data export in the sandbox. Learn <a href="{{ '/get-a-bearer-token' | relative_url }}">how to get a bearer token</a> and <a href="{{ '/access-sandbox-data' | relative_url }}">access sandbox data</a>.
      </p>
    <h3 class="font-sans-sm">Provide your IP addresses</h3>
      <p>
        Your organization must provide the AB2D team with the public, static IP address(es) of every system that will use the API. These addresses are reviewed, approved, and added to the allowlist for production access.
      </p>
  </li>
  <li class="usa-process-list__item">
    <h2 class="usa-process-list__heading margin-bottom-2">Get production credentials</h2>
    <span class="usa-tag bg-accent-cool-darker">Completed by Attestor</span>
    <span>and</span>
    <span class="usa-tag">Completed by AB2D Data Ops Specialist</span>
    <p>
      The Attestor receives an email with production credentials. These credentials allow your organization to <a href="{{ '/get-a-bearer-token' | relative_url }}">get a bearer token</a> and <a href="{{ '/access-production-claims-data' | relative_url }}">access production claims data</a>.
    </p>

    {% capture phiAlert %}
    Production claims data contains Personally Identifiable Information (PII) and Protected Health Information (PHI) that must not be shared under any circumstances.
    {% endcapture %}
    {% include alert.html variant="warning" text=phiAlert classNames="measure-6" %}

    <p>
      If you have questions or need help, visit <a href="{{ '/support' | relative_url }}">Support</a> or contact the AB2D team at <a href="mailto:ab2d@cms.hhs.gov">ab2d@cms.hhs.gov</a>.
    </p>
  </li>
</ol>

## Next step

After receiving your production credentials, follow the guide to [access production claims data]({{ '/access-production-claims-data' | relative_url }}).

{% include feedback-form.html id="cbddf9b6" %}
