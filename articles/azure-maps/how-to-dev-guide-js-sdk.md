---
title: How to create Azure Maps applications using the JavaScript REST SDK (preview)
titleSuffix: Azure Maps
description: How to develop applications that incorporate Azure Maps using the JavaScript SDK Developers Guide.
author: stevemunk
ms.author: v-munksteve
ms.date: 11/15/2021
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
---

# JavaScript/TypeScript REST SDK Developers Guide (preview)

The Azure Maps JavaScript/TypeScript REST SDK (JavaScript SDK) supports searching using the [Azure Maps search Rest API][search], like searching for an address, fuzzy searching for a point of interest (POI), and searching by coordinates. This article will help you get started building location-aware applications that incorporate the power of Azure Maps.

> [!NOTE]
> Azure Maps JavaScript SDK supports the LTS version of Node.js. For more information, see [Node.js Release Working Group][Node.js Release].

## Prerequisites

- [Azure Maps account][Azure Maps account].
- [Subscription key][Subscription key] or other form of [authentication][authentication].
- [Node.js][Node.js].

> [!TIP]
> You can create an Azure Maps account programmatically, Here's an example using the Azure CLI:
>
> ```azurecli
> az maps account create --kind "Gen2" --account-name "myMapAccountName" --resource-group "<resource group>" --sku "G2"
> ```

## Create a Node.js project

The example below creates a new directory then a Node.js program named _mapsDemo_ using npm:

```powershell
mkdir mapsDemo
cd mapsDemo
npm init
```

## Install the search package

To use Azure Maps JavaScript SDK, you'll need to install the search package. Each of the Azure Maps services including search, routing, rendering and geolocation are each in their own package.

```powershell
npm install @azure/maps-search
```

Once the package is installed, create a `search.js` file in the `mapsDemo` directory:

```text
mapsDemo
+-- package.json
+-- package-lock.json
+-- node_modules/
+-- search.js
```

### Azure Maps services

| Service Name  | npm packages            |  Samples     |
|---------------|-------------------------|--------------|
| [Search][search readme] | [@azure-rest/maps-search][search package] | [search samples][search sample] |
| [Route][js route readme] | [@azure-rest/maps-route][js route package] | [route samples][js route sample] |
| [Render][js render readme] | [@azure-rest/maps-render][js render package]|[render sample][js render sample] |
| [Geolocation][js geolocation readme]|[@azure-rest/maps-geolocation][js geolocation package]|[geolocation sample][js geolocation sample] |

## Create and authenticate a MapsSearchClient

You'll need a `credential` object for authentication when creating the `MapsSearchClient` object used to access the Azure Maps search APIs. You can use either an Azure Active Directory (Azure AD) credential or an Azure subscription key to authenticate. For more information on authentication, see [Authentication with Azure Maps][authentication].

> [!TIP]
> The`MapsSearchClient` is the primary interface for developers using the Azure Maps search library. See [Azure Maps Search client library][JS-SDK] to learn more about the search methods available.

### Using an Azure AD credential

You can authenticate with Azure AD using the [Azure Identity library][Identity library]. To use the [DefaultAzureCredential][defaultazurecredential] provider, you'll need to install the `@azure/identity` package:

```powershell
npm install @azure/identity
```

You'll need to register the new Azure AD application and grant access to Azure Maps by assigning the required role to your service principal. For more information, see [Host a daemon on non-Azure resources][Host daemon]. During this process you'll get an Application (client) ID, a Directory (tenant) ID, and a client secret. Copy these values and store them in a secure place. You'll need them in the following steps.

Set the values of the Application (client) ID, Directory (tenant) ID, and client secret of your Azure AD application, and the map resource’s client ID as environment variables:

| Environment Variable  | Description                                                     |
|-----------------------|-----------------------------------------------------------------|
| AZURE_CLIENT_ID       | Application (client) ID in your registered application          |
| AZURE_CLIENT_SECRET   | The value of the client secret in your registered application   |
| AZURE_TENANT_ID       | Directory (tenant) ID in your registered application            |
| MAPS_CLIENT_ID        | The client ID in your Azure Map account                         |

