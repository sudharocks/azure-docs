---
title: Release notes - Map Control
titleSuffix: Microsoft Azure Maps
description: Release notes for the Azure Maps Web SDK. 
author: stevemunk
ms.author: v-munksteve
ms.date: 1/31/2023
ms.topic: reference
ms.service: azure-maps
services: azure-maps
---

# Web SDK map control release notes

This document contains information about new features and other changes to the Map Control.

## v3 (preview)

### [3.0.0-preview.3] (February 2, 2023)

#### Installation (3.0.0-preview.3)

The preview is available on [npm][3.0.0-preview.3] and CDN.

- **NPM:** Refer to the instructions at [azure-maps-control@3.0.0-preview.3][3.0.0-preview.3]

- **CDN:** Reference the following CSS and JavaScript in the `<head>` element of an HTML file:

    ```html
    <link href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/3.0.0-preview.3/atlas.min.css" rel="stylesheet" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/3.0.0-preview.3/atlas.min.js"></script>
    ```

#### New features (3.0.0-preview.3)

- **\[BREAKING\]** Migrated from [adal-angular] to [@azure/msal-browser] used for authentication with Microsoft Azure Active Directory ([Azure AD]).
  Changes that may be required:
  - `Platform / Reply URL` Type must be set to `Single-page application` on Azure AD App Registration portal.
  - Code change is required if a custom `authOptions.authContext` is used.
  - For more information, see [How to migrate a JavaScript app from ADAL.js to MSAL.js][migration guide].

- Allow pitch and bearing being set with [CameraBoundsOptions] in [Map.setCamera(options)].

#### Bug fixes (3.0.0-preview.3)

- Fixed issue in [language mapping], now `zh-Hant-TW` will no longer revert back to `en-US`.

- Fixed the inability to switch between [user regions (view)].

- Fixed exception that occurred when style switching while progressive layer loading is in progress.

- Fixed the accessibility information retrieval from map tile label layers.

- Fixed the occasional issue where vector tiles aren't being rerendered after images are being added via [ImageSpriteManager.add()].

### [3.0.0-preview.2] (December 16, 2022)

#### Installation (3.0.0-preview.2)

The preview is available on [npm][3.0.0-preview.2] and CDN.

- **NPM:** Refer to the instructions at [azure-maps-control@3.0.0-preview.2][3.0.0-preview.2]

- **CDN:** Reference the following CSS and JavaScript in the `<head>` element of an HTML file:

  ```html
  <link href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/3.0.0-preview.2/atlas.min.css" rel="stylesheet" />
  <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/3.0.0-preview.2/atlas.min.js"></script>
    ```

#### New features (3.0.0-preview.2)

