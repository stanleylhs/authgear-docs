# Connect Apps to LinkedIn

## Prerequisite

1. Create an app in the [Linkedin Developers Portal](https://developer.linkedin.com/).
2. In the "Products" section, choose "Sign In with LinkedIn"
3. In the details page of the created app, click the "Auth" tab
4. Take notes of "Client ID" and "Client Secret", add `https://<YOUR_AUTHGEAR_ENDPOINT>/sso/oauth2/callback/linkedin` to "Redirect URLs" in "OAuth 2.0 settings" section

{% hint style="info" %}
Redirect URI has the form of `/sso/oauth2/callback/:alias`. The `alias` is used as the identifier of OAuth provider. You can configure the `alias` in Authgear Portal
{% endhint %}

## Configure Sign in with LinkedIn through the portal

In Authgear portal, go to the "Single-Sign On" page, then do the following:

1. Enable "Sign in with LinkedIn"
2. Fill in "Client ID" and "Client Secret" obtained above
3. **Save** the settings

🎉 Done! You have just added Linkedin Login to your apps!