You can use a `.env` file for these variables. You'll need to install the [dotenv][dotenv] package:

```powershell
npm install dotenv
```

Next, add a `.env` file in the **mapsDemo** directory and specify these properties:

```text
AZURE_CLIENT_ID="<client-id>"
AZURE_CLIENT_SECRET="<client-secret>"
AZURE_TENANT_ID="<tenant-id>"
MAPS_CLIENT_ID="<maps-client-id>"
```

Once your environment variables are created, you can access them in your JavaScript code:

```JavaScript
const { MapsSearchClient } = require("@azure/maps-search"); 
const { DefaultAzureCredential } = require("@azure/identity"); 
require("dotenv").config(); 
 
const credential = new DefaultAzureCredential(); 
const client = new MapsSearchClient(credential, process.env.MAPS_CLIENT_ID); 
```

### Using a subscription key credential

You can authenticate with your Azure Maps subscription key. Your subscription key can be found in the **Authentication** section in the Azure Maps account as shown in the following screenshot:

:::image type="content" source="./media/rest-sdk-dev-guides/subscription-key.png" alt-text="A screenshot showing the subscription key in the Authentication section of an Azure Maps account." lightbox="./media/rest-sdk-dev-guides/subscription-key.png":::

You need to pass the subscription key to the `AzureKeyCredential` class provided by the [Azure Maps Search client library for JavaScript/TypeScript][JS-SDK]. For security reasons, it's better to specify the key as an environment variable than to include it in your source code.

You can accomplish this by using a `.env` file to store the subscription key variable. You'll need to install the [dotenv][dotenv] package to retrieve the value:

```powershell
npm install dotenv
```

Next, add a `.env` file in the **mapsDemo** directory and specify the property:

```text
MAPS_SUBSCRIPTION_KEY="<subscription-key>"
```

Once your environment variable is created, you can access it in your JavaScript code:

```JavaScript
const { MapsSearchClient, AzureKeyCredential } = require("@azure/maps-search");
require("dotenv").config();

const credential = new AzureKeyCredential(process.env.MAPS_SUBSCRIPTION_KEY);
const client = new MapsSearchClient(credential);
```

## Fuzzy search an entity

The following code snippet demonstrates how, in a simple console application, to import the `azure-maps-search` package and perform a fuzzy search on “Starbucks” near Seattle:

```JavaScript

const { MapsSearchClient, AzureKeyCredential } = require("@azure/maps-search"); 
require("dotenv").config(); 
 
async function main() { 
  // Authenticate with Azure Map Subscription Key 
  const credential = new AzureKeyCredential(process.env.MAPS_SUBSCRIPTION_KEY); 
  const client = new MapsSearchClient(credential); 
 
  // Setup the fuzzy search query 
  const response = await client.fuzzySearch({ 
    query: "Starbucks", 
    coordinates: [47.61010, -122.34255], 
    countryCodeFilter: ["US"], 
  }); 
 
  // Log the result 
  console.log(`Starbucks search result nearby Seattle:`); 
  response.results.forEach((result) => { 
    console.log(`\ 
      * ${result.address.streetNumber} ${result.address.streetName} 
        ${result.address.municipality} ${result.address.countryCode} ${result.address.postalCode} 
        Coordinate: (${result.position[0].toFixed(4)}, ${result.position[1].toFixed(4)})\ 
    `); 
} 
 
main().catch((err) => { 
  console.error(err); 
}); 

```

