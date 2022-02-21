# Asendia iShipper

<aside class="notice">

iShipper is Asendia Online Shipping Platform for Asia region. iShipper provdies customers with online access to web services for direct system integration. This section outlines how to invoke the web services, and provide field by field details for each service.
Asendia I-Shipper Platform's API is based on HTTP protocol and rest services. The data format of the Request & Response is in JSON format.
The basic workflow of iShipper can be described in the diagram below:
</aside>

<img src="/images/iShipper/Diagram_iShipper.png">

## Environment endpoints

### Production
`https://api.asendiahk.com/`

### Test

`https://uat.asendiahk.com/ `


## Add or Update Order

This operation is used to transmit shipment’s data (Address, Service, Weight, Format…) For some destination countries (i.e. United States) entry of customs information is mandatory to enable efficient processing of shipments by local customs authorities.


| Method | Post                              |
|--------|-----------------------------------|
| Route  | openapi/user/1.2/addOrUpdateOrder |

Necessary headers:

| Header       | Value            |
|--------------|------------------|
| Content-Type | application/json |


```shell

>The following code presents i-Shipper request: 

```Shell
curl --location --request POST 'https://api.asendiahk.com/openapi/user/1.2/addOrUpdateOrder' \
--header 'Content-Type: application/json' \
--data-raw '{
    "ApiToken": "kid9sf23h4kliweeurOwef9sdf4",
    "OrderList": [
        {
            "OrderNumber": "VAT0609006",
            "TrackingNo": "",
            "Consignee": "Halem",
            "ServiceType": "EP",
            "Address1": "5 lieu dit parmonet",
            "Address2": "",
            "Address3": "",
            "City": "sdfsdf",
            "State": "SP",
            "CountryCode": "FR",
            "ConsigneePhone": "2432515",
            "CellPhone": "",
            "HouseNumber": "",
            "Zip": "12335",
            "Email": "ksjdf@jslkfj.com",
            "Remark": "",
            "CardId": "123456",
            "ShipperName": "Asendia Hong Kong Limited",
            "ShipperTel": "123456",
            "ShipperCountry": "HK",
            "ShipperState": "HK",
            "ShipperCity": "HK",
            "ShipperZipCode": "13257",
            "ShipperAddress1": "ADP 5/F, Tsuen Wan",
            "ShipperAddress2": "",
            "ShipperEmail": "",
            "SenderVAT": "",
            "Pickup": "N",
            "HasBattery": "N",
            "DutyPaid": "1",
            "ABN": "",
            "GSTExemptionCode": "",
            "LabelBase64": "",
            "InvoiceBase64": "",
            "OtherBase64": "",
            "ChVatNo": "",
            "ChRegisterName": "",
            "CPF": "",
            "PostagePrice": "",
            "PostageCurrency": "",
            "IOSSnumber": "",
            "Currency": "USD",
            "CustomsType": "O",
            "OrderCustoms": [
                {
                    "Description": "1 x Plastic Bottle",
                    "DescriptionCn": "中文货品",
                    "Qty": "1",
                    "Weight": "1",
                    "Value": "10.00",
                    "Length": "",
                    "Width": "",
                    "Height": "",
                    "TaxCode": "",
                    "HsCode": "",
                    "OriginLocationCode": "US"
                },
                {
                    "Description": "1 x Plastic Bottle",
                    "DescriptionCn": "中文货品",
                    "Qty": "1",
                    "Weight": "1",
                    "Value": "10.00",
                    "Length": "",
                    "Width": "",
                    "Height": "",
                    "TaxCode": "",
                    "HsCode": "",
                    "OriginLocationCode": "US"
                }
            ],
            "OrderItems": [
                {
                    "Quantity": "",
                    "Sku": ""
                }
            ]
        }
    ]
}
```

<aside class="success">
I-Shipper json response for	success:
</aside>

The response will be returned from i-Shipper. :

> i-Shipper json Response:

```json
{
  "Result": [
  {
  "ParcelId": "11140951364",
  "Status": "success",
  "ThirdPartyNo": "",
  "OrderId": "87c674a6-b1a8-4262-a84b-28b53bf96b1a",
  "TrackingNo": "LS824629194CH",
  "AsendiaTrackingIdentifier": "ASHK20000000525021",
  "Error": ""
  }
  ],
  "Status": "success",
  "ErrorMessage": ""
  }
```

## Draft Order

This operation is used to draft shipment’s data (Address, Service, Weight, Format…) For some destination countries (i.e. United States) entry of customs information is mandatory to enable efficient processing of shipments by local customs authorities.	The status of this order type operation at the end is to be confirmed.

| Method | Post                        |
|--------|-----------------------------|
| Route  | openapi/user/1.2/draftorder |

Necessary headers:

| Header       | Value            |
|--------------|------------------|
| Content-Type | application/json |

> i-Shipper json raw request:

```json

{"ApiToken":"kid9sf23h4kliweeurOwef9sdf4",
  "OrderList":[
    {
      "OrderNumber": "624506dd9155",
      "ServiceType":"RP",
      "Consignee": "Luis Figueira",
      "Address1": "Rue des Terreaux 4 ",
      "Address2": "Rue des Terreaux 4 ",
      "Address3": " ",
      "City": "Corsier-sur-Vevey",
      "State": "Vaud",
      "CountryCode": "FR",
      "ConsigneePhone": "+852 123456",
      "Zip": "1804",
      "Email":"Email",
      "Qty":"2",
      "ChVatNo":"123456789",
      "ChRegisterName":"Asendia HK",
      "Weight":"0.12",
      "BoxId":"1",
      "Height":"15",
      "Width":"20",
      "Length":"10",
      "HsCode":"hscode",
      "CustomsType":"G",
      "Description":"phone case",
      "Value":"10",
      "Currency":"USD",
      "TaxCode":"123456",
      "CardId":"Z1414141",
      "Pickup":"N",
      "HasBattery":"N",
      "ShipperAddress":"",
      "ShipperTel":"",
      "ShipperZipCode":"",
      "ShipperCity":"",
      "ShipperState":"",
      "ShipperCountry":"",
      "ShipperName":"",
      "Sku":"Sku0001",
      "Quantity":"1",
      "WarehouseCode":"PRINCE"
    }]
}
```
## Confirmed Order

This operation is confirms a drafted order.

| Method | Post                          |
|--------|-------------------------------|
| Route  | openapi/user/1.2/confirmorder |

Necessary headers:

| Header       | Value            |
|--------------|------------------|
| Content-Type | application/json |

> i-Shipper json raw request:

```json

{"ApiToken":"kid9sf23h4kliweeurOwef9sdf4",
"OrderList":[
{"OrderNumber":"PRO00000014"}
]
}
```
