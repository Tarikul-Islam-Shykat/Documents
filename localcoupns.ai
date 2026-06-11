# Deep Link Hosting Handoff

Domain already live:

```text
localcoupons.ai
```

This document tells the web developer what must be hosted on the domain so the app can open deep links correctly on Android and iOS.

## Goal

When a user opens this link:

```text
https://localcoupons.ai/offer/<offerId>
```

the operating system should open the mobile app instead of only opening the website.

## What must be hosted

Two files must be published on the website:

```text
https://localcoupons.ai/.well-known/assetlinks.json
https://localcoupons.ai/.well-known/apple-app-site-association
```

Both files must:

```text
- be served over HTTPS
- return HTTP 200
- have no redirects
- be publicly reachable
```

## Android

### File path

```text
https://localcoupons.ai/.well-known/assetlinks.json
```

### Content

```json
[
  {
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
      "namespace": "android_app",
      "package_name": "com.local.coupons.app",
      "sha256_cert_fingerprints": [
        "D7:DC:DE:70:67:E0:63:D0:0C:3E:92:0E:C8:A1:FC:76:5F:D4:D5:DE:F7:F5:27:AD:65:C9:47:B8:B3:BE:EE:56"
      ]
    }
  }
]
```

### Notes for Android

```text
- package_name must stay com.local.coupons.app
- use the SHA-256 fingerprint of the release signing certificate
- if the app is distributed through Google Play with Play App Signing, the Play App Signing certificate fingerprint may be different
- the file name must be exactly assetlinks.json
```

## iOS

### File path

```text
https://localcoupons.ai/.well-known/apple-app-site-association
```

### Content

```json
{
  "applinks": {
    "apps": [],
    "details": [
      {
        "appID": "XXXWV8TPSR.com.local.coupons.app",
        "paths": [ "/offer/*" ]
      }
    ]
  }
}
```

### Notes for iOS

```text
- the file name must be exactly apple-app-site-association
- do not add a .json extension
- serve it with HTTPS
- do not redirect it
- the app identifier is TEAM_ID.bundle_id
- for this app, the bundle id is com.local.coupons.app
- the team id currently used in the project is XXXWV8TPSR
```

## iOS app-side setup

The iOS app also needs the Associated Domains capability added in Xcode:

```text
applinks:localcoupons.ai
```

That is not a website file. It is an app entitlement/capability.

## What the web developer should do

1. Create the `.well-known` folder on `localcoupons.ai` if it does not already exist.
2. Host `assetlinks.json` at `/.well-known/assetlinks.json` with the Android content above.
3. Host `apple-app-site-association` at `/.well-known/apple-app-site-association` with the iOS content above.
4. Make sure both URLs return `200 OK` with no redirect.
5. Make sure the server does not force a file extension onto the iOS association file.
6. Confirm the Android fingerprint matches the actual release signing certificate that will ship to users.

## What the mobile app already has

The app already contains:

```text
- Android intent filters for https://localcoupons.ai/offer/*
- iOS custom URL schemes for builtbyclint:// and localcoupons://
- deep link parsing and routing code
```

So the missing piece is the hosted association files plus the iOS Associated Domains capability.

## Testing

### Android

After hosting `assetlinks.json`, test a real HTTPS link such as:

```text
https://localcoupons.ai/offer/6a0c7d56855a828622040c2c
```

### iOS

After adding Associated Domains and hosting `apple-app-site-association`, test the same HTTPS link on a real device.

For simulator testing before universal links are fully configured, use the custom scheme:

```text
localcoupons://offers/6a0c7d56855a828622040c2c
```

