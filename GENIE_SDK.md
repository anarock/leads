# Genie SDK

## Introduction
This documentation outlines the installation process, usage, and configuration options of the Genie SDK.

#### Pre Requisite - Agent Id 
Get the Agent Id from the support team.

## Getting Started

### Installation
To integrate the Genie SDK into your web application, include the following script tag in the `<head>` section of your HTML document:

```html
<script src="https://genie-an.s3.ap-south-1.amazonaws.com/genie_sdk/v2.0.0/genieSDK.min.js" defer></script>
```

### Initialization
Initialize the Genie SDK by adding the following JavaScript code, typically after the script tag or within a script file that is loaded after the page content:

```javascript
document.addEventListener("DOMContentLoaded", function () {
  const genieSDK = new GenieSDK({
    containerId: "your_iframe_container_id_here",
    iframeId: "your_iframe_id_here",
    agentId: "your_agent_id_here", // Get the Agent ID from the support team.
    iframeClassName: "your_iframe_class_name_here",
    landingPageUrl: "your_redirection_url_here",
    domain: "anarockgenie.com", // Optional, default is "anarockgenie.com"
    campaignId: "your_campaign_id_here", // Optional, If provided, it will be used for lead synchronization when lead_id is not available
  });
  genieSDK.insertIframe();
});
```

### HTML Setup
For example, your complete HTML might look like this:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <!-- FOR MOBILE RESPONSIVE VIEW -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Genie SDK Integration Example</title>
    <script src="https://genie-an.s3.ap-south-1.amazonaws.com/genie_sdk/v2.0.0/genieSDK.min.js" defer></script>
    <script>
        document.addEventListener("DOMContentLoaded", function() {
            const genieSDK = new GenieSDK({
                containerId: "iframe-wrapper",
                iframeId: "genie-iframe",
                agentId: "XX", // Get the agentId from the support team
                iframeClassName: "iframe-fullscreen",
                landingPageUrl: "https://example.com/", // Replace this with your landing page URL
                domain: "anarockgenie.com",             // Optional
                campaignId: "your_campaign_id_here", // Optional, If provided, it will be used for lead synchronization when lead_id is not available
            });
            genieSDK.insertIframe();
        });
    </script>

    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            width: 100%;
        }

        .header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 1rem;
        }
        
        @media screen and (min-width: 1024px) {
            .header {
                padding: 20px 100px;
            }
        }
    
        .home-btn {
            color: #fff;
            background-color: #6161f1;
            font-size: 0.9em;
            padding: 8px 15px;
            border-radius: 3px;
            text-decoration: none;
        }
    
        .main-container {
            height: 100svh;
            width: 100svw;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }
    
        .iframe-container {
            flex-grow: 1;
        }
    
        .iframe-fullscreen {
            width: 100%;
            height: 100%;
            border: none;
        }
    </style>
</head>
<body>

  <div class="main-container">
    <div class="header">
      <!-- Replace img src with your logo -->
      <img src="images/logo.png" width="80px"/>

      <!-- Replace href with your landing page URL -->
      <a class="home-btn" href="https://example.com">HOME</a>
    </div>

    <div id="iframe-wrapper" class="iframe-container"></div>
  </div>

