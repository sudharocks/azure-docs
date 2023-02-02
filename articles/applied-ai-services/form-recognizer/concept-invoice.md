---
title: Invoice data extraction – Form Recognizer
titleSuffix: Azure Applied AI Services
description: Automate invoice data extraction with Form Recognizer's invoice model to extract accounts payable data including invoice line items.
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 01/25/2023
ms.author: lajanuar
recommendations: false
---
<!-- markdownlint-disable MD033 -->

# Azure Form Recognizer invoice model

::: moniker range="form-recog-3.0.0"
[!INCLUDE [applies to v3.0](includes/applies-to-v3-0.md)]
::: moniker-end

::: moniker range="form-recog-2.1.0"
[!INCLUDE [applies to v2.1](includes/applies-to-v2-1.md)]
::: moniker-end

The Form Recognizer invoice model uses powerful Optical Character Recognition (OCR) capabilities to analyze and extract key fields and line items from sales invoices, utility bills, and purchase orders. Invoices can be of various formats and quality including phone-captured images, scanned documents, and digital PDFs. The API analyzes invoice text; extracts key information such as customer name, billing address, due date, and amount due; and returns a structured JSON data representation. The model currently supports both English and Spanish invoices.

**Supported document types:**

* Invoices
* Utility bills
* Sales orders
* Purchase orders

## Automated invoice processing

Automated invoice processing is the process of extracting key accounts payable fields from billing account documents. Extracted data includes line items from invoices integrated with your accounts payable (AP) workflows for reviews and payments. Historically, the accounts payable process has been done manually and, hence, very time consuming. Accurate extraction of key data from invoices is typically the first and one of the most critical steps in the invoice automation process.

::: moniker range="form-recog-3.0.0"

