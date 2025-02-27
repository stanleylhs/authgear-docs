---
description: >-
  Allow guest users to use your apps and website and promote to regular users
  later.
---

# Anonymous Users

## Overview

You can use the create an Anonymous User account for the guests in your apps, so they can carry out interactions just like a normal user. For example, guests can post comments and save preference in your social platform before setting email and password. The user session will persist even the app has been closed.

This improves the app experience because the user do not need to set up authenticators until further down the user journey, while still enjoying most of the app features. For app developers, the ability to create and assign Anonymous User also makes it easier to link the activities of an individual before and after sign-up.&#x20;

## Enable Anonymous User in your application

1. In the **Portal** , go to the **Anonymous Users** page
2. Turn on the **Enable anonymous users?** toggle in the page and click **Save**

## Using the SDK

### Sign up as an Anonymous User

This will create an Anonymous User for the session. Subsequent requests from the end-user in the session can be identify by the same `sub`

{% tabs %}
{% tab title="React Native" %}
```typescript
authgear
    .authenticateAnonymously()
    .then(({userInfo}) => {
        // Logged in as anonymous user successfully
    })
    .catch((err) => {
        // Handle the error
    });
```

{% endtab %}

{% tab title="Flutter" %}
```dart
try {
    final userInfo = await authgear.authenticateAnonymously();
    // Logged in as anonymous user successfully
} catch (e) {
    // Handle the error
}
```

{% endtab %}

{% tab title="iOS" %}
```swift
authgear.authenticateAnonymously { result in
    switch result {
    case let .success(authResult):
        let userInfo = authResult.userInfo
        // Logged in as anonymous user successfully
    case let .failure(error):
        // Handle the error
    }
}
```
{% endtab %}

{% tab title="Android" %}
```java
mAuthgear.authenticateAnonymously(new OnAuthenticateAnonymouslyListener() {
    @Override
    public void onAuthenticated(@NonNull UserInfo userInfo) {
        // Logged in as anonymous user successfully
    }

    @Override
    public void onAuthenticationFailed(@NonNull Throwable throwable) {
        // Handle the error
    }
});
```

{% endtab %}

{% tab title="Web" %}
```typescript
authgear
    .authenticateAnonymously()
    .then(({userInfo}) => {
        // Logged in as anonymous user successfully
    })
    .catch((err) => {
        // Handle the error
    });
```
{% endtab %}
{% endtabs %}

### Check the UserInfo object

After "signing up" as an anonymous user, you can [retrieve the "UserInfo" object](../integrate/user-profile.md#userinfo-endpoint) and see the `sub` of the end-user.

**UserInfo**

```json
{
  "sub": "...",
  "isVerified": false,
  "isAnonymous": true
}
```

### Promotion of an Anonymous User

`promoteAnonymousUser` function can be called to promote an anonymous user to a regular user with login ID (e.g. email, phone number) and authenticators (e.g. password). The end-user will be prompted a sign up page to complete the promotion. The `sub` of an end-user will remain the same after promotion.

{% tabs %}
{% tab title="React Native" %}
```javascript
authgear
    .promoteAnonymousUser({
        redirectURI: THE_REDIRECT_URI,
    })
    .then(({userInfo}) => {
        // Promote anonymous user successfully
    })
    .catch((e) => {
        // Handle the error
    });
```
{% endtab %}

{% tab title="Flutter" %}
```dart
try {
    final userInfo = await authgear.promoteAnonymousUser(redirectURI: THE_REDIRECT_URI);
    // Promote anonymous user successfully
} catch (e) {
    // Handle the error
}
```
{% endtab %}

{% tab title="iOS" %}
```swift
authgear.promoteAnonymousUser(
    redirectURI: THE_REDIRECT_URI
) { result in
    switch result {
    case let .success(authResult):
        let userInfo = authResult.userInfo
        // Promote anonymous user successfully
    case let .failure(error):
        // Handle the error
    }
}
```
{% endtab %}

{% tab title="Android" %}
```java
PromoteOptions options = new PromoteOptions(THE_REDIRECT_URI);
authgear.promoteAnonymousUser(options, new OnPromoteAnonymousUserListener() {
    @Override
    public void onPromoted(@NonNull AuthorizeResult result) {
        // Promote anonymous user successfully
    }
    @Override
    public void onPromotionFailed(@NonNull Throwable throwable) {
        // Handle the error
    }
});
```
{% endtab %}

{% tab title="Web" %}

**Step 1: Start the promotion flow**

When the user clicks promote on your website, make a **start promotion** call to redirect them to the promotion page.

```typescript
authgear
    .startPromoteAnonymousUser({
        // Configure redirectURI which users will be redirected to
        // after they have promoted with Authgear.
        // You can use any path in your website.
        // Make sure it is in the "Redirect URIs" list of the Application.
        // The redirect uri for anonymous user promotion should be
        // different from the one for normal user authentication.
        // e.g. "https://yourdomain.com/promote-redirect"
        redirectURI: THE_REDIRECT_URI,
    })
    .then(({userInfo}) => {
        // Started the promotion flow
    })
    .catch((err) => {
        // Failed to start the promotion flow
    });
```

**Step 2: Handle the promotion result**

After the user promotes on the promotion page, the user will be redirected to the
`redirectURL` with a `code` parameter in the URL query. In the `redirectURI` of your application, make a **finish promotion** call to handle the promotion result.

```typescript
authgear
    .finishPromoteAnonymousUser()
    .then(({userInfo}) => {
        // Promoted successfully
        // You should redirect the user to another path
    })
    .catch((err) => {
        // Failed to finish promotion
    });
);
```
{% endtab %}
{% endtabs %}

## User Lifetime

### Mobile apps

On Mobile SDKs, creating an anonymous user will create a key-pair. The key-pair is stored in the native encrypted store on the mobile device. The end-user can always re-login to the same anonymous user with the key-pair. Such anonymous user will become inaccessible when the encrypted store is removed.

### Web apps and websites

On the Web SDK, there will be no key-pair created. Therefore the end-user will not be able to login to the same Anonymous User after the their session become invalid. For cookie-based authentication, it is controlled by the "idle timeout" and "session lifetime" of the **Cookie**. For token-based authentication, it is controlled by the "idle timeout" and "token lifetime" of the **Refresh Token**.

In other words, The anonymous user account lifetime is the same as the logged in session lifetime.

To adjust the lifetime settings, change the timeouts and lifetimes in **Portal** > **Applications** accordingly.

#### Caution for high traffic websites

You should create anonymous users only when necessary in the user journey to prevent creating excessive orphan accounts in your tenant.
