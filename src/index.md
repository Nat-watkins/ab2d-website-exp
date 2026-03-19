---
layout: base
page_title: "AB2D API"
seo_title: "Get Enrollees' Medicare Parts A & B Claims Data | AB2D API"
description: "Access enrollees' Medicare Parts A & B claims data through the AB2D API to optimize Medication Therapy Management programs."
---

<main id="main-content">
<div class="usa-section--dark bg-primary bg-primary-gradient">
  <div class="grid-container padding-y-8">
    <h1 class="hero-title font-body-3xl measure-1 margin-bottom-0 line-height-sans-2 text-semibold text-balance">
      Access Medicare Claims Data for Better Medication Therapy Management
    </h1>
    <p class="hero-paragraph line-height-sans-4 measure-5">
      The AB2D API provides Prescription Drug Plan (PDP) sponsors with Medicare Parts A and B claims data. Use enrollee diagnosis codes, service dates, and provider information to improve MTM programs, coordinate care, and detect fraud.
    </p>
    <div class="grid-row grid-gap margin-top-2">
      <div class="tablet:grid-col-auto margin-top-2">
        <a href="{{ '/api-documentation' | relative_url }}" class="hero-button usa-button usa-button--inverse usa-button--outline width-full">
          Get started
          {% include sprite.html icon="arrow_forward" size="3" %}
        </a>
      </div>
      <div class="tablet:grid-col-auto margin-top-2">
        <a href="https://github.com/CMSgov/ab2d" class="hero-button usa-button usa-button--inverse usa-button--outline width-full">
          Code repo
          {% include sprite.html icon="github" size="3" %}
        </a>
      </div>
    </div>
  </div>