Add `progressiveLoading` and `progressiveLoadingInitialLayerGroups` to [StyleOptions] to enable the capability of loading map layers progressively. This feature improves the perceived loading time of the map. For more information, see [2.2.2 release notes](#222-december-15-2022).

#### Bug fixes (3.0.0-preview.2)

- Fixed an issue that the ordering of user layers wasn't preserved after calling `map.layers.move()`.

- Fixed the inability to disable traffic incidents in [TrafficControlOptions] when `new atlas.control.TrafficControl({incidents: false})` is used.

- Add `.atlas-map` to all css selectors to scope the styles within the map container. The fix prevents the css from accidentally adding unwanted styles to other elements on the page.

### [3.0.0-preview.1] (November 18, 2022)

### Installation (3.0.0-preview.1)

The preview is available on [npm][3.0.0-preview.1].

- Install [azure-maps-control@next][azure-maps-control] to your dependencies:

    ```shell
    npm i azure-maps-control@next
    ```

#### New features (3.0.0-preview.1)

This update is the first preview of the upcoming 3.0.0 release. The underlying [maplibre-gl] dependency has been upgraded from `1.14` to `3.0.0-pre.1`, offering improvements in stability and performance.

#### Bug fixes (3.0.0-preview.1)

- Fixed a regression issue that prevents IndoorManager from removing a tileset:

  ```js
  indoorManager.setOptions({
      tilesetId: undefined
  })
  ```

## v2 (latest)

### [2.2.3]

#### New features (2.2.3)

- Allow pitch and bearing being set with [CameraBoundsOptions] in [Map.setCamera(options)].

#### Bug fixes (2.2.3)

- Fixed issue in [language mapping], now `zh-Hant-TW` will no longer revert back to `en-US`.

- Fixed the inability to switch between [user regions (view)].

- Fixed exception that occurred when style switching while progressive layer loading is in progress.

- Fixed the accessibility information retrieval from map tile label layers.

- Fixed the occasional issue where vector tiles aren't being rerendered after images are being added via [ImageSpriteManager.add()].

### [2.2.2] (December 15, 2022)

#### New features (2.2.2)

Add `progressiveLoading` and `progressiveLoadingInitialLayerGroups` to [StyleOptions] to enable the capability of loading map layers progressively. This feature improves the perceived loading time of the map.

- `progressiveLoading`
  - Enables progressive loading of map layers.
  - Defaults to `false`.
- `progressiveLoadingInitialLayerGroups`
  - Specifies the layer groups to load first.
  - Defaults to `["base"]`.
  - Possible values are `base`, `transit`, `labels`, `buildings`, and `labels_places`.
  - Other layer groups are deferred such that the initial layer groups can be loaded first.

#### Bug fixes (2.2.2)

- Fixed an issue that the ordering of user layers wasn't preserved after calling `map.layers.move()`.

- Fixed the inability to disable traffic incidents in [TrafficControlOptions] when `new atlas.control.TrafficControl({incidents: false})` is used.

## Next steps

Explore samples showcasing Azure Maps:

> [!div class="nextstepaction"]
> [Azure Maps Samples]

Stay up to date on Azure Maps:

> [!div class="nextstepaction"]
> [Azure Maps Blog]

[3.0.0-preview.3]: https://www.npmjs.com/package/azure-maps-control/v/3.0.0-preview.3
[3.0.0-preview.2]: https://www.npmjs.com/package/azure-maps-control/v/3.0.0-preview.2
[3.0.0-preview.1]: https://www.npmjs.com/package/azure-maps-control/v/3.0.0-preview.1
[2.2.3]: https://www.npmjs.com/package/azure-maps-control/v/2.2.3
[2.2.2]: https://www.npmjs.com/package/azure-maps-control/v/2.2.2
[Azure AD]: /azure/active-directory/develop/v2-overview
[adal-angular]: https://github.com/AzureAD/azure-activedirectory-library-for-js
[@azure/msal-browser]: https://github.com/AzureAD/microsoft-authentication-library-for-js
[migration guide]: /azure/active-directory/develop/msal-compare-msal-js-and-adal-js
[CameraBoundsOptions]: /javascript/api/azure-maps-control/atlas.cameraboundsoptions?view=azure-maps-typescript-latest
[Map.setCamera(options)]: /javascript/api/azure-maps-control/atlas.map?view=azure-maps-typescript-latest#azure-maps-control-atlas-map-setcamera
[language mapping]: https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/azure-maps/supported-languages.md#azure-maps-supported-languages
[user regions (view)]: /javascript/api/azure-maps-control/atlas.styleoptions?view=azure-maps-typescript-latest#azure-maps-control-atlas-styleoptions-view
[ImageSpriteManager.add()]: /javascript/api/azure-maps-control/atlas.imagespritemanager?view=azure-maps-typescript-latest#azure-maps-control-atlas-imagespritemanager-add
[azure-maps-control]: https://www.npmjs.com/package/azure-maps-control
[maplibre-gl]: https://www.npmjs.com/package/maplibre-gl
[StyleOptions]: /javascript/api/azure-maps-control/atlas.styleoptions
[TrafficControlOptions]: /javascript/api/azure-maps-control/atlas.trafficcontroloptions
[Azure Maps Samples]: https://samples.azuremaps.com
[Azure Maps Blog]: https://techcommunity.microsoft.com/t5/azure-maps-blog/bg-p/AzureMapsBlog
