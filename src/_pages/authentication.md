---
layout: api-docs
page_title: "Authentication"
seo_title: "Authentication & Bearer Tokens | AB2D Medicare API"
description: "Set up your environment and obtain a bearer token for authenticating requests to the AB2D API."
permalink: /authentication
redirect_from:
  - /get-a-bearer-token
  - /setup-instructions
in-page-nav: true
---

# {{ page.page_title }}

AB2D uses OAuth 2.0 for authentication. You need a bearer token for every API request to the sandbox or production environment.

{% capture tokenAlert %}
Bearer tokens expire 30 minutes after they are issued. If your token expires, request a new one by repeating the authentication process.
{% endcapture %}
{% include alert.html variant="warning" text=tokenAlert classNames="measure-6" %}

## Prerequisites

You can use a variety of tools to access the AB2D API, including [curl](https://curl.se/) and [jq](https://jqlang.github.io/jq/). curl comes preinstalled on some operating systems. In a terminal, type `curl --version` to check if it's installed.

Have your credentials available:
- **Sandbox**: Use one of the [sandbox credential sets](#sandbox-credentials) below
- **Production**: Use the credentials provided by the AB2D team after completing [production access]({{ '/production-access' | relative_url }})

{% capture macSetupContent %}
<ol>
    <li>
        Install or update jq using <a href="https://brew.sh/" target="_blank" rel="noopener">Homebrew</a>:
{% capture setupSnippet %}{% raw %}
brew install jq
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=setupSnippet language="shell" can_copy=true %}
        <ul>
            <li>
        If you have already installed jq, you may receive and ignore the following warning:
{% capture setupSnippet2 %}{% raw %}
Warning: jq {version} is already installed and up-to-date
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=setupSnippet2 language="shell" %}
            </li>
        </ul>
    </li>
    <li>
        You can verify if jq is installed if you enter the following and receive a version number in response. Any version of jq will work for this set up.
{% capture setupSnippet3 %}{% raw %}
jq --version
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=setupSnippet3 language="shell" can_copy=true %}
    </li>
</ol>
{% endcapture %}

{% capture linuxSetupContent %}
<p>The following instructions are intended for systems using yum as the package installer. In the case of other Linux distributions with alternative package managers, please use the equivalent package manager command(s) to install jq.</p>
<ol>
    <li>
        Install jq:
{% capture setupSnippet4 %}{% raw %}
sudo yum install -y jq
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=setupSnippet4 language="shell" can_copy=true %}
    </li>
    <li>
        You can verify if jq is installed if you enter the following and receive a version number in response. Any version of jq will work for this set up.
{% capture setupSnippet5 %}{% raw %}
jq --version
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=setupSnippet5 language="shell" can_copy=true %}
    </li>
</ol>
{% endcapture %}

{% capture windowsSetupContent %}
<p>In this example, we will be using the Linux Subsystem for Windows 10.</p>
<ol>
    <li>Select <i>Type here to search</i> near the bottom left of your Windows desktop.</li>
    <li>Enter <code>windows features</code> in the text box.</li>
    <li>Select <i>Turn Windows features on or off</i> from the leftmost panel.</li>
    <li>Scroll down to and check <i>Windows Subsystem for Linux</i>. If it is already installed, skip to step 10 to install Ubuntu.</li>
    <li>Select <i>OK</i> on the <i>Windows Features</i> window.</li>
    <li>Wait for the changes to complete. If installation is successful, skip to step 9.</li>
    <li>If the installation is unsuccessful, open the PowerShell or Windows Command Prompt in administrator mode by right-clicking and selecting <i>Run as administrator</i>.</li>
    <li>Enter <code>wsl -- install</code> in your terminal.</li>
    <li>When prompted, restart your system.</li>
    <li>Select <i>Type here to search</i> again.</li>
    <li>Enter <code>microsoft store</code> in the text box.</li>
    <li>Select <i>Microsoft Store</i> from the leftmost panel.</li>
    <li>Select <i>Search</i> on the <i>Microsoft Store</i> page.</li>
    <li>Enter <code>linux</code> in the text box.</li>
    <li>Select <i>Run Linux on Windows</i>.</li>
    <li>Select <i>Ubuntu</i>.</li>
    <li>Select <i>Get</i>.</li>
    <li>Select <i>No, thanks</i> on the <i>Use across your devices</i> dialog. The installation will begin automatically.</li>
    <li>Once the installation is complete, select <i>Launch</i>.</li>
    <li>When prompted, choose a username and password.</li>
    <li>You'll be entering commands at the dollar sign prompt ($). The easiest way to do this is to copy and paste. Update the Ubuntu system by entering the command:
{% capture setupSnippet6 %}{% raw %}
sudo apt-get update -y
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=setupSnippet6 language="shell" can_copy=true %}
    </li>
    <li>Install jq by entering this command at the dollar sign prompt:
{% capture setupSnippet7 %}{% raw %}
sudo apt-get install -y jq
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=setupSnippet7 language="shell" can_copy=true %}
    </li>
    <li>When prompted, enter your password.</li>
    <li>Wait for the installation to complete.</li>
    <li>You can verify if jq is installed if you enter the following and receive a version number in response. Any version of jq will work for this set up.
{% capture setupSnippet8 %}{% raw %}
jq --version
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=setupSnippet8 language="shell" can_copy=true %}
    </li>
</ol>
{% endcapture %}

{% include accordion.html id="setupMac" heading="Install curl and jq on Mac" expanded=false bordered=true accordionContent=macSetupContent %}

{% include accordion.html id="setupLinux" heading="Install curl and jq on Linux (CentOS/RedHat)" expanded=false bordered=true accordionContent=linuxSetupContent %}

{% include accordion.html id="setupWindows" heading="Install curl and jq on Windows 10 and 11" expanded=false bordered=true accordionContent=windowsSetupContent %}

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
{% capture curlSnippetCred1 %}{% raw %}
AUTH_FILE=/home/abcduser/credentials_Z123456_base64.txt
OKTA_CLIENT_ID=abcd
OKTA_CLIENT_PASSWORD=badpassword
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippetCred1 language="shell" can_copy=true %}
    </li>
    <li>
        Encode and save the credentials:
{% capture curlSnippetCred2 %}{% raw %}
echo -n "$OKTA_CLIENT_ID:$OKTA_CLIENT_PASSWORD" | base64 > $AUTH_FILE
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippetCred2 language="shell" can_copy=true %}
    </li>
</ol>

<h5>PowerShell</h5>
<ol>
    <li>
        Create a new file:
{% capture curlSnippetCred3 %}{% raw %}
$AUTH_FILE="C:\users\abcduser\credentials_Z123456_base64.txt"
New-Item -Path $AUTH_FILE -ItemType File
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippetCred3 language="shell" can_copy=true %}
    </li>
    <li>
        Save Base64-encoded credentials to the file:
{% capture curlSnippetCred4 %}{% raw %}
$BASE64_ENCODED_ID_PASSWORD='{base64_encoded_credentials}'
Set-Content -Path $AUTH_FILE $BASE64_ENCODED_ID_PASSWORD
{% endraw %}{% endcapture %}
{% include copy_snippet.html code=curlSnippetCred4 language="shell" can_copy=true %}
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

Now that you have a bearer token, try the [Sandbox Guide]({{ '/sandbox-guide' | relative_url }}) to test the export workflow.

{% include feedback-form.html id="5d9bf43f" %}
