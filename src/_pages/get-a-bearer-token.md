---
layout: api-docs
page_title: "How to Get a Bearer Token"
seo_title: "Get a Bearer Token for Claims Data | AB2D Medicare API"
description: "Obtain a bearer token for authenticating requests to the AB2D API, ensuring secure access to enrollees' Medicare claims data."
permalink: /get-a-bearer-token
in-page-nav: true
---

# {{ page.page_title }}

AB2D uses OAuth 2.0 for authentication. You need a bearer token for every API request to the sandbox or production environment.

{% capture tokenAlert %}
Bearer tokens expire 30 minutes after they are issued. If your token expires, request a new one by repeating the authentication process.
{% endcapture %}
{% include alert.html variant="warning" text=tokenAlert classNames="measure-6" %}

## Before you begin

- Install [curl and jq]({{ '/setup-instructions' | relative_url }}) or have another HTTP client ready
- Have your credentials available:
  - **Sandbox**: Use one of the [sandbox credential sets](#sandbox-credentials) below
  - **Production**: Use the credentials provided by the AB2D team after completing [production access]({{ '/production-access' | relative_url }})

## Step 1: Encode your credentials in Base64

Combine your client ID and password in the format `clientId:password`, then encode the string in Base64.

{% capture curlSnippet %}{% raw %}
AUTH=$(echo -n "${okta_client_id}:${okta_client_password}" | base64)
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

For example, using sandbox contract Z1001:

{% capture curlSnippet %}{% raw %}
AUTH=$(echo -n "0oa9jyx2w9Z0AntLE297:hskbPu-YoWfGDY1gcQq34BfIEyMVuayu87zWDliG" | base64)
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

## Step 2: Request a bearer token

Use the Base64-encoded credentials to request a token from the identity provider.

**Sandbox** (test.idp.idm.cms.gov):

{% capture curlSnippet %}{% raw %}
RESP1=$(curl -X POST "https://test.idp.idm.cms.gov/oauth2/aus2r7y3gdaFMKBol297/v1/token?grant_type=client_credentials&scope=clientCreds" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -H "Authorization: Basic ${AUTH}")
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

**Production** (idm.cms.gov):

{% capture curlSnippet %}{% raw %}
RESP1=$(curl -X POST "https://idm.cms.gov/oauth2/aus2r7y3gdaFMKBol297/v1/token?grant_type=client_credentials&scope=clientCreds" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -H "Authorization: Basic ${AUTH}")
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

The response contains your bearer token in the `access_token` field:

{% capture curlSnippet %}{% raw %}
{
  "token_type": "Bearer",
  "expires_in": 1800,
  "access_token": "eyJraWQiOiJFc..."
}
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="json" %}

## Step 3: Extract and save the token

Extract the token from the response and save it to a variable for use in subsequent API requests:

{% capture curlSnippet %}{% raw %}
TOKEN=$(echo $RESP1 | jq -r ".access_token")
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="shell" can_copy=true %}

You can verify the token was saved by running `echo $TOKEN`.

## Sandbox credentials

Anyone can try the API using these test credentials. Sandbox credentials do not work in the production environment.

### Contract Z1001 (10,000 enrollees)

Client ID:
{% capture curlSnippet %}{% raw %}
0oa9jyx2w9Z0AntLE297
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="code" can_copy=true %}

Client password:
{% capture curlSnippet %}{% raw %}
hskbPu-YoWfGDY1gcQq34BfIEyMVuayu87zWDliG
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="code" can_copy=true %}

Base64-encoded credentials:
{% capture curlSnippet %}{% raw %}
MG9hOWp5eDJ3OVowQW50TEUyOTc6aHNrYlB1LVlvV2ZHRFkxZ2NRcTM0QmZJRXlNVnVheXU4N3pXRGxpRw=={% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="code" can_copy=true %}

### Contract Z1002 (10,000 enrollees)

Client ID:
{% capture curlSnippet %}{% raw %}
0oa9jz0e1dyNfRMm6297
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="code" can_copy=true %}

Client password:
{% capture curlSnippet %}{% raw %}
shnG6NGkHcu29ptDsKKRW6q5uFJSSpIpdl_K5fVW
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="code" can_copy=true %}

Base64-encoded credentials:
{% capture curlSnippet %}{% raw %}
MG9hOWp6MGUxZHlOZlJNbTYyOTc6c2huRzZOR2tIY3UyOXB0RHNLS1JXNnE1dUZKU1NwSXBkbF9LNWZWVw==
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet language="code" can_copy=true %}

{% capture credFileAccordionContent %}
<h5>Linux or Mac</h5>
<ol>
    <li>
        Open a terminal and set your variables:
{% capture curlSnippet2 %}{% raw %}
AUTH_FILE=/home/abcduser/credentials_Z123456_base64.txt
OKTA_CLIENT_ID=abcd
OKTA_CLIENT_PASSWORD=badpassword
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet2 language="shell" can_copy=true %}
    </li>
    <li>
        Encode and save the credentials:
{% capture curlSnippet3 %}{% raw %}
echo -n "$OKTA_CLIENT_ID:$OKTA_CLIENT_PASSWORD" | base64 > $AUTH_FILE
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet3 language="shell" can_copy=true %}
    </li>
</ol>

<h5>PowerShell</h5>
<ol>
    <li>
        Create a new file:
{% capture curlSnippet4 %}{% raw %}
$AUTH_FILE="C:\users\abcduser\credentials_Z123456_base64.txt"
New-Item -Path $AUTH_FILE -ItemType File
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet4 language="shell" can_copy=true %}
    </li>
    <li>
        Save Base64-encoded credentials to the file:
{% capture curlSnippet5 %}{% raw %}
$BASE64_ENCODED_ID_PASSWORD='{base64_encoded_credentials}'
Set-Content -Path $AUTH_FILE $BASE64_ENCODED_ID_PASSWORD
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippet5 language="shell" can_copy=true %}
    </li>
</ol>
{% endcapture %}
{% include accordion.html id="credFile" heading="Advanced: Create a credential file" expanded=false bordered=true accordionContent=credFileAccordionContent %}

## Troubleshooting

Visit the [Troubleshooting Guide]({{ '/troubleshooting-guide' | relative_url }}) for help with HTTP response codes and common issues. If you need additional assistance, email the AB2D team at [ab2d@cms.hhs.gov](mailto:ab2d@cms.hhs.gov) with:

- Your operating system
- The HTTP response code you received
- A description of the issue and which step you are on
- Any relevant logs

## Next step

Now that you have a bearer token, [access sandbox data]({{ '/access-sandbox-data' | relative_url }}) to try the export workflow.

{% include feedback-form.html id="5d9bf43f" %}