</body>
</html>
```

## Configuration

### GenieSDKOptions
Configure the `GenieSDK` class using the following options:

| Option            | Type     | Required/Optional | Default Value        | Description                                                                                                                                     |
| ----------------- | -------- | ----------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `containerId`     | `string` | **Required**      |                      | The ID of the container element where the iframe will be embedded.                                                                              |
| `iframeId`        | `string` | *Optional*        | `"genie-id"`         | The ID that will be assigned to the iframe.                                                                                                     |
| `agentId`         | `string` | **Required**      |                      | Identifier for the agent or service the SDK is interacting with.                                                                                |
| `iframeClassName` | `string` | *Optional*        | `"genie-class"`      | CSS class to assign to the iframe.                                                                                                              |
| `landingPageUrl`  | `string` | *Optional*        |                      | URL to redirect the user to in case something went wrong or for tracking purposes.                                                              |
| `domain`          | `string` | *Optional*        | `"anarockgenie.com"` | The domain where the Genie iframe is hosted. Change this only if instructed by the support team.                                                |
| `campaignId`      | `string` | *Optional*        |                      | Campaign identifier used for lead synchronization if `lead_id` is not provided. If provided, the SDK will attempt to sync lead using this ID.   |

---

### Methods

#### `insertIframe()`
This method creates and inserts an iframe into the specified container on the web page. It generates the iframe's source URL using the configured options and the current page's URL parameters.

---

## Required Query Parameters in Parent Window

To ensure proper functionality of the Genie SDK, the parent window URL should include the following query parameters:

- `name`: The name of the lead.
- `phone`: The phone number of the lead.
- `country_code`: The country code of the phone number.
- `lead_id`: The unique identifier for the lead.

**Note:** If `lead_id` is not provided and `campaignId` is provided in the SDK options, the SDK will attempt to synchronize the lead by calling the lead sync API. If `lead_id` is not provided and `campaignId` is not provided, the SDK will log an error and will not proceed.

---

### Example: Passing Query Parameters

Use the following example to pass the required parameters inside your `Anarock.submitLead` function call.
```javascript
function onLeadSuccessCallback(leadId) {
  const ccode = 'Lead`s CountryCode'; // Example: '91'
  const phone = 'Lead`s PhoneNumber'; // Example: '1234567890'
  const name = 'Lead`s Name';         // Example: 'John Doe'

  const params = new URLSearchParams({
    country_code: ccode,
    phone: phone,
    lead_id: leadId,
    name: name,
  });

  const thankyouPage = 'thank-you.html'; // Replace with the name of your thank-you page.

  window.location.href = `${window.location.protocol}//${window.location.hostname}/${thankyouPage}?${params.toString()}`;
}
```

---

### Complete `Anarock.submitLead` Function Call Example

```javascript
Anarock.submitLead({
  name: name,
  phone: phone,
  ccode: ccode,
  remarks: remarks,
  email: email,
  source: source,
  subsource: sub_source,
  campaign_id: 'xxxx',
  channel_name: 'website',
  api_key: 'xxxx',

  onLeadSuccessCallback: function (leadID) {
    const params = new URLSearchParams({
      country_code: ccode,
      phone: phone,
      lead_id: leadID,
      name: name,
    });

    const thankyouPage = 'thank-you.html';

    window.location.href = `${window.location.protocol}//${window.location.hostname}/${thankyouPage}?${params.toString()}`;
  },

  onLeadFailureCallback: function () {
    alert("An error occurred while submitting the lead.");
  },

  onInvalidPhone: function () {
    alert("Please enter a valid phone number.");
  },

  env: 'production',
});
```

**Replace** `your_campaign_id_here`, `your_api_key_here`, `Lead's CountryCode`, `Lead's PhoneNumber`, and `Lead's Name` with actual data relevant to your application. The `leadId` parameter is provided to the callback function upon a successful lead submission.

---

## Handling Lead Synchronization

If `lead_id` is not provided in the query parameters, and you have provided `campaignId` in the SDK options, the SDK will attempt to synchronize the lead by calling the lead sync API. Ensure that you have provided the `campaignId` in the SDK options. The SDK uses the following parameters for lead synchronization:

- `integration_key`: Provided internally by the SDK.
- `source`, `sub_source`, `purpose`: Predefined constants used by the SDK.
- `name`, `phone`: Retrieved from the query parameters or defaults.
- `campaign_id`: Provided in the SDK options.

---

## Error Handling and Logging

The SDK includes improved error handling and logging:

- **Missing Parameters:** The `validateRequiredParameters` method logs missing or invalid parameters but does not halt execution.
- **Lead Sync Errors:** If lead synchronization fails, an error is logged, and the iframe is not inserted.
- **Element Not Found:** If the container element specified by `containerId` is not found in the DOM, an error is logged.
- **Missing `lead_id` and `campaignId`:** If `lead_id` is not provided and `campaignId` is also not provided, the SDK logs an error and does not proceed.

---

## Conclusion
The Genie SDK facilitates the integration of a dynamic iframe into web pages, providing various customization and configuration options to meet application needs.

---

**Note:** Ensure that all the required query parameters are provided in the parent window URL to avoid any unexpected behavior. If you wish to enable automatic lead synchronization when `lead_id` is not available, provide the `campaignId` in the SDK options.

---