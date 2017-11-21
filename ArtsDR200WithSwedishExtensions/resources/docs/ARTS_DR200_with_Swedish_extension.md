

# **_ARTS DR version 200 with swedish extension_**

# Reference

<table>
  <tr>
    <td>Version</td>
    <td>Datum</td>
    <td>Ändrad av</td>
    <td>Ändring</td>
  </tr>
  <tr>
    <td>0.10</td>
    <td>2017-08-13</td>
    <td>Lars Bergström</td>
    <td>Initial draft</td>
  </tr>
  <tr>
    <td>0.11</td>
    <td>2017-09-08</td>
    <td>Lars Bergström</td>
    <td>updated business unit and customer sections</td>
  </tr>
  <tr>
    <td>0.12</td>
    <td>2017-11-21</td>
    <td>Lars Bergström</td>
    <td>Converted to markdown from google doc format</td>
  </tr>  
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>


#Table of content
1. TODO ADD...

## Introduction <a name="introduction"></a>

The purpose of this document is to explain the structure of an ARTS DR-200 XML based receipt with swedish standard extensions. 

The ARTS XML schema, DigitalReceiptV2.0.0.xsd is flexible and the compulsory components are few. To conform to swedish tax legislations some of the optional ARTS components and in some cases swedish standard extensions made to the ARTS DR-200 schema are required. All components/extensions needed to conform to swedish tax legislations will be described in this document. All extensions can be found in the proposed swedish standard extension schema SweArtsDR200ExtV1.0.0.xsd.

This document is part of the Swedish digital receipt standard package. This package also contains sample receipts, the ARTS DR 200 XML schema and the swedish standard extension XML schema used for receipt validation and construction.

## Recommended reading <a name="recommended-reading"></a>