In the above code snippet, you create a `MapsSearchClient` object using your Azure credentials. This is done using your Azure Maps subscription key, however you could use the [Azure AD credential](#using-an-azure-ad-credential) discussed in the previous section. You then pass the search query and options to the `fuzzySearch` method. Search for Starbucks (`query: "Starbucks"`) near Seattle (`coordinates: [47.61010, -122.34255], countryFilter: ["US"]`). For more information, see [FuzzySearchRequest][FuzzySearchRequest] in the [Azure Maps Search client library for JavaScript/TypeScript][JS-SDK].

The method `fuzzySearch` provided by `MapsSearchClient` will forward the request to Azure Maps REST endpoints. When the results are returned, they're written to the console. For more information, see [SearchAddressResult][SearchAddressResult].

Run `search.js` with Node.js:

```powershell
node search.js 
```

## Search an Address

The [searchAddress][searchAddress] method can be used to get the coordinates of an address. Modify the `search.js` from the sample as follows:

```JavaScript
const { MapsSearchClient, AzureKeyCredential } = require("@azure/maps-search");
require("dotenv").config();

async function main() {
  const credential = new AzureKeyCredential(process.env.MAPS_SUBSCRIPTION_KEY);
  const client = new MapsSearchClient(credential);

  const response = await client.searchAddress(
    "1912 Pike Pl, Seattle, WA 98101, US"
  );

  console.log(`The coordinate is: ${response.results[0].position}`);}

main().catch((err) => {
  console.error(err);
});
```

The results returned from `client.searchAddress` are ordered by confidence score and in this example only the first result returned with be displayed to the screen.

## Batch reverse search

Azure Maps Search also provides some batch query methods. These methods will return Long Running Operations (LRO) objects. The requests might not return all the results immediately, so you can wait until completion or query the result periodically. The example below demonstrates how to call batched reverse search method:

```JavaScript
    const poller = await client.beginReverseSearchAddressBatch([
    // This is an invalid query
    { coordinates: [148.858561, 2.294911] },
    {
      coordinates: [47.61010, -122.34255],
    },
    { coordinates: [47.6155, -122.33817] },
      options: { radiusInMeters: 5000 },
  ]);
```

In this example, three queries are passed into the _batched reverse search_ request. The first query is invalid, see [Handing failed requests](#handing-failed-requests) for an example showing how to handle the invalid query.

Use the `getResult` method from the poller to check the current result. You check the status using `getOperationState` to see if the poller is still running. If it is, you can keep calling `poll` until the operation is finished:

```JavaScript
  while (poller.getOperationState().status === "running") {
    const partialResponse = poller.getResult();
    logResponse(partialResponse)
    await poller.poll();
  }
```

Alternatively, you can wait until the operation has completed, by using `pollUntilDone()`:

```JavaScript
const response = await poller.pollUntilDone();
logResponse(response)
```

A common scenario for LRO is to resume a previous operation later. Do that by serializing the poller’s state with the `toString` method, and rehydrating the state with a new poller using `resumeReverseSearchAddressBatch`:

```JavaScript
  const serializedState = poller.toString();
  const rehydratedPoller = await client.resumeReverseSearchAddressBatch(
    serializedState
  );
  const response = await rehydratedPoller.pollUntilDone();
  logResponse(response);
```

Once you get the response, you can log it:

```JavaScript
function logResponse(response) {
  console.log(
    `${response.totalSuccessfulRequests}/${response.totalRequests} succeed.`
  );
  response.batchItems.forEach((item, idx) => {
    console.log(`The result for ${idx + 1}th request:`);
   // Check if the request is failed
    if (item.response.error) {
      console.error(item.response.error);
    } else {
      item.response.results.forEach((result) => {
        console.log(result.address.freeformAddress);
      });
    }
  });
}
```

### Handing failed requests

Handle failed requests by checking for the `error` property in the response batch item. See the `logResponse` function in the completed batch reverse search example below.

### Completed batch reverse search example

The complete code for the reverse address batch search example:

```JavaScript
const { MapsSearchClient, AzureKeyCredential } = require("@azure/maps-search");
require("dotenv").config();

async function main() {
  const credential = new AzureKeyCredential(process.env.MAPS_SUBSCRIPTION_KEY);
  const client = new MapsSearchClient(credential);

  const poller = await client.beginReverseSearchAddressBatch([
    // This is an invalid query
    { coordinates: [148.858561, 2.294911] },
    {
      coordinates: [47.61010, -122.34255],
    },
    { coordinates: [47.6155, -122.33817] },
      options: { radiusInMeters: 5000 },
  ]);

  // Get the partial result and keep polling
  while (poller.getOperationState().status === "running") {
    const partialResponse = poller.getResult();
    logResponse(partialResponse);
    await poller.poll();
  }

  // You can simply wait for the operation is done
  // const response = await poller.pollUntilDone();
  // logResponse(response)

  // Resume the poller
  const serializedState = poller.toString();
  const rehydratedPoller = await client.resumeReverseSearchAddressBatch(
    serializedState
  );
  const response = await rehydratedPoller.pollUntilDone();
  logResponse(response);
}

function logResponse(response) {
  console.log(
    `${response.totalSuccessfulRequests}/${response.totalRequests} succeed.`
  );
  response.batchItems.forEach((item, idx) => {
    console.log(`The result for ${idx + 1}th request:`);
    if (item.response.error) {
      console.error(item.response.error);
    } else {
      item.response.results.forEach((result) => {
        console.log(result.address.freeformAddress);
      });
    }
  });
}

main().catch((err) => {
  console.error(err);
});
```

## Additional information

- The [Azure Maps Search client library for JavaScript/TypeScript][JS-SDK].

[JS-SDK]: /javascript/api/overview/azure/maps-search-readme?view=azure-node-preview

[defaultazurecredential]: https://github.com/Azure/azure-sdk-for-js/tree/@azure/maps-search_1.0.0-beta.1/sdk/identity/identity#defaultazurecredential

[searchAddress]: /javascript/api/@azure/maps-search/mapssearchclient?view=azure-node-preview#@azure-maps-search-mapssearchclient-searchaddress

[FuzzySearchRequest]: /javascript/api/@azure/maps-search/fuzzysearchrequest?view=azure-node-preview

[SearchAddressResult]: /javascript/api/@azure/maps-search/searchaddressresult?view=azure-node-preview

[search]: /rest/api/maps/search
[Node.js Release]: https://github.com/nodejs/release#release-schedule
[Node.js]: https://nodejs.org/en/download/
[Azure Maps account]: quick-demo-map-app.md#create-an-azure-maps-account
[Subscription key]: quick-demo-map-app.md#get-the-primary-key-for-your-account

[authentication]: azure-maps-authentication.md
[Identity library]: /javascript/api/overview/azure/identity-readme

[Host daemon]: ./how-to-secure-daemon-app.md#host-a-daemon-on-non-azure-resources
[dotenv]: https://github.com/motdotla/dotenv#readme

[search package]: https://www.npmjs.com/package/@azure-rest/maps-search
[search readme]: https://github.com/Azure/azure-sdk-for-js/blob/main/sdk/maps/maps-search-rest/README.md
[search sample]: https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/maps/maps-search-rest/samples/v1-beta

[js route readme]: https://github.com/Azure/azure-sdk-for-js/blob/main/sdk/maps/maps-route-rest/README.md
[js route package]: https://www.npmjs.com/package/@azure-rest/maps-route
[js route sample]: https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/maps/maps-route-rest/samples/v1-beta

[js render readme]: https://github.com/Azure/azure-sdk-for-js/blob/main/sdk/maps/maps-render-rest/README.md
[js render package]: https://www.npmjs.com/package/@azure-rest/maps-render
[js render sample]: https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/maps/maps-render-rest/samples/v1-beta

[js geolocation readme]: https://github.com/Azure/azure-sdk-for-js/blob/main/sdk/maps/maps-geolocation-rest/README.md
[js geolocation package]: https://www.npmjs.com/package/@azure-rest/maps-geolocation
[js geolocation sample]: https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/maps/maps-geolocation-rest/samples/v1-beta
