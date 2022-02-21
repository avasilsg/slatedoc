# Asendia Shipping
<aside class="notice">

Asendia Shipping platform, powered by Centiro, is an intuitive web tool that allows you to integrate your orders, create and print labels and manifests for worldwide delivery using several delivery partners integrated in our single shipping tool. In addition to the standard web access, Asendia Shipping can also be connected to your own system via an API (webservice based on SOAP protocol).
Centiro provided WSDL file that contains the different operations that are allowed through API and describes the full request’s structure. The original file can be found in the following web address:

This endpoint deletes a specific kitten.
</aside>
### WSDL endpoints 

#### Authentication
`https://uat.centiro.com/Universe.Services/TMSBasic/Wcf/c1/i1/TMSBasic/Authenticate.svc?wsdl `

#### All other operations

`https://uat.centiro.com/Universe.Services/TMSBasic/Wcf/c1/i1/TMSBasic/TMSBasic.svc?wsdl `

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the shipment to delete

### Shipping Operations

The main goal of the shipment creations will be to cover the new product and services of Asendia Shipping that will be listed below: e-PAQ by Asendia is the new range of international packet and parcels services from Asendia. e-PAQ structure has 3 levels:

- Product: 4 main outbound services and 2 main Returns services
- Mandatory Option: mandatory choice
- Additional Options: non-mandatory choice for e-PAQ Standard and Plus, Format (Boxable or Non boxable) must be transmitted as well

The following table will show all the e-PAQ products as well as the compulsory codes that mediation layer needs to provide in order the request to work:

| Product Name   | Product Code (*) |
|----------------|------------------|
| e-PAQ Standard | EPAQSTD          |
| e-PAQ Plus     | EPAQPLS          |
| e-PAQ Select   | EPAQSCT          |
| e-PAQ Elite    | EPAQELT          |

(*) Unique code to be passed as value for the Attribute Product in the AddAndPrintShipment request

| Error Code | Meaning                                                                    |
|------------|----------------------------------------------------------------------------|
| 400        | Bad Request -- Your request is invalid.                                    |
| 401        | Unauthorized -- Your API key is wrong.                                     |
| 403        | Forbidden -- The kitten requested is hidden for administrators only.       |
| 404        | Not Found -- The specified kitten could not be found.                      |
| 405        | Method Not Allowed -- You tried to access a kitten with an invalid method. |
| 406        | Not Acceptable -- You requested a format that isn't json.                  |

## Authentication

<aside class="success">
Shipping SOAP request for authentication:	
</aside>

The original authentication request is sent from AML to I-Shipping through SOAP. Originally, SOAP uses XML for communication through HTTP. The request for the authentication for SOAP will look like this:

> i-Shipping XML request:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:dat="http://centiro.com/facade/shared/1/0/datacontract" xmlns:ser="http://centiro.com/facade/shared/1/0/servicecontract" xmlns:cen="http://schemas.datacontract.org/2004/07/Centiro.Facade.Common.Operations.Authenticate">
<soapenv:Header>
<dat:MessageId>?</dat:MessageId>
</soapenv:Header>
<soapenv:Body>
<ser:AuthenticateRequest>
<cen:Password>HSDass286@!</cen:Password>
<cen:UserName>wsHSVHand@asendia.de</cen:UserName>
</ser:AuthenticateRequest>
</soapenv:Body>
</soapenv:Envelope>
```

<aside class="success">
Shipping SOAP response for authentication	(success):
</aside>

The response will be returned from i-Shipping platform in XML. If the response is correct, then it will look like this:

> i-Shipping XML Response:

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <h:ErrorMessages i:nil="true" xmlns:h="http://centiro.com/facade/shared/1/0/datacontract" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"/>
    <h:Success xmlns:h="http://centiro.com/facade/shared/1/0/datacontract">true</h:Success>
  </s:Header>
  <s:Body>
    <AuthenticateResponse xmlns="http://centiro.com/facade/shared/1/0/servicecontract" xmlns:a="http://schemas.datacontract.org/2004/07/Centiro.Facade.Common.Operations.Authenticate" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <a:AuthenticationTicket>ERjXNo89J7plBQihaVtn98%2f0AHhU940EJUqW1VAQlToNfcLc9bZ1V4IkE7xxWInZvhBpg3gUbueCJ2rnFW%2bQ3jmgKjfaMENj78z6higZvO%2foaf0EnsornRjB8GWpgpxI%2bbTx8Q2NqHHxwlVof%2fBB3c8cxvyOIzSn</a:AuthenticationTicket>
    </AuthenticateResponse>
  </s:Body>
</s:Envelope>

```
> i-Shipping XML error  response:

<aside class="warning">
Shipping error XML response for authentication:	
</aside>

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <h:ErrorMessages xmlns:h="http://centiro.com/facade/shared/1/0/datacontract" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">Login failed, check username and/or password.</string>
    </h:ErrorMessages>
    <h:Success xmlns:h="http://centiro.com/facade/shared/1/0/datacontract">false</h:Success>
  </s:Header>
  <s:Body>
    <AuthenticateResponse xmlns="http://centiro.com/facade/shared/1/0/servicecontract" xmlns:a="http://schemas.datacontract.org/2004/07/Centiro.Facade.Common.Operations.Authenticate" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <a:AuthenticationTicket/>
    </AuthenticateResponse>
  </s:Body>
</s:Envelope>
```

## Add and Print Shipment

This operation is used to transmit shipment’s data (Address, Service, Weight, Format…) For some destination countries (i.e. United States) entry of customs information is mandatory to enable efficient processing of shipments by local customs authorities.
Response returns a parcel label coded in Base64 format, in PDF, ZPL, PNG or TIFF format to be printed and placed on physical parcel. Recommendation is to avoid a multi-parcels shipment (ie a shipment for a single addressee split into multiple physical boxes) and so to have 1 shipment = 1 parcel, because it is not always offered by final carrier. In some specific cases, response can also return specific documents, also coded in Base64 format, usually in A4 PDF. 


| Error Code | Meaning                                                                    |
|------------|----------------------------------------------------------------------------|
| 400        | Bad Request -- Your request is invalid.                                    |
| 401        | Unauthorized -- Your API key is wrong.                                     |
| 403        | Forbidden -- The kitten requested is hidden for administrators only.       |
| 404        | Not Found -- The specified kitten could not be found.                      |
| 405        | Method Not Allowed -- You tried to access a kitten with an invalid method. |
| 406        | Not Acceptable -- You requested a format that isn't json.                  |


> i-Shipping XML add and print shipment request:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:dat="http://centiro.com/facade/shared/1/0/datacontract" xmlns:ser="http://centiro.com/facade/tmsBasic/1/0/servicecontract" xmlns:cen="http://schemas.datacontract.org/2004/07/Centiro.Facade.TMSBasic.Contract.c1.i1.TMSBasic.BaseTypes.DTO">
  <soapenv:Header>
    <dat:AuthenticationTicket>ERjXNo89J7plBQihaVtn98%2f0AHhU940EJUqW1VAQlToNfcLc9bZ1V4IkE7xxWInZvhBpg3gUbudeNUSVQ39wx1nvsXIHEvs%2fp2ZNhbY81NkCJflVaePbq9WSsDXbyWE00yLzIr5Lr63tpMQPBbV0d2rNPcT5pKNB</dat:AuthenticationTicket>
  </soapenv:Header>
  <soapenv:Body>
    <ser:AddAndPrintShipmentRequest>
      <ser:LabelType>PDF</ser:LabelType>
      <ser:Shipment>
        <cen:Addresses>
          <cen:Address>
            <cen:Address1>Stefan rue des Anciens Combattants d Afn 5</cen:Address1>
            <cen:AddressType>Receiver</cen:AddressType>
            <cen:CellPhone>1234556677</cen:CellPhone>
            <cen:City>Metz</cen:City>
            <cen:Contact>Peter Baer</cen:Contact>
            <cen:Email>duringeranne@gmail.com</cen:Email>
            <cen:ISOCountry>FR</cen:ISOCountry>
            <cen:Name>Stefan</cen:Name>
            <cen:Phone>0782235482</cen:Phone>
            <cen:ZipCode>57000</cen:ZipCode>
          </cen:Address>
        </cen:Addresses>
        <cen:Attributes>
          <cen:Attribute>
            <cen:Code>OriginSub</cen:Code>
            <cen:Value>DE</cen:Value>
          </cen:Attribute>
          <cen:Attribute>
            <cen:Code>CRMID</cen:Code>
            <cen:Value>DE091019</cen:Value>
          </cen:Attribute>
          <cen:Attribute>
            <cen:Code>Product</cen:Code>
            <cen:Value>EPAQPLS</cen:Value>
          </cen:Attribute>
          <cen:Attribute>
            <cen:Code>Service</cen:Code>
            <cen:Value>CUP</cen:Value>
          </cen:Attribute>
          <cen:Attribute>
            <cen:Code>Format</cen:Code>
            <cen:Value>N</cen:Value>
          </cen:Attribute>
          <cen:Attribute>
            <cen:Code>AdditionalService</cen:Code>
            <cen:Value/>
          </cen:Attribute>
          <cen:Attribute>
            <cen:Code>SenderTaxID</cen:Code>
            <cen:Value/>
          </cen:Attribute>
        </cen:Attributes>
        <cen:CreateReturnShipment>false</cen:CreateReturnShipment>
        <cen:Currency>EUR</cen:Currency>
        <cen:ModeOfTransport>ACSS</cen:ModeOfTransport>
        <cen:OrderNumber>HHV3941420</cen:OrderNumber>
        <cen:Parcels>
          <cen:Parcel>
            <cen:OrderLines>
              <cen:OrderLine>
                <cen:CountryOfOrigin>DE</cen:CountryOfOrigin>
                <cen:Currency>EUR</cen:Currency>
                <cen:Description1>ITEM 1</cen:Description1>
                <cen:HarmonizationCode>85238099</cen:HarmonizationCode>
                <cen:OrderLineNumber>1</cen:OrderLineNumber>
                <cen:OrderNumber/>
                <cen:ProductNumber>1234567</cen:ProductNumber>
                <cen:PurchaseOrderLineNumber>1</cen:PurchaseOrderLineNumber>
                <cen:QuantityOrdered>1</cen:QuantityOrdered>
                <cen:QuantityShipped>1</cen:QuantityShipped>
                <cen:UnitOfMeasure>EA</cen:UnitOfMeasure>
                <cen:UnitPrice>13.88</cen:UnitPrice>
                <cen:UnitWeight>0.25</cen:UnitWeight>
                <cen:Weight>0.5</cen:Weight>
              </cen:OrderLine>
            </cen:OrderLines>
            <cen:ParcelIdentifier>HHV_ZK_01</cen:ParcelIdentifier>
            <cen:Value>20</cen:Value>
            <cen:Volume>0</cen:Volume>
            <cen:Weight>0.50</cen:Weight>
            <cen:Width>0</cen:Width>
          </cen:Parcel>
        </cen:Parcels>
        <cen:SenderCode>DE09119</cen:SenderCode>
        <cen:ShipDate>2022-02-07T12:58:47</cen:ShipDate>
        <cen:ShipmentIdentifier>HHV_ZK_01</cen:ShipmentIdentifier>
        <cen:ShipmentType>OUTB</cen:ShipmentType>
      </ser:Shipment>
    </ser:AddAndPrintShipmentRequest>
  </soapenv:Body>
</soapenv:Envelope>
```

