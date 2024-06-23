# Batch Ship Packages

Use this API to batch ship packages from a single shop by providing multiple package IDs in the request body. You can retrieve package IDs by running the [Get Order Detail](https://partner.tiktokshop.com/docv2/page/650aa8ccc16ffe02b8f167a0?external_id=650aa8ccc16ffe02b8f167a0) API.

This API is available for both TikTok shipping orders and seller shipping orders.

* TikTok Shipping: Schedule a package handover time for TikTok shipping carriers to pick up a package from the seller. Include the pickup `start_time` and `end_time`.
* Seller Shipping: The seller arranges their own shipping and provides a `tracking_number` and `shipping_provider_id`.

## URL

`POST /fulfillment/202309/packages/ship`

## Request

View [common parameters](https://partner.tiktokshop.com/docv2/page/662fd3ffbd5e0902be3a486a).

### Header

| Properties | Type | Description| 
| -----------|------|------------|
| `content-type` <br> <span style="color: red;font-family:Consolas">Required</span> | `string` | Allowed type: application/json 

### Query

| Properties    | Type | Description| 
| --------------|------|------------|
|`shop_cipher` <span style="color: red;font-family:Consolas">Required</span> | `string` | Use this property to pass a shop's cipher in the requesting API. Failure to pass a cipher for an authorized cross-border shop will result in an error. You can retrieve a list of ciphers for authorized shops by running the [Get Authorized Shops](https://partner.tiktokshop.com/docv2/page/6507ead7b99d5302be949ba9?external_id=6507ead7b99d5302be949ba9) API.  


### Body

| Properties | Type | Description| 
| -----------|------|------------|
|`packages`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br> <span style="color: red;font-family&nbsp;&nbsp;&nbsp;&nbsp;nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Consolas">Required</span> | `[]object]` | An array of package objects |
| &nbsp;&nbsp;&nbsp;&nbsp;`packages.id` <br> <span style="color: red;font-family:Consolas">&nbsp;&nbsp;Required</span> | `string` | The package ID |
| &nbsp;&nbsp;&nbsp;&nbsp;`packages.handover_method` | `string` | The package handover type. Available values are <br><ul><li>`PICKUP`: A shipping provider will pick up the package(s) from the seller's pickup address.</li><li>`DROP_OFF`: The seller will drop off the package(s) at a designated location.</li></ul> |
| &nbsp;&nbsp;&nbsp;&nbsp;`packages.pickup_slot` | `[]object` | The package pickup timeslot when `handover_method` is `PICKUP`. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`pickup_slot.start_time` | `string` | The start date and time of the package pickup time slot. This value is a Unix time stamp. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`pickup_slot.end_time` | `string` | The end date and time of the package pickup time slot. This value is a Unix time stamp. |
| &nbsp;&nbsp;&nbsp;&nbsp;`packages.self_shipment` | `[]object` | This object is only needed when the seller is shipping packages. <br> Review the shipping_type field in the [Get Package Detail](https://partner.tiktokshop.com/docv2/page/650aa39fbace3e02b75d8617?external_id=650aa39fbace3e02b75d8617) API response to determine who is shipping. <br> Use the shipping provider ID retrieved from the [Get Shipping Providers](https://partner.tiktokshop.com/docv2/page/650aa48d4a0bb702c06d85cd?external_id=650aa48d4a0bb702c06d85cd) API and upload the corresponding tracking number. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`self_shipment.tracking_number`<br> <span style="color: red;font-family:Consolas">&nbsp;&nbsp;&nbsp;&nbsp;Required</span> | `string ` | The package's tracking number when `shipping_type` is `SELLER`. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`self_shipment.shipping_provider_id`<br> <span style="color: red;font-family:Consolas">&nbsp;&nbsp;&nbsp;&nbsp;Required</span> | `string ` | The shipping provider ID when `shipping_type` is `SELLER`. Use the [Get Shipping Providers](https://partner.tiktokshop.com/docv2/page/650aa48d4a0bb702c06d85cd?external_id=650aa48d4a0bb702c06d85cd) API to retrieve this value.

## Examples


=== "CURL"

    ``` curl linenums="1"
    https://open-api.tiktokglobalshop.com/fulfillment/202309/packages/ship?app_key=123abc&sign=5361235029d141222525e303d742f9e38aea052d10896d3197ab9d6233730b8c&timestamp=1625484268&shop_cipher=ROW_RHkDDABBAAB8tKAVoAqsMTjsQZFLyNfY
    ```

=== "JSON"

    ``` json linenums="1"
    {
      "packages": [
        {
          "handover_method": "PICKUP",
          "id": "12321312312431",
          "pickup_slot": {
            "end_time": 1623812664,
            "start_time": 1623812664
          },
          "self_shipment": {
            "shipping_provider_id": "6617675021119438849",
            "tracking_number": "JX12345"
          }
        }
      ]
    }
    ```

=== "JAVA"

    ``` java linenums="1"
    import io.swagger.client.*;
    import io.swagger.client.auth.*;
    import io.swagger.client.model.*;
    import io.swagger.client.api.ShipApi;

    import java.io.File;
    import java.util.*;

    public class ShipApiExample {

        public static void main(String[] args) {
            ApiClient defaultClient = Configuration.getDefaultApiClient();

            // Configure OAuth2 access token for authorization: tiktok-shops
            OAuth tiktok-shops_auth = (OAuth) defaultClient.getAuthentication("tiktok-shops_auth");
            tiktok-shops_auth.setAccessToken("YOUR ACCESS TOKEN");

            ShipApi apiInstance = new ShipApi();
            Packages body = ; // Packages | View common parameters.
            String shopCipher = shopCipher_example; // String | Use this property to pass a shop's cipher in the requesting API. Failure to pass a cipher for an authorized cross-border shop will result in an error. You can retrieve a list of ciphers for authorized shops by running the Get Authorized Shops API.
            try {
                packages result = apiInstance.batchShipPackages(body, shopCipher);
                System.out.println(result);
            } catch (ApiException e) {
                System.err.println("Exception when calling ShipApi#batchShipPackages");
                e.printStackTrace();
            }
        }
    }
    ```
=== "C++"

    ``` c++ linenums="1"
    using System;
    using System.Diagnostics;
    using IO.Swagger.Api;
    using IO.Swagger.Client;
    using IO.Swagger.Model;

    namespace Example
    {
        public class batchShipPackagesExample
        {
            public void main()
            {

                // Configure OAuth2 access token for authorization: tiktok-shops
                Configuration.Default.AccessToken = "YOUR_ACCESS_TOKEN";

                var apiInstance = new ShipApi();
                var body = new Packages(); // Packages | View common parameters.
                var shopCipher = shopCipher_example;  // String | Use this property to pass a shop's cipher in the requesting API. Failure to pass a cipher for an authorized cross-border shop will result in an error. You can retrieve a list of ciphers for authorized shops by running the Get Authorized Shops API.

                try
                {
                    // Batch ship packages
                    packages result = apiInstance.batchShipPackages(body, shopCipher);
                    Debug.WriteLine(result);
                }
                catch (Exception e)
                {
                    Debug.Print("Exception when calling ShipApi.batchShipPackages: " + e.Message );
                }
            }
        }
    }
    ```

=== "Python"

    ``` py linenums="1"
    from __future__ import print_statement
    import time
    import swagger_client
    from swagger_client.rest import ApiException
    from pprint import pprint

    # Configure OAuth2 access token for authorization: tiktok-shop_auth
    swagger_client.configuration.access_token = 'YOUR_ACCESS_TOKEN'

    # create an instance of the API class
    api_instance = swagger_client.ShipApi()
    body =  # Packages | View common parameters.
    shopCipher = shopCipher_example # String | Use this property to pass a shop's cipher in the requesting API. Failure to pass a cipher for an authorized cross-border shop will result in an error. You can retrieve a list of ciphers for authorized shops by running the Get Authorized Shops API.

    try: 
        # Batch ship packages
        api_response = api_instance.batch_ship_packages(body, shopCipher)
        pprint(api_response)
    except ApiException as e:
    print("Exception when calling ShipApi->batchShipPackages: %s\n" % e)
    ```

## Error Codes

View [common error codes](https://partner.tiktokshop.com/docv2/page/662fd3cbba28f302e05be68c).

Code | Message 
-----|---------
11001001 | System timeout. If multiple retries have failed, please contact the platform for assistance.
11006013 | Failed to ship. Products under “Phones & Tablets” category could not be shipped together with other items in one package. Please split the order. 
11006014 | 3c category item count is over the limit.
11021009 | No available shipping services for this request. Please modify the package weight or check the recipient address. If no available shipping service is found, please contact the platform for assistance.
11021010 | The shipment failed due to an invalid postal code. Please ask the buyer to edit the address. If the address cannot be changed, cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021011 | Failed to ship package because the product name is invalid. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021012 | Failed to ship package because the buyer's recipient district is invalid. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021013 | The shipment failed because the delivery address is too long. Please ask the buyer to shorten the address. If the address cannot be changed, cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021014 | The shipment failed because the product is oversized. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021015 | Failed to ship the package because the package is overweight. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021017 | Failed to ship the package because the SKU dimensions exceed the restrictions. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021018 | Failed to ship the package because the number of items in the package exceeds the allowed limit. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021019 | Failed to ship package because the package dimensions exceed the restrictions. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021020 | Failed to ship the package because the package's circumference exceeds the allowed limit. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021021 | Failed to ship the package because the SKU name is invalid. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021022 | Failed to ship the package because the longest side of the package exceeds the allowed limit. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021023 | Failed to ship the package because the invalid buyer and seller addresses Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021024 | Failed to ship the package because the buyer recipient address is invalid. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11021025 | Failed to ship the package because the seller shipping address is invalid. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
11028006 | This tracking number cannot be matched to any carrier. Please check the tracking number and upload again.
21001002 | System timeout. If multiple retries have failed, please contact the platform for assistance.
21001003 | System timeout. If multiple retries have failed, please contact the platform for assistance.
21001005 | Cache error
21001010 | System process error. Please try again later.
21001011 | System process error. Please try again later.
21001201 | System process error. Please try again later.
21004006 | Failed to ship. Please upload the invoice before shipping.
21008044 | Package item has an after-sale request. Please process the after-sale request first.
21011001 | Package not found. If multiple retries have failed, please contact the platform for assistance.
21011003 | Package does not allow “ready-to-ship” (RTS). If multiple retries have failed, please contact the platform for assistance
21011007 | Package item has an after-sale request. Please process the after-sale request first.
21011008 | Package is not complete. If multiple retries have failed, please contact the platform for assistance.
21011022 | Invalid tracking number or provider ID. Please check these values and upload again.
21011025 | Duplicate tracking number. Pease check the tracking number and upload again.
21011027 | Package has been delivered. Please do not retry.
21011028 | The package does not belong to the current shop. 
21011040 | Package has shipped. Please do not retry.
21011041 | Cannot complete the operation when order is cancelled. Ensure to operate orders according to the correct statues.
21011500 | System process error. Please try again later.
21014009 | Failed to ship. Cancel the order and select the following reason for cancellation: Unable to deliver to buyer address. This will not be counted toward your cancellation rate.
99999999 | System process error. If multiple retries have failed, please contact the platform for assistance.