</div>

  <div class="minw-15 padding-y-6 padding-x-3 grid-container">
    <div>
      <h2>How to get started</h2>
      <p>Follow these steps to access Medicare claims data through the AB2D API.</p>
      <ol class="usa-process-list margin-top-1">
        <li class="usa-process-list__item">
          <p class="usa-process-list__heading">Learn about the data</p>
          <p>
            Review the <a href="{{ '/ab2d-data' | relative_url }}">available data</a>, <a href="{{ '/ab2d-data#data-dictionary' | relative_url }}">Data Dictionary</a>, and <a href="{{ '/ab2d-data#sample-files' | relative_url }}">sample files</a> to understand what AB2D provides.
          </p>
        </li>
        <li class="usa-process-list__item">
          <p class="usa-process-list__heading">Try the sandbox</p>
          <p>
            The sandbox is open to everyone. <a href="{{ '/get-a-bearer-token' | relative_url }}">Get a bearer token</a> and <a href="{{ '/access-sandbox-data' | relative_url }}">download test claims data</a> to see how the API works.
          </p>
        </li>
        <li class="usa-process-list__item">
          <p class="usa-process-list__heading">Get production access</p>
          <p>
            Active, stand-alone PDP sponsors can <a href="{{ '/production-access' | relative_url }}">request production credentials</a> to access their enrollees' real claims data.
          </p>
        </li>
      </ol>
    </div>

    <div class="padding-top-4">
      <h2>Use cases for the AB2D API</h2>
      <div class="usa-graphic-list__row grid-row tablet:grid-gap-6 padding-y-2">
        <div class="tablet:grid-col-6 padding-y-3">
          {% include sprite.html icon="people" size=8 %}
          <h3 class="margin-y-1">Target MTM program enrollees</h3>
          <p>
            Identify enrollees who qualify for Medication Therapy Management by analyzing diagnosis codes and medication histories across Parts A and B claims.
          </p>
        </div>
        <div class="tablet:grid-col-6 padding-y-3">
          {% include sprite.html icon="local_pharmacy" size=8 %}
          <h3 class="margin-y-1">Enhance MTM consultations</h3>
          <p>
            Give pharmacists and care managers a complete picture of enrollee medical histories for more informed, effective consultations.
          </p>
        </div>
        <div class="tablet:grid-col-6 padding-y-3">
          {% include sprite.html icon="insights" size=8 %}
          <h3 class="margin-y-1">Boost health outcomes</h3>
          <p>
            Improve medication adherence and reduce adverse drug events by identifying gaps in care and potential drug interactions.
          </p>
        </div>
        <div class="tablet:grid-col-6 padding-y-3">
          {% include sprite.html icon="security" size=7 %}
          <h3 class="margin-y-1">Prevent fraud, waste, and abuse</h3>
          <p>
            Detect suspicious billing patterns and provider activity through access to comprehensive claims data.
          </p>
        </div>
      </div>
      <p>
        <a href="{{ '/use-cases' | relative_url }}" class="usa-button usa-button--unstyled">Learn more about use cases {% include sprite.html icon="arrow_forward" %}</a>
      </p>
    </div>

    <div class="grid-row grid-gap-4 desktop:grid-gap-6 padding-y-10 flex-align-center">
      <div class="tablet:grid-col">
        <img src="{{ '/assets/img/data-analysis.svg' | relative_url }}" alt="" />
      </div>
      <div class="tablet:grid-col padding-top-2">
        <h2>About the data</h2>
        <p>AB2D delivers claims data in <a href="https://hl7.org/fhir/R4/index.html" target="_blank" rel="noopener">FHIR</a>-compliant NDJSON format, including:</p>
        <ul>
          <li>Enrollee identifiers (Medicare Beneficiary Identifier)</li>
          <li>Diagnosis and procedure codes</li>
          <li>Dates, times, and places of service</li>
          <li>Provider National Provider Identifiers (NPIs)</li>
        </ul>
        <p><a href="{{ '/ab2d-data' | relative_url }}" class="usa-button usa-button--unstyled">Learn about the data {% include sprite.html icon="arrow_forward" %}</a></p>
      </div>
    </div>

    <div>
      <h2>Permitted uses of the data</h2>
      <div class="grid-row grid-gap padding-y-2">
        <div class="tablet:grid-col">
          <p class="margin-bottom-2 text-bold">
            The <a href="https://www.federalregister.gov/documents/2019/04/16/2019-06822/medicare-and-medicaid-programs-policy-and-technical-changes-to-the-medicare-advantage-medicare#page-15745" target="_blank" rel="noopener">final rule</a> permits using the data for:
          </p>
          <ul class="usa-icon-list">
            <li class="usa-icon-list__item">
              <div class="usa-icon-list__icon">
                {% include sprite.html icon="check_circle" class="text-green" %}
              </div>
              <div class="usa-icon-list__content">
                Optimizing therapeutic outcomes through improved medication use
              </div>
            </li>
            <li class="usa-icon-list__item">
              <div class="usa-icon-list__icon">
                {% include sprite.html icon="check_circle" class="text-green" %}
              </div>
              <div class="usa-icon-list__content">
                Improving care coordination
              </div>
            </li>
            <li class="usa-icon-list__item">
              <div class="usa-icon-list__icon">
                {% include sprite.html icon="check_circle" class="text-green" %}
              </div>
              <div class="usa-icon-list__content">
                Fraud and abuse detection or compliance activities
              </div>
            </li>
          </ul>
        </div>
        <div class="tablet:grid-col">
          <p class="margin-bottom-2 text-bold">The following uses are not permitted:</p>
          <ul class="usa-icon-list">
            <li class="usa-icon-list__item">
              <div class="usa-icon-list__icon">
                {% include sprite.html icon="cancel" class="text-red" %}
              </div>
              <div class="usa-icon-list__content">
                Informing coverage determinations under Part D
              </div>
            </li>
            <li class="usa-icon-list__item">
              <div class="usa-icon-list__icon">
                {% include sprite.html icon="cancel" class="text-red" %}
              </div>
              <div class="usa-icon-list__content">
                Conducting retroactive reviews of medically accepted indication determinations
              </div>
            </li>
            <li class="usa-icon-list__item">
              <div class="usa-icon-list__icon">
                {% include sprite.html icon="cancel" class="text-red" %}
              </div>
              <div class="usa-icon-list__content">
                Facilitating enrollment changes to a different prescription drug plan or MA-PD plan offered by the same parent organization
              </div>
            </li>
            <li class="usa-icon-list__item">
              <div class="usa-icon-list__icon">
                {% include sprite.html icon="cancel" class="text-red" %}
              </div>
              <div class="usa-icon-list__content">
                Informing marketing of benefits
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div>
        <p>
          <a href="{{ '/about' | relative_url }}" class="usa-button usa-button--unstyled">Learn more about AB2D {% include sprite.html icon="arrow_forward" %}</a>
        </p>
      </div>
    </div>
  </div>
</main>