**Sample invoice processed with [Form Recognizer Studio](https://formrecognizer.appliedai.azure.com/studio/prebuilt?formType=invoice)**:

:::image type="content" source="media/studio/overview-invoices.png" alt-text="Screenshot of a sample invoice analyzed in the Form Recognizer Studio." lightbox="media/overview-invoices-big.jpg":::

::: moniker-end

::: moniker range="form-recog-2.1.0"

**Sample invoice processed with [Form Recognizer Sample Labeling tool](https://fott-2-1.azurewebsites.net)**:

:::image type="content" source="media/invoice-example-new.jpg" alt-text="Screenshot of a sample invoice.":::

::: moniker-end

## Development options

::: moniker range="form-recog-3.0.0"

The following tools are supported by Form Recognizer v3.0:

| Feature | Resources | Model ID |
|----------|-------------|-----------|
|**Invoice model** | <ul><li>[**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)</li><li>[**REST API**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-2022-08-31/operations/AnalyzeDocument)</li><li>[**C# SDK**](quickstarts/get-started-sdks-rest-api.md?view=form-recog-3.0.0&preserve-view=true)</li><li>[**Python SDK**](quickstarts/get-started-sdks-rest-api.md?view=form-recog-3.0.0&preserve-view=true)</li><li>[**Java SDK**](quickstarts/get-started-sdks-rest-api.md?view=form-recog-3.0.0&preserve-view=true)</li><li>[**JavaScript SDK**](quickstarts/get-started-sdks-rest-api.md?view=form-recog-3.0.0&preserve-view=true)</li></ul>|**prebuilt-invoice**|

::: moniker-end

::: moniker range="form-recog-2.1.0"

The following tools are supported by Form Recognizer v2.1:

| Feature | Resources |
|----------|-------------------------|
|**Invoice model**| <ul><li>[**Form Recognizer labeling tool**](https://fott-2-1.azurewebsites.net/prebuilts-analyze)</li><li>[**REST API**](./how-to-guides/use-sdk-rest-api.md?pivots=programming-language-rest-api&preserve-view=true&tabs=windows&view=form-recog-2.1.0#analyze-invoices)</li><li>[**Client-library SDK**](/azure/applied-ai-services/form-recognizer/how-to-guides/v2-1-sdk-rest-api)</li><li>[**Form Recognizer Docker container**](containers/form-recognizer-container-install-run.md?tabs=invoice#run-the-container-with-the-docker-compose-up-command)</li></ul>|

::: moniker-end

## Input requirements

::: moniker range="form-recog-3.0.0"

[!INCLUDE [input requirements](./includes/input-requirements.md)]

::: moniker-end

::: moniker range="form-recog-2.1.0"

* Supported file formats: JPEG, PNG, PDF, and TIFF
* For PDF and TIFF, up to 2000 pages are processed. For free tier subscribers, only the first two pages are processed.
* The file size must be less than 50 MB and dimensions at least 50 x 50 pixels and at most 10,000 x 10,000 pixels.

::: moniker-end

## Try invoice data extraction

See how data, including customer information, vendor details, and line items, is extracted from invoices. You'll need the following resources:

* An Azure subscription—you can [create one for free](https://azure.microsoft.com/free/cognitive-services/)

* A [Form Recognizer instance](https://portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) in the Azure portal. You can use the free pricing tier (`F0`) to try the service. After your resource deploys, select **Go to resource** to get your key and endpoint.

 :::image type="content" source="media/containers/keys-and-endpoint.png" alt-text="Screenshot: keys and endpoint location in the Azure portal.":::

::: moniker range="form-recog-3.0.0"

## Form Recognizer Studio

1. On the Form Recognizer Studio home page, select **Invoices**

1. You can analyze the sample invoice or select the **+ Add** button to upload your own sample.

1. Select the **Analyze** button:

    :::image type="content" source="media/studio/invoice-analyze.png" alt-text="Screenshot: analyze invoice menu.":::

> [!div class="nextstepaction"]
> [Try Form Recognizer Studio](https://formrecognizer.appliedai.azure.com/studio/prebuilt?formType=invoice)

::: moniker-end

::: moniker range="form-recog-2.1.0"

## Form Recognizer Sample Labeling tool

1. Navigate to the [Form Recognizer Sample Tool](https://fott-2-1.azurewebsites.net/).

1. On the sample tool home page, select the **Use prebuilt model to get data** tile.

    :::image type="content" source="media/label-tool/prebuilt-1.jpg" alt-text="Screenshot of layout model analyze results process.":::

1. Select the **Form Type**  to analyze from the dropdown menu.

1. Choose a URL for the file you would like to analyze from the below options:

    * [**Sample invoice document**](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/invoice_sample.jpg).
    * [**Sample ID document**](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/DriverLicense.png).
    * [**Sample receipt image**](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/contoso-allinone.jpg).
    * [**Sample business card image**](https://raw.githubusercontent.com/Azure/azure-sdk-for-python/master/sdk/formrecognizer/azure-ai-formrecognizer/samples/sample_forms/business_cards/business-card-english.jpg).

1. In the **Source** field, select **URL** from the dropdown menu, paste the selected URL, and select the **Fetch** button.

    :::image type="content" source="media/label-tool/fott-select-url.png" alt-text="Screenshot of source location dropdown menu.":::

1. In the **Form recognizer service endpoint** field, paste the endpoint that you obtained with your Form Recognizer subscription.

1. In the **key** field, paste  the key you obtained from your Form Recognizer resource.

    :::image type="content" source="media/fott-select-form-type.png" alt-text="Screenshot showing the select-form-type dropdown menu.":::

1. Select **Run analysis**. The Form Recognizer Sample Labeling tool will call the Analyze Prebuilt API and analyze the document.

1. View the results - see the key-value pairs extracted, line items, highlighted text extracted and tables detected.

    :::image type="content" source="media/invoice-example-new.jpg" alt-text="Screenshot of layout model analyze results operation.":::

> [!NOTE]
> The [Sample Labeling tool](https://fott-2-1.azurewebsites.net/) does not support the BMP file format. This is a limitation of the tool not the Form Recognizer Service.

::: moniker-end

::: moniker range="form-recog-3.0.0"

## Supported languages and locales

>[!NOTE]
> Form Recognizer auto-detects language and locale data.

| Supported languages | Details |
|:----------------------|:---------|
| &bullet; English | United States (us), Australia (-au), Canada (-ca), Great Britain (-gb), India (-in)|
| &bullet; Spanish |Spain (es)|
| &bullet; German | Germany (de)|
| &bullet; French | France (fr) |
| &bullet; Italian | Italy (it)|
| &bullet; Portuguese | Portugal (-pt), Brazil (-br)|
| &bullet; Dutch | Netherlands (de)|

## Field extraction

|Name| Type | Description | Standardized output |
|:-----|:----|:----|:---:|
| CustomerName | String | Invoiced customer| |
| CustomerId | String | Customer reference ID | |
| PurchaseOrder | String | Purchase order reference number | |
| InvoiceId | String | ID for this specific invoice (often "Invoice Number") | |
| InvoiceDate | Date | Date the invoice was issued | yyyy-mm-dd|
| DueDate | Date | Date payment for this invoice is due | yyyy-mm-dd|
| VendorName | String | Vendor name |  |
| VendorTaxId | String | The taxpayer number associated with the vendor | |
| VendorAddress | String |  Vendor mailing address|  |
| VendorAddressRecipient | String | Name associated with the VendorAddress |  |
| CustomerAddress | String | Mailing address for the Customer | |
| CustomerTaxId | String | The taxpayer number associated with the customer | |
| CustomerAddressRecipient | String | Name associated with the CustomerAddress | |
| BillingAddress | String | Explicit billing address for the customer |  |
| BillingAddressRecipient | String | Name associated with the BillingAddress | |
| ShippingAddress | String | Explicit shipping address for the customer | |
| ShippingAddressRecipient | String | Name associated with the ShippingAddress |  |
| PaymentTerm | String | The terms of payment for the invoice | |
| SubTotal | Number | Subtotal field identified on this invoice | Integer |
| TotalTax | Number | Total tax field identified on this invoice | Integer |
| InvoiceTotal | Number (USD) | Total new charges associated with this invoice | Integer |
| AmountDue |  Number (USD) | Total Amount Due to the vendor | Integer |
| ServiceAddress | String | Explicit service address or property address for the customer | |
| ServiceAddressRecipient | String | Name associated with the ServiceAddress |  |
| RemittanceAddress | String | Explicit remittance or payment address for the customer |   |
| RemittanceAddressRecipient | String | Name associated with the RemittanceAddress |  |
| ServiceStartDate | Date | First date for the service period (for example, a utility bill service period) | yyyy-mm-dd |
| ServiceEndDate | Date | End date for the service period (for example, a utility bill service period) | yyyy-mm-dd|
| PreviousUnpaidBalance | Number | Explicit previously unpaid balance | Integer |
| CurrencyCode | String | The Currency Code associated with an extracted amount |  |
| PaymentOptions | Array | An array that holds Payment Option details such as `IBAN`and `SWIFT` |  |
| TotalDiscount | Number | The total discount applied to an invoice | Integer |
| TaxItems (en-IN only) | Array | AN array that holds added tax information such as `CGST`, `IGST`, and `SGST`. This line item is currently only available for the en-in locale  |  |

### Line items

Following are the line items extracted from an invoice in the JSON output response (the following output uses this [sample invoice](media/sample-invoice.jpg))

|Name| Type | Description | Text (line item #1) | Value (standardized output) |
|:-----|:----|:----|:----| :----|
| Items | String | Full string text line of the line item | 3/4/2021 A123 Consulting Services 2 hours $30.00 10% $60.00 | |
| Amount | Number | The amount of the line item | $60.00 | 100 |
| Description | String | The text description for the invoice line item | Consulting service | Consulting service |
| Quantity | Number | The quantity for this invoice line item | 2 | 2 |
| UnitPrice | Number | The net or gross price (depending on the gross invoice setting of the invoice) of one unit of this item | $30.00 | 30 |
| ProductCode | String| Product code, product number, or SKU associated with the specific line item | A123 | |
| Unit | String| The unit of the line item, e.g,  kg, lb etc. | Hours | |
| Date | Date| Date corresponding to each line item. Often it's a date the line item was shipped | 3/4/2021| 2021-03-04 |
| Tax | Number | Tax associated with each line item. Possible values include tax amount and tax Y/N | 10.00 | |
| TaxRate | Number | Tax Rate associated with each line item. | 10% | |

The invoice key-value pairs and line items extracted are in the `documentResults` section of the JSON output.

### Key-value pairs

The prebuilt invoice **2022-06-30** and later releases support returns key-value pairs at no extra cost. Key-value pairs are specific spans within the invoice that identify a label or key and its associated response or value. In an invoice, these pairs could be the label and the value the user entered for that field or telephone number. The AI model is trained to extract identifiable keys and values based on a wide variety of document types, formats, and structures.

Keys can also exist in isolation when the model detects that a key exists, with no associated value or when processing optional fields. For example, a middle name field may be left blank on a form in some instances. key-value pairs are always spans of text contained in the document. For documents where the same value is described in different ways, for example, customer/user, the associated key will be either customer or user (based on context).

::: moniker-end

::: moniker range="form-recog-2.1.0"

## Supported locales

**Prebuilt invoice v2.1** supports invoices in the **en-us** locale.

## Fields extracted

The Invoice service will extract the text, tables, and 26 invoice fields. Following are the fields extracted from an invoice in the JSON output response (the following output uses this [sample invoice](media/sample-invoice.jpg)).

|Name| Type | Description | Text | Value (standardized output) |
|:-----|:----|:----|:----| :----|
| CustomerName | string | Customer being invoiced | Microsoft Corp |  |
| CustomerId | string | Reference ID for the customer | CID-12345 |  |
| PurchaseOrder | string | A purchase order reference number | PO-3333 | |
| InvoiceId | string | ID for this specific invoice (often "Invoice Number") | INV-100 | |
| InvoiceDate | date | Date the invoice was issued | 11/15/2019 | 2019-11-15 |
| DueDate | date | Date payment for this invoice is due | 12/15/2019 | 2019-12-15 |
| VendorName | string | Vendor who has created this invoice | CONTOSO LTD. | |
| VendorAddress | string | Mailing address for the Vendor | 123 456th St New York, NY, 10001 | |
| VendorAddressRecipient | string | Name associated with the VendorAddress | Contoso Headquarters | |
| CustomerAddress | string | Mailing address for the Customer | 123 Other Street, Redmond WA, 98052 | |
| CustomerAddressRecipient | string | Name associated with the CustomerAddress | Microsoft Corp | |
| BillingAddress | string | Explicit billing address for the customer | 123 Bill Street, Redmond WA, 98052 | |
| BillingAddressRecipient | string | Name associated with the BillingAddress | Microsoft Services | |
| ShippingAddress | string | Explicit shipping address for the customer | 123 Ship Street, Redmond WA, 98052 | |
| ShippingAddressRecipient | string | Name associated with the ShippingAddress | Microsoft Delivery | |
| SubTotal | number | Subtotal field identified on this invoice | $100.00 | 100 |
| TotalTax | number | Total tax field identified on this invoice | $10.00 | 10 |
| InvoiceTotal | number | Total new charges associated with this invoice | $110.00 | 110 |
| AmountDue |  number | Total Amount Due to the vendor | $610.00 | 610 |
| ServiceAddress | string | Explicit service address or property address for the customer | 123 Service Street, Redmond WA, 98052 | |
| ServiceAddressRecipient | string | Name associated with the ServiceAddress | Microsoft Services | |
| RemittanceAddress | string | Explicit remittance or payment address for the customer | 123 Remit St New York, NY, 10001 |  |
| RemittanceAddressRecipient | string | Name associated with the RemittanceAddress | Contoso Billing |  |
| ServiceStartDate | date | First date for the service period (for example, a utility bill service period) | 10/14/2019 | 2019-10-14 |
| ServiceEndDate | date | End date for the service period (for example, a utility bill service period) | 11/14/2019 | 2019-11-14 |
| PreviousUnpaidBalance | number | Explicit previously unpaid balance | $500.00 | 500 |

Following are the line items extracted from an invoice in the JSON output response (the following output uses this [sample invoice](./media/sample-invoice.jpg))

|Name| Type | Description | Text (line item #1) | Value (standardized output) |
|:-----|:----|:----|:----| :----|
| Items | string | Full string text line of the line item | 3/4/2021 A123 Consulting Services 2 hours $30.00 10% $60.00 | |
| Amount | number | The amount of the line item | $60.00 | 100 |
| Description | string | The text description for the invoice line item | Consulting service | Consulting service |
| Quantity | number | The quantity for this invoice line item | 2 | 2 |
| UnitPrice | number | The net or gross price (depending on the gross invoice setting of the invoice) of one unit of this item | $30.00 | 30 |
| ProductCode | string| Product code, product number, or SKU associated with the specific line item | A123 | |
| Unit | string| The unit of the line item, e.g,  kg, lb etc. | hours | |
| Date | date| Date corresponding to each line item. Often it's a date the line item was shipped | 3/4/2021| 2021-03-04 |
| Tax | number | Tax associated with each line item. Possible values include tax amount, tax %, and tax Y/N | 10% | |

### JSON output

The JSON output has three parts:

* `"readResults"` node contains all of the recognized text and selection marks. Text is organized by page, then by line, then by individual words.
* `"pageResults"` node contains the tables and cells extracted with their bounding boxes, confidence, and a reference to the lines and words in "readResults".
* `"documentResults"` node contains the invoice-specific values and line items that the model discovered. It's where you'll find all the fields from the invoice such as invoice ID, ship to, bill to, customer, total, line items and lots more.

## Migration guide

* Follow our [**Form Recognizer v3.0 migration guide**](v3-migration-guide.md) to learn how to use the v3.0 version in your applications and workflows.

::: moniker-end

## Next steps

::: moniker range="form-recog-3.0.0"

* Try processing your own forms and documents with the [Form Recognizer Studio](https://formrecognizer.appliedai.azure.com/studio)

* Complete a [Form Recognizer quickstart](quickstarts/get-started-sdks-rest-api.md?view=form-recog-3.0.0&preserve-view=true) and get started creating a document processing app in the development language of your choice.

::: moniker-end

::: moniker range="form-recog-2.1.0"

* Try processing your own forms and documents with the [Form Recognizer Sample Labeling tool](https://fott-2-1.azurewebsites.net/)

* Complete a [Form Recognizer quickstart](quickstarts/get-started-sdks-rest-api.md?view=form-recog-2.1.0&preserve-view=true) and get started creating a document processing app in the development language of your choice.

::: moniker-end
