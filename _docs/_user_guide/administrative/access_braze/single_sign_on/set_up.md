---
nav_title: SAML SSO Set Up
page_order: 0
---

# Service Provider (SP) Initiated Login

## Requirements

Upon setup, you will be asked to provide a Sign-On URL and an Assertion Consumer Service (ACS) URL.  

| Requirement | Details |
|---|---|
| **Sign-On URL** | `https://<SUBDOMAIN>.braze.com/sign_in` <br><br> For the subdomain, use the coordinating subdomain listed in [your Braze instance URL]({{site.baseurl}}/user_guide/administrative/access_braze/braze_instances/). For example, if your instance is `US-01`, your URL is `https://dashboard-01.braze.com`. This means that your subdomain will be `dashboard-01`. |
| **Assertion Consumer Service (ACS) URL** | `https://<SUBDOMAIN>/auth/saml/callback` <br><br> *For some IdPs, this can also be referred to as the Reply URL, Audience URL, or Audience URI.* |
| **Entity ID** | `braze_dashboard` |
{: .reset-td-br-1 .reset-td-br-2}

## Configure Your Identity Provider

First, you must set up Braze as a Service Provider (SP) in your Identity Provider (IdP) with the information below.

In addition, you’ll need to set up SAML attribute mapping.

| SAML Attribute | Required? | Accepted SAML Attributes |
|---|---|---|
|`email` | Required | `email` <br> `mail` <br> `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/email` |
| `first_name` | Optional | `first_name` <br> `firstname` <br> `firstName`<br>`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/first_name` |
| `last_name` | Optional | `last_name` <br> `lastname` <br> `lastName` <br>`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/last_name` |

{% alert note %}
Braze only requires `email` in the SAML Assertion.
{% endalert %}

## Configure Braze

Once you have set up Braze within your IdP, they will provide a Target URL and `x.509` certificate which you will input into your Braze account.

After your Account Manager has enabled SAML SSO for your account, go to `Company Settings` > `Security Settings` and toggle the SAML SSO section to `ON`.

On this page, you, input:

| Requirement | Details |
|---|---|
| `SAML Name` | This will appear as the button text on the login screen.<br>This is typically your IdP name, like “Okta.” |
| `Target URL` | This is provided after setting up Braze within your IdP.<br> Some IdPs reference this as the SSO URL or SAML 2.0 Endpoint. |
| `Certificate` | The `x.509` certificate is provided by your IdP.|
{: .reset-td-br-1 .reset-td-br-2}

Please make sure that your certificate follows this format when adding it to the dashboard:
```
-----BEGIN CERTIFICATE-----
<certificate>
-----END CERTIFICATE-----
```

![Enable SAML SSO]({% image_buster /assets/img/samlsso.gif %})

When you save your Security Settings and log out, you should now be able to sign in with your IdP.

![Login Page with SSO]({% image_buster /assets/img/sso1.png %}){: style="max-width:40%;"}

## Create and Enable a Braze API Key for IdP Login (Optional)

To enable IdP initiated login, you will first need to create an API Key in `Developer Console` > `API Settings`.

![SSO Set Up]({% image_buster /assets/img/sso2.png %})

Input the generated API Key as the `RelayState` parameter within your IdP, which will be used to identify which company the user is trying to log into.

{% alert tip %}
If you want your Braze account users to only sign in with SAML SSO, you can [restrict single sign-on authentication]({{site.baseurl}}/user_guide/administrative/access_braze/single_sign_on/restriction/) from the `Company Settings` page.
{% endalert %}