## Add master shipment

This operation is used to transmit the list of shipments that customer will deliver to Asendia facility. In the request, all shipments are included into a Master Shipment  Response returns the PDF Customer manifest (coded in Base64) to be printed and delivered to Asendia with physical shipments. Note: Master Shipment is the virtual object that contains all the shipments and customer manifest is the related PDF document
About frequency: it is technically possible to create one Customer manifest per week, per day and even several ones for the same day. So just check with you Account Manager what is the most suitable. 

> add master shipment template

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:dat="http://centiro.com/facade/shared/1/0/datacontract" xmlns:ser="http://centiro.com/facade/tmsBasic/1/0/servicecontract" xmlns:cen="http://schemas.datacontract.org/2004/07/Centiro.Facade.TMSBasic.Contract.c1.i1.TMSBasic.BaseTypes.DTO" xmlns:arr="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
  <soapenv:Header>
    <dat:MessageId>?</dat:MessageId>
    <dat:AuthenticationTicket>?</dat:AuthenticationTicket>
  </soapenv:Header>
  <soapenv:Body>
    <ser:AddMasterShipmentRequest>
      <!--Optional:-->
      <ser:LabelType>?</ser:LabelType>
      <!--Optional:-->
      <ser:MasterShipment>
        <!--Optional:-->
        <cen:Addresses>
          <!--Zero or more repetitions:-->
          <cen:Address>
            <!--Optional:-->
            <cen:Address1>?</cen:Address1>
            <!--Optional:-->
            <cen:Address2>?</cen:Address2>
            <!--Optional:-->
            <cen:Address3>?</cen:Address3>
            <!--Optional:-->
            <cen:AddressType>?</cen:AddressType>
            <!--Optional:-->
            <cen:CellPhone>?</cen:CellPhone>
            <!--Optional:-->
            <cen:City>?</cen:City>
            <!--Optional:-->
            <cen:Contact>?</cen:Contact>
            <!--Optional:-->
            <cen:CustomerNumber>?</cen:CustomerNumber>
            <!--Optional:-->
            <cen:Email>?</cen:Email>
            <!--Optional:-->
            <cen:Fax>?</cen:Fax>
            <!--Optional:-->
            <cen:Freetext1>?</cen:Freetext1>
            <!--Optional:-->
            <cen:Freetext2>?</cen:Freetext2>
            <!--Optional:-->
            <cen:Freetext3>?</cen:Freetext3>
            <!--Optional:-->
            <cen:Freetext4>?</cen:Freetext4>
            <!--Optional:-->
            <cen:ISOCountry>?</cen:ISOCountry>
            <!--Optional:-->
            <cen:Name>?</cen:Name>
            <!--Optional:-->
            <cen:Phone>?</cen:Phone>
            <!--Optional:-->
            <cen:State>?</cen:State>
            <!--Optional:-->
            <cen:VatNo>?</cen:VatNo>
            <!--Optional:-->
            <cen:ZipCode>?</cen:ZipCode>
          </cen:Address>
        </cen:Addresses>
        <!--Optional:-->
        <cen:Attributes>
          <!--Zero or more repetitions:-->
          <cen:Attribute>
            <!--Optional:-->
            <cen:Code>?</cen:Code>
            <!--Optional:-->
            <cen:Value>?</cen:Value>
          </cen:Attribute>
        </cen:Attributes>
        <!--Optional:-->
        <cen:ChildShipmentIdentifiers>
          <!--Zero or more repetitions:-->
          <arr:string>?</arr:string>
        </cen:ChildShipmentIdentifiers>
        <!--Optional:-->
        <cen:DeliveryInstruction>?</cen:DeliveryInstruction>
        <!--Optional:-->
        <cen:LoadingMeasure>?</cen:LoadingMeasure>
        <!--Optional:-->
        <cen:ModeOfTransport>?</cen:ModeOfTransport>
        <!--Optional:-->
        <cen:OrderNumber>?</cen:OrderNumber>
        <!--Optional:-->
        <cen:SenderCode>?</cen:SenderCode>
        <!--Optional:-->
        <cen:SequenceNumber>?</cen:SequenceNumber>
        <!--Optional:-->
        <cen:ShipDate>?</cen:ShipDate>
        <!--Optional:-->
        <cen:ShipmentIdentifier>?</cen:ShipmentIdentifier>
        <!--Optional:-->
        <cen:Value>?</cen:Value>
        <!--Optional:-->
        <cen:Volume>?</cen:Volume>
        <!--Optional:-->
        <cen:Weight>?</cen:Weight>
      </ser:MasterShipment>
    </ser:AddMasterShipmentRequest>
  </soapenv:Body>
