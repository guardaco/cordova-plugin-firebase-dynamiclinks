# Cordova plugin for [Firebase Dynamic Links](https://firebase.google.com/docs/dynamic-links/)

## Index

<!-- MarkdownTOC levels="2" autolink="true" -->

- [About this fork](#about-this-fork)
- [Supported Platforms](#supported-platforms)
- [Installation](#installation)
- [Quirks](#quirks)
- [Methods](#methods)
- [Dynamic link parameters](#dynamic-link-parameters)

<!-- /MarkdownTOC -->

## About this fork

[![npm version](https://badge.fury.io/js/mslobodan_cordova-plugin-firebase-dynamiclinks.svg)](https://www.npmjs.com/package/mslobodan_cordova-plugin-firebase-dynamiclinks)

This fork has updated firebase versions on iOS and Android platform to matches versions with [mslobodan_phonegap-plugin-push](https://www.npmjs.com/package/mslobodan_phonegap-plugin-push).

On iOS it uses pods:
```
Firebase/Core: 6.24.0
Firebase/DynamicLinks: 6.24.0
```

On Android platform it uses dependencies: 
```

com.google.firebase:firebase-core: 16.0.7
com.google.firebase:firebase-dynamic-links: 17.0.0
``` 

## Supported Platforms

- iOS
- Android
 
## Installation

    $ cordova plugin add https://github.com/guardaco/cordova-plugin-firebase-dynamiclinks --variable APP_DOMAIN="guarda.co" --variable PAGE_LINK_DOMAIN="multiwallet.page.link"

Use variable `APP_DOMAIN` specify web URL where your app will start an activity to handle the link.

Use variable `PAGE_LINK_DOMAIN` specify your `*.page.link` domain.

Use variable `FIREBASE_DYNAMIC_LINKS_VERSION` and `FIREBASE_CORE_VERSION` to override dependency version on Android.

On Android you have to test whether you react properly to URI in your app just specify app package and Activity:

    $ adb shell am start -W -a android.intent.action.VIEW -d "https://multiwallet.page.link/iGuj" com.crypto.multiwallet/.MainActivity

Also you need to install the app first in order to register the deeplink

Firebase Dynamic Links SDK has an [unresolved bug](https://github.com/firebase/firebase-ios-sdk/issues/233) related to parsing `deepLink` for new app installs. In order to get it work your dynamic link MUST have an [app preview page](https://firebase.google.com/docs/dynamic-links/link-previews), which by default.

## Methods

### onDynamicLink(_callback_)
Registers callback that is triggered on each dynamic link click.
```js
cordova.plugins.firebase.dynamiclinks.onDynamicLink(function(data) {
    console.log("Dynamic link click with data:", data);
});
```
Every `create*` method accepts `dynamicLinkInfo` object as the first argument. Read section below to understand all supported [dynamic link parameters](#dynamic-link-parameters).

### createDynamicLink(_parameters_)
Creates a Dynamic Link from the parameters. Returns a promise fulfilled with the new dynamic link url.
```js
cordova.plugins.firebase.dynamiclinks.createDynamicLink({
    link: "https://google.com"
}).then(function(url) {
    console.log("Dynamic link was created:", url);
});
```

### createShortDynamicLink(_parameters_)
Creates a shortened Dynamic Link from the parameters. Shorten the path to a string that is only as long as needed to be unique, with a minimum length of 4 characters. Use this method if sensitive information would not be exposed if a short Dynamic Link URL were guessed.
```js
cordova.plugins.firebase.dynamiclinks.createShortDynamicLink({
    link: "https://google.com"
}).then(function(url) {
    console.log("Dynamic link was created:", url);
});
```

### createUnguessableDynamicLink(_parameters_)
Creates a Dynamic Link from the parameters. Shorten the path to an unguessable string. Such strings are created by base62-encoding randomly generated 96-bit numbers, and consist of 17 alphanumeric characters. Use unguessable strings to prevent your Dynamic Links from being crawled, which can potentially expose sensitive information.
```js
cordova.plugins.firebase.dynamiclinks.createUnguessableDynamicLink({
    link: "https://google.com"
}).then(function(url) {
    console.log("Dynamic link was created:", url);
});
```

## Dynamic link parameters
Any create method supports all options below to customize a returned dynamic link. Parameter names has the same meaning as in the [Firebase Dynamic Links Short Links API Reference](https://firebase.google.com/docs/reference/dynamic-links/link-shortener#parameters):
```json
{
  "domainUriPrefix": string,
  "link": string,
  "androidInfo": {
    "androidPackageName": string,
    "androidFallbackLink": string,
    "androidMinPackageVersionCode": number
  },
  "iosInfo": {
    "iosBundleId": string,
    "iosFallbackLink": string,
    "iosIpadFallbackLink": string,
    "iosIpadBundleId": string,
    "iosAppStoreId": string
  },
  "navigationInfo": {
    "enableForcedRedirect": boolean,
  },
  "analyticsInfo": {
    "googlePlayAnalytics": {
      "utmSource": string,
      "utmMedium": string,
      "utmCampaign": string,
      "utmTerm": string,
      "utmContent": string
    },
    "itunesConnectAnalytics": {
      "at": string,
      "ct": string,
      "pt": string
    }
  },
  "socialMetaTagInfo": {
    "socialTitle": string,
    "socialDescription": string,
    "socialImageLink": string
  }
}
```