This document focuses on details of the digital receipt XML structure. To simplify the understanding of the content of this document the following resources should be studied alongside:
* Sample receipts and schemas - LOCATION AND CONTENT TO BE DEFINED.
* NRF/ARTS  documentation (schemas, uses cases etc)
[https://nrf.com/resources/retail-library/digital-receipt-integration-standard-30](https://nrf.com/resources/retail-library/digital-receipt-integration-standard-30)
* Swedish standard documentation and uses cases - TO BE DEFINED.

## Scope

The swedish digital receipt standard package focuses on the construction and content of the digital receipt. It does not describe how the receipt is transferred from the issuer to the customer and how it is presented.

## Terminology

### NRF

NRF - National Retail Foundation, is the world’s largest retail trade association. Manages the ARTS Standards.

For more information on NRF: [https://nrf.com](https://nrf.com)

### OMG

OMG - Object management group, will manage the ARTS retails standard schemas together with NRF from mid 2017. 

For more information on retail schemas: [http://www.omg.org/retail/](http://www.omg.org/retail/)

### ARTS

ARTS - The Association for Retail Technology Standards of the National Retail Federation is a retailer-driven membership organization dedicated to creating an open environment where both retailers and technology vendors work together to create international retail technology standards.

For more information on ARTS:[ http://www.nrf-arts.org/](http://www.nrf-arts.org/)

### ARTS-DR-SE

ARTS-DR-SE will be used in this document to reference the ARTS DR200 standard with Swedish extension.

### POS

POS - Point of Sales, the system creating and delivering receipts to the customer. The term POS in this document can be any system creating receipt data.

### XML (Extensible Markup Language)

[w3c standard](http://www.w3.org/XML). XML is a set of rules for encoding documents in machine-readable form.  (Wikipedia)

### XSD (XML Schema Definition)

[w3c standard](http://www.w3.org/XML/Schema). An XML schema is a description of a type of XML document, typically expressed in terms of constraints on the structure and content of documents of that type, above and beyond the basic syntactical constraints imposed by XML itself. (Wikipedia)

## Digital receipts - ARTS

This section covers the most common ARTS terms and their corresponding POS data terms where relevant. For more information see the documentation in the ARTS schema specification.

**NOTE!** The relationship between the ARTS elements (terms) is not always obvious when reading this document so we recommend that you have sample xml receipts, the ARTS DR200 XML schema and the swedish standard extension XML schema available.

### DigitalReceipt

Root element, includes the namespace for all terms (elements, attributes) used within a receipt. If the ARTS schema is extended, the extension namespace must be declared as well.

The attribute **copy** has to be provided with either **true** or **false** value. This indicates if the digital receipt is a copy or an original. If a paper receipt is printed as the original receipt a digital copy should be sent with copy="true” flag.

Example:
```xml
<nam:DigitalReceipt MajorVersion="6" MinorVersion="0" FixVersion="0" se:copy="true" 
                    xmlns:nam="http://www.nrf-arts.org/IXRetail/namespace/"
                    xmlns:se="http://**www.swestandard.se**/schemas/namespace/se"
                    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                    xsi:schemaLocation="http://www.nrf-arts.org/IXRetail/namespace/ http://www.nrf-arts.org/IXRetail/namespace/DigitalReceiptV2.0.0.xsd
                    http://www.swestandard.se/schemas/namespace/se http://www.swestandard.se/schemas/dr/SweARTS200DRExtensionsV1.0.0.xsd">
```

#### Transaction

The *DigitalReceipt* element can contain multiple transactions. The way a *Transaction* is defined it can contain one or more *RetailTransaction/CustomerOrderTransaction* from one and the same sales company, POS (*WorkstationID)* and operator (*OperatorID)*. A *Transaction *is basically a single receipt.  

This document describes single transaction receipts.

#### HeaderText - Swedish standard extension

*HeaderText* is an arbitrary text field (one or more text lines), could contain information like:
```
<se:HeaderText>
    <se:Text>Open Mon - Fri 10 - 20 Sat 10 - 17, Sun 11 - 17</se:Text>
</se:HeaderText>
```

The *HeaderText* is displayed in the receipt header.

#### BusinessUnit

The ARTS element *BusinessUnit* contains information about the sales company. The ARTS *BusinessUnit* is not extendable. 

ARTS-DR-SE defines an extension to *BusinessUnit* to enable:	
* The use of *organisationsnummer* to identify the company.
* The use of a digital receipt service provider ID if used by the company.
* Future extensions to the *BusinessUnit* content.

Example:

```xml
<se:BusinessUnit>
    <UnitID Name="MyShop">b-100</UnitID>
    <Address>
        <AddressLine TypeCode="Street">Storgatan 11</AddressLine>
        <City>Metropolis</City>
        <PostalCode>10000</PostalCode>
        <Country Code="SE">Sverige</Country>
    </Address>
    <Telephone TypeCode="Work">
        <AreaCode>0771</AreaCode>
        <LocalNumber>584886</LocalNumber>
        <ITUCountryCode>46</ITUCountryCode>
    </Telephone>
    <Website>http://www.myshop.com</Website>
    <se:Identifier id="5922f97bfeb148d6b9a6ef76f76ebdce” idType=”ServiceID”>
        <se:OrganisationID>9999999999</se:OrganisationID>
    </se:Identifier>
</se:BusinessUnit>
```

#### Customer

The ARTS *Customer* element contains information about the customer. The ARTS Customer element can be used as is if it is enough to identify the customer using email or telephone number. ARTS-DR-SE defines an extension to the ARTS *Customer *element if other customer identification mechanisms are needed .

Example:
```
<se:Customer>
	…
	<TelephoneNumber>
   		<LocalNumber>tel:+46-0700-123456</LocalNumber>
   	</TelephoneNumber>
    <EMail>
   		<EMailAddress>first.last@email.com</EMailAddress>
   	</EMail>
	<se:Identifier Type="ServiceID”>400000000010</se:Identifier>
</se:Customer>
```

Custom types can be added to the *CustomerID* element. Format must conform to:
```
    <xs:simpleType name="IXREnumerationExtension">
        <xs:restriction base="xs:string">
            <xs:pattern value="[0-9A-Za-z][0-9A-Za-z]*:[A-Z][0-9A-Za-z]*"/>
        </xs:restriction>
    </xs:simpleType>
```
Example:
```xml
<se:CustomerID Type="MyCompany:IdentifierType">123456</se:CustomerID>
```
#### ReceiptCode - Swedish standard extension

To simplify the transition from paper to digital receipts ARTS-DR-SE defines the element *ReceiptCode*. 

Use case:
The POS produces a digital receipt with a *ReceiptCode* and a paper receipt with the same *ReceiptCode* is handed to the customer. The customer can use the printed *ReceiptCode *to pick up the digital receipt later on from the sales company and/or a digital receipt service provider. 

Example:
```xml
<se:ReceiptCode>1200961100705490</se:ReceiptCode>
```

The *ReceiptCode* can also be used to connect a sales receipt with a return receipt. See [return references](#heading=h.r9lmnancrnyc).

#### WorkstationID

*WorkstationID* identifies the system (POS/ECR) creating the receipt.

Example:
```xml
<WorkstationID SerialNumber="EA38329" Mode="Retail" TypeCode="POS">65252</WorkstationID>
```
#### OperatorID

Information about the operator..

Example:
```xml
<OperatorID OperatorType="Cashier" OperatorName="Olle">SKOP-123A</OperatorID>
```
#### CurrencyCode

Currency code can be specified in several places and the receipt data can contain a mix of currencies. This element gives the default currency code for all elements where currency is applicable but with no specific currency code given.

Example:
```xml
<CurrencyCode>EUR</CurrencyCode>
```
#### TrailerText

Arbitrary text data that will be displayed at the end of the receipt.

Example:
```
<TrailerText>
    <Text>Spara kvittot, gäller som garanti.</Text>
    <Text>Öppet köp 30 dagar.</Text>
</TrailerText>
```
The *TrailerText* is the only type of text element defined in ARTS DR200. The swedish extension makes it possible to add a header text and line item texts.

#### ReceiptDateTime

Date and time when the transaction was made.

Example:
```xml
<ReceiptDateTime>2012-01-11T09:49:00+01:00</ReceiptDateTime>
```
**NOTE!** It is important that the date/time format is based on the DateTime type (xs:dateTime) used in ARTS and be aware of how it changes with daylight saving time.

#### ReceiptNumber

If the receipt number *TypeCode* identifies a barcode, the barcode can be displayed on the digital receipt. The supported formats are *Interleaved2Of5, EAN13, EAN8, Codabar, Code39, Code128, PDF417, UPC-A, UPC-E*.

Example:
```
<ReceiptNumber TypeCode="EAN13">5060033952504</ReceiptNumber>
```
#### RetailTransaction

The *RetailTransaction* element contains detailed information about the order, sales etc.

The next sections will describe the sub elements of the *RetailTransaction* elements. The *Customer* and *LoyaltyAccount* elements (part of *RetailTransaction*) have been described earlier in this document.

See the ARTS DR200 schema for details and information about other elements.

##### LineItem

Each *LineItem* contains payment info, tax info, article info etc. Each *LineItem* must have a unique *SequenceNumber* element. All *LineItems* kan contain the *href* and *imagesrc* attributes. These attributes can be used to add product links and product images to the receipt.

###### LineItem - Sale

Example sale article:
```
<LineItem se:href="http://myshop.foo/item/608794" se:imgsrc="http://myshop.foo/item/608794/image.png">
    <Sale ItemType="Stock" TaxableFlag="true" ReturnableFlag="true">
      	<ItemID Type="GTIN">17311310026612</ItemID>
        <POSIdentity>
            <POSItemID>608794</POSItemID>
        </POSIdentity>
        <POSIdentity POSIDType="mainGroup”>
            <POSItemID>010300</POSItemID>
            <Qualifier>bil-mc</Qualifier>
        </POSIdentity>
        <POSIdentity POSIDType="articleGroup”>
            <POSItemID>010303</POSItemID>
            <Qualifier>säkerhetsdetaljer</Qualifier>
        </POSIdentity>
        <Description>Bogserlina med krokar</Description>
        <UnitCostPrice Currency="SEK" Quantity="1" UnitOfMeasureCode="EA">79.00</UnitCostPrice>
        <ActualSalesUnitPrice Currency="SEK" Action="Add">69.00</ActualSalesUnitPrice>
        <ExtendedAmount Currency="SEK" Action="Add">138.00</ExtendedAmount>
        <DiscountAmount>10</DiscountAmount>
        <ExtendedDiscountAmount>20</ExtendedDiscountAmount>
        <Quantity Units="1" UnitOfMeasureCode="EA">2</Quantity>
        <Tax>
            <Percent>25</Percent>
        </Tax>
        <se:Reminder TypeCode="Warranty">
            <se:Description>Din garanti går ut om 2 veckor. Kontaka oss i butiken för att förlänga din garanti.</se:Description>
            <se:ReminderDate>2014-03-16T16:10:00+01:00</se:ReminderDate>
        </se:Reminder>
    </Sale>
    <SequenceNumber>1</SequenceNumber>
</LineItem>
```
There are several ways to identify a sale item:
* POSIdentity (*POSItemID*) - unqualified (No *POSIDType* given)
* POSIdentity (*POSItemID* and *Qualifier*) - qualified with *POSIDType*. Supported *POSIDTypes*: 
    * mainGroup
    * articleGroup
    * supplier
    * brand
* ItemID - qualified with *Type. *Supported  *Types:*
    * GTIN
* Description

The following elements should be displayed when presenting the content of a digital receipt:
* Identifier by choice of supplier
* Description
* Quantity
* ActualSalesUnitPrice
* ExtendedAmount
* ExtendedDiscountAmount
* Tax

###### LineItem - Return

Example return article:
```xml
<LineItem>
    <Return>
        <ItemID Type="GTIN">17311310026612</ItemID>
        <POSIdentity>
            <POSItemID>923048</POSItemID>
        <POSIdentity>
        <POSIdentity POSIDType="mainGroup”>
            <POSItemID>070700</POSItemID>
            <Qualifier>kontor-teknik</Qualifier>
        </POSIdentity>
	    <POSIdentity POSIDType="articleGroup”>
            <POSItemID>070773</POSItemID>
            <Qualifier>hörlurar</Qualifier>
        </POSIdentity>
        <Description>Headset hörlurar - SLR</Description>
        <UnitCostPrice Currency="SEK" Quantity="1" UnitOfMeasureCode="EA">49.00</UnitCostPrice>
        <ActualSalesUnitPrice Currency="SEK">49.00</ActualSalesUnitPrice>
        <ExtendedAmount Currency="SEK">49.00</ExtendedAmount>
        <Quantity Units="1" UnitOfMeasureCode="EA">1</Quantity>
        <TransactionLink>
            <TransactionID>cd5d49f0-56e8-11e1-b86c-0800200c9a66</**TransactionID**>
        </TransactionLink>
        <se:ReceiptCodeLink>1210961120788496</se:ReceiptCodeLink>
        <Reason>NA</Reason>
    </Return>
    <SequenceNumber>1</SequenceNumber>
</LineItem>
```

The *TransactionLink* element is what ARTS uses to reference the original (sales) receipt. This *TransactionLink**/TransactionID* is based on a company internal reference, for companies using ARTS. The swedish extension defines an extension to ARTS, the *ReceiptCodeLink**, *to have a reference that works outside company borders. The *ReceiptCodeLink* refers to the sale receipts *ReceiptCode* element ([described earlier in this document](#heading=h.8waduk8c0qyh)). 

As the *ReceiptCode* can displayed on the digital sales receipt, both as barcode and characters, it it easy for the cashier to add this as a sales receipt reference on the return receipt. Use either *ReceiptCodeLink *or *TransactionLink*, both are not needed.

It is also important that the *Return *element contains item identifiers and number of items returned to make it possible to determine which item(s) from the sales receipt that has been returned.

For fields displayed on the digital receipt see the *LineItem - Sale* section above.

###### LineItem - Tax

This element will give the total tax amount for the given tax level.

Example tax info:
```
<LineItem>
    <Tax>
        <TaxableAmount Currency="SEK" TaxIncludedInTaxableAmountFlag="false">446.80</TaxableAmount>
        <Amount Currency="SEK">111.70</Amount>
        <Percent>25</Percent>
    </Tax>
    <SequenceNumber>5</SequenceNumber>
</LineItem>
```
Total tax for each tax level should be displayed on the receipt.

###### LineItem - Tender

Example tender info:
```
<LineItem>
    <Tender TypeCode="Sale" TenderType="Cash">
        <Amount Currency="SEK">55.50</Amount>
        <AmountAppliedToBill Currency="SEK">55.00</AmountAppliedToBill>
        <Rounding Currency="SEK">0.50</Rounding>
    </Tender>
    <SequenceNumber>7</SequenceNumber>
</LineItem>
```
Tender information should be displayed on the receipt.

###### LineItem - Tender related

There are several payment related *LineItems* like:
* TenderChange
* Deposit
* GiftCertificate
* Rebate
* etc

They should all be displayed on the digital receipt.

If a debit/credit card is used for payment and the payment terminal is integrated with the POS, the payment terminal information can be added to the digital receipt. This way there is no need to print the payment slip on paper. The *PaymentTerminal* element is a swedish standard extension.

Example:
```
<nam:LineItem>
    <nam:Tender TypeCode="Sale" TenderType="Cash">
        <nam:Amount Currency="SEK">55.50</nam:Amount>
	        ...
        <nam:CreditDebit CardType="Debit">
            <nam:IssuerIdentificationNumber>11</nam:IssuerIdentificationNumber>
            <nam:PrimaryAccountNumber>xxxxxxxxxxxx1234</nam:PrimaryAccountNumber>
            <nam:CreditCardCompanyCode>123456</nam:CreditCardCompanyCode>
        </nam:CreditDebit>
        <se:PaymentTerminal>
            <se:Text>Personlig kod</se:Text>
            <se:Text>XXXXXXXXXXXX1234</se:Text>
            <se:Text>VISA</se:Text>
            <se:Text Name="Term:">27605R101-154734</se:Text>
            <se:Text Name="IB1:">Butiksnr. 120006</se:Text>
            <se:Text/>
            <se:Text Name="ATC:">00038</se:Text>
            <se:Text Name="AID:">A0000000041010</se:Text>
            <se:Text Name="ARC:">00 STATUS:0000</se:Text>
            <se:Text Name="AUT.KOD:">319318</se:Text>
            <se:Text Name="REF:">154734</se:Text>
            <se:Text>TVR: 00 80 00 80 00</se:Text>
        </se:PaymentTerminal>
    </nam:Tender>
    <nam:SequenceNumber>7</nam:SequenceNumber>
</nam:LineItem>
```
###### LineItem - Text (Swedish standard extension)

The swedish extension supports text only line items. The text row(s) should be displayed on the receipt.

Example:
```
<nam:LineItem>
    <se:ItemText>
        <se:Text>text row 1</se:Text>
        <se:Text>text row 2</se:Text>
        <se:Text>etc...</se:Text>
    </se:ItemText>
    <nam:SequenceNumber>8</nam:SequenceNumber>
</nam:LineItem>
```
The *ItemText* can also be added to an other type of line item like sale, tender etc. In this case the text should be displayed after the other line item information.

##### Total

There are number of different total types, the default total type is *TransactionGrossAmount.* The *TransactionGrossAmount* is considered receipt total. 

All total types should be displayed on the receipt.

Example:

<Total TotalType="TransactionGrandAmount" CurrencyCode="SEK">558.00</Total>

##### Attachment - Swedish standard extension

Base64 encoded binary files or external viewable resources can be attached to the receipt. This could be:
* Insurance documents
* Warranty documents
* Assembly instructions
* Care instructions
* etc

If the "attachment information" is not to comprehensive it can be added as LineItem/ItemText elements instead of adding an attachment element.

Example:
```
<Attachment>
    <Name>Warranty</Name>
    <Description>Warranty terms - Headset</Description>
    <DisplayPeriod>
       <FromDate>2013-02-25</FromDate>
       <ToDate>2014-02-25</ToDate>
    </DisplayPeriod>
    <ValidPeriod>
       <FromDate>2013-02-25</FromDate>
       <ToDate>2013-03-25</ToDate>
    </ValidPeriod>
    <Image TypeCode="PDF">B64ENCODED WARRANTY ATTACHMENT</Image>
    <ItemLink>1</ItemLink>
</Attachment>
<Attachment>
   <Name>Washing instructions</Name>
   <Description>Video, how to take care of your new garment</Description>
   <URL>http://link.to.some.video.file</URL>
</Attachment>
```
Compulsory elements:
* *Name*: will be displayed as the attachment name in the digital receipt view.
* One of *Image* or *URL*:
    * *Image*: The base64 encoded attachment. Supported attachment types (*TypeCode*):
        * PDF
        * JPG
        * PNG
    * *URL*: URL to web-viewable content (html, movie, etc)
Non-compulsory elements:
* *Description*: Text giving further information on the attachment content.
* *ValidPeriod*: The start/end date, if the attachment is valid for a limited period.
* *DisplayPeriod*: The start/end date, if the attachment will be displayed for a limited period.
* *ItemLink*: Link to the sale item sequence number if the attachment is connected to a specific item.

The attachment element should added at the end of the RetailTransaction, example:
```
<DigitalReceipt>
    …
    …
   <RetailTransaction>
       …
       <Attachment>
           <Name>Warranty</Name>
           …
      </Attachment>
       <Attachment>
           <Name>Assembly instructions</Name>
           …
      </Attachment>
   <RetailTransaction>
…
<DigitalReceipt>
```
### Control unit information - Swedish standard extension

In some countries/markets (like Sweden) a control unit is connected to the POS. Information from this unit is (can be) printed on the paper receipt. To add this information to the digital receipt use the *ControlUnit* element. Example:
```
<DigitalReceipt>
    ...
    ...
   <se:ControlUnit>
       <se:Text Name="Kontrollenhetens tillverkningsnummer:">RIHTT102001055784</se:Text>
       <se:Text Name="Löpnummer för kontrollbox:">66262</se:Text>
   </se:ControlUnit>
</DigitalReceipt>
```