</soapenv:Envelope>
```

## Cancel shipment

This operation is used to cancel a shipment before it is manifested to Asendia ie before creation of the customer manifest. 

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:dat="http://centiro.com/facade/shared/1/0/datacontract" xmlns:ser="http://centiro.com/facade/tmsBasic/1/0/servicecontract">
  <soapenv:Header>
    <dat:MessageId>?</dat:MessageId>
    <dat:AuthenticationTicket>?</dat:AuthenticationTicket>
  </soapenv:Header>
  <soapenv:Body>
    <ser:CancelShipmentRequest>
      <!--Optional:-->
      <ser:ShipmentIdentifier>?</ser:ShipmentIdentifier>
      <!--Optional:-->
      <ser:ShipmentType>?</ser:ShipmentType>
    </ser:CancelShipmentRequest>
  </soapenv:Body>
</soapenv:Envelope>
```

## Close shipment

The operation is used when customer wants to manifest the shipments directly to carrier itself (instead of to Asendia Hub). In other words, this operation is an alternative to add master shipment.

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:dat="http://centiro.com/facade/shared/1/0/datacontract" xmlns:ser="http://centiro.com/facade/tmsBasic/1/0/servicecontract" xmlns:arr="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
  <soapenv:Header>
    <dat:MessageId>?</dat:MessageId>
    <dat:AuthenticationTicket>?</dat:AuthenticationTicket>
  </soapenv:Header>
  <soapenv:Body>
    <ser:CloseShipmentsRequest>
      <!--Optional:-->
      <ser:ShipmentIdentifiers>
        <!--Zero or more repetitions:-->
        <arr:string>?</arr:string>
      </ser:ShipmentIdentifiers>
    </ser:CloseShipmentsRequest>
  </soapenv:Body>
</soapenv:Envelope>
```
