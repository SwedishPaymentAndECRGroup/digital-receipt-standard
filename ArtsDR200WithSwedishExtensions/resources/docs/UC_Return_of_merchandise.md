# **_Use Case: A customer wants to return some merchandise bought_**

This document describes some alternative scenarios based on this use case. The scenarios depends on the capabilities of the POS, merchant backend and digital receipt provider systems used.

The document does not describe any internal routines the merchant has, like signing receipts etc upon repurchase. 
## Scenario <a name="scenario"></a>
A customer wants to return some merchandise bought. The customer has a digital sales receipt. The customer will receive a digital return receipt when the return is made.   


### Brief descrition <a name="scenario-brief-description"></a>

A customer enters a store to return some merchandise bought. To prove the purchase the customer has a digital sales receipt.

### Main scenario<a name="main-scenario"></a>

The merchant can verify the purchase using the customers digital sales receipt via the receipt number or receipt code. The merchant POS can produce a return receipt connected to the sales receipt.
The customer uses a digital receipt service able to visualize returned items using the sales and return receipts. 

1. The customer enters the store with the merchandise to return and a digital receipt to prove the purchase.
2. The customer hands over the merchandise to the operator. 
3. The operator scans the digital sales receipts (*receipt code* or *receipt number*, both barcodes) to verify the purchase.
4. The operator registers the merchandise returned, returns the money paid. 
5. A digital return receipt is created and is sent to the customer.
6. The sales and return receipt will be connected, meaning the customers original sales receipt will show that the merchandise has been returned.

#### Data
The receipt samples below only shows the parts of the receipt relevant to the return. 
#### Sales receipt
```xml
<nam:DigitalReceipt...">
    <nam:Transaction>
    ...
        <nam:ReceiptNumber TypeCode="Interleaved2Of5">02890093672714</nam:ReceiptNumber>
        <nam:RetailTransaction>
            <nam:ItemCount>7</nam:ItemCount>
            ...
            <nam:LineItem>
                <nam:Sale ItemType="Stock" TaxableFlag="true" ReturnableFlag="true">
                    <nam:POSIdentity>
                        <nam:POSItemID>923048</nam:POSItemID>
                    </nam:POSIdentity>
                    <nam:POSIdentity POSIDType="mainGroup">
                        <nam:POSItemID>070700</nam:POSItemID>
                        <nam:Qualifier>kontor-teknik</nam:Qualifier>
                    </nam:POSIdentity>
                    <nam:POSIdentity POSIDType="articleGroup">
                        <nam:POSItemID>0870773</nam:POSItemID>
                        <nam:Qualifier>hörlurar</nam:Qualifier>
                    </nam:POSIdentity>
                    <nam:Description>Headset hörlurar - SLR</nam:Description>
                    <nam:UnitCostPrice Currency="SEK" Quantity="1" UnitOfMeasureCode="EA">49.00</nam:UnitCostPrice>
                    <nam:ActualSalesUnitPrice Currency="SEK" Action="Add">49.00</nam:ActualSalesUnitPrice>
                    <nam:ExtendedAmount Currency="SEK" Action="Add">49.00</nam:ExtendedAmount>
                    <nam:Quantity Units="1" UnitOfMeasureCode="EA">1</nam:Quantity>
                    <nam:Tax>
                        <nam:Percent>25</nam:Percent>
                    </nam:Tax>
                </nam:Sale>
                <nam:SequenceNumber>2</nam:SequenceNumber>
            </nam:LineItem>
            ...
        </nam:RetailTransaction>
        <se:ReceiptCode>1281111260543493</se:ReceiptCode>
    </nam:Transaction>
</nam:DigitalReceipt>                                
```

#### Return receipt
```xml
<nam:DigitalReceipt...">
    <nam:Transaction>
    ...
        <nam:ReceiptDateTime>2017-12-05T21:52:33+01:00</nam:ReceiptDateTime>
        <nam:ReceiptNumber TypeCode="Interleaved2Of5">02890093684021</nam:ReceiptNumber>
        <nam:RetailTransaction>
            <nam:ItemCount>3</nam:ItemCount>
            <nam:LineItem>
                <nam:Return>
                    <nam:POSIdentity>
                        <nam:POSItemID>923048</nam:POSItemID>
                    </nam:POSIdentity>
                    <nam:Description>Headset hörlurar - SLR</nam:Description>
                    <nam:UnitCostPrice Currency="SEK" Quantity="1" UnitOfMeasureCode="EA">49.00</nam:UnitCostPrice>
                    <nam:ActualSalesUnitPrice Currency="SEK">49.00</nam:ActualSalesUnitPrice>
                    <nam:ExtendedAmount Currency="SEK">49.00</nam:ExtendedAmount>
                    <nam:Quantity Units="1" UnitOfMeasureCode="EA">1</nam:Quantity>
                    <nam:Tax>
                        <nam:Percent>25</nam:Percent>
                    </nam:Tax>
                    <nam:TransactionLink>
                        <nam:TransactionID>02890093672714</nam:TransactionID>
                    </nam:TransactionLink>
                    <se:ReceiptCodeLink>1281111260543493</se:ReceiptCodeLink>
                    <nam:Reason>NA</nam:Reason>
                </nam:Return>
                <nam:SequenceNumber>1</nam:SequenceNumber>
            </nam:LineItem>
            <nam:LineItem>
                <nam:Tax>
                    <nam:TaxableAmount Currency="SEK" TaxIncludedInTaxableAmountFlag="false">39.20</nam:TaxableAmount>
                    <nam:Amount Currency="SEK">9.80</nam:Amount>
                    <nam:Percent>25</nam:Percent>
                </nam:Tax>
                <nam:SequenceNumber>2</nam:SequenceNumber>
            </nam:LineItem>
            <nam:LineItem>
                <nam:Tender TypeCode="Refund" TenderType="Cash">
                    <nam:Amount Currency="SEK">49.00</nam:Amount>
                    <se:Text>Åter</se:Text>
                </nam:Tender>
                <nam:SequenceNumber>3</nam:SequenceNumber>
            </nam:LineItem>
            <nam:Total TotalType="TransactionGrandAmount" TypeCode="Return">49.00</nam:Total>            
            ...
        </nam:RetailTransaction>
        <se:ReceiptCode>1202151240570529</se:ReceiptCode>
    </nam:Transaction>
</nam:DigitalReceipt>                                
```
In this example the sales and return receipt are connected by the *TransactionLink* and the *ReceiptCodeLink*. 
Only one of them is needed. Which one to use depends on POS cababilities. *TransactionLink* is the standard ARTS way.
The *TransactionLink* can be built in different ways. See the ARTS DR schema. The *ReceiptCodeLink* is a Swedish extension that might be easier to use depending on POS and merchant backend capabilities.

### Alternative scenario - one<a name="alternative-scenario-one"></a>
The merchant has no system (POS or backend) that can verify the purchase. However it uses a digital receipt provider 
with the capability to verify the receipt and to connect the sales and return receipt.   
The merchant POS can not produce a return receipt connected to the sales receipt.
The customer uses a digital receipt service able to visualize returned items using the sales and return receipts. 

1. The customer enters the store with the merchandise to return and a digital receipt to prove the purchase.
2. The customer hands over the merchandise to the operator. 
3. The operator scans the digital sales receipts *receipt code* (barcode) to verify the purchase.
  * The merchant POS sends a request to the digital receipt provider to verify the purchase.  
  * The digital receipt provider verifies/returns information about the purchase including a transaction id.
4. The operator registers the merchandise returned, returns the money paid. 
5. A digital return receipt is created and is sent to the customer.

#### Data
The receipt samples below only shows the parts of the receipt relevant to the return. 
#### Sales receipt
```xml
<nam:DigitalReceipt...">
    <nam:Transaction>
    ...
        <nam:ReceiptNumber TypeCode="Interleaved2Of5">02890093672714</nam:ReceiptNumber>
        <nam:RetailTransaction>
            <nam:ItemCount>7</nam:ItemCount>
            ...
            <nam:LineItem>
                <nam:Sale ItemType="Stock" TaxableFlag="true" ReturnableFlag="true">
                    <nam:POSIdentity>
                        <nam:POSItemID>923048</nam:POSItemID>
                    </nam:POSIdentity>
                    <nam:POSIdentity POSIDType="mainGroup">
                        <nam:POSItemID>070700</nam:POSItemID>
                        <nam:Qualifier>kontor-teknik</nam:Qualifier>
                    </nam:POSIdentity>
                    <nam:POSIdentity POSIDType="articleGroup">
                        <nam:POSItemID>0870773</nam:POSItemID>
                        <nam:Qualifier>hörlurar</nam:Qualifier>
                    </nam:POSIdentity>
                    <nam:Description>Headset hörlurar - SLR</nam:Description>
                    <nam:UnitCostPrice Currency="SEK" Quantity="1" UnitOfMeasureCode="EA">49.00</nam:UnitCostPrice>
                    <nam:ActualSalesUnitPrice Currency="SEK" Action="Add">49.00</nam:ActualSalesUnitPrice>
                    <nam:ExtendedAmount Currency="SEK" Action="Add">49.00</nam:ExtendedAmount>
                    <nam:Quantity Units="1" UnitOfMeasureCode="EA">1</nam:Quantity>
                    <nam:Tax>
                        <nam:Percent>25</nam:Percent>
                    </nam:Tax>
                </nam:Sale>
                <nam:SequenceNumber>2</nam:SequenceNumber>
            </nam:LineItem>
            ...
        </nam:RetailTransaction>
        <se:ReceiptCode>1281111260543493</se:ReceiptCode>
    </nam:Transaction>
</nam:DigitalReceipt>                                
```

#### Return receipt
```xml
<nam:DigitalReceipt...">
    <nam:Transaction>
    ...
        <nam:ReceiptDateTime>2017-12-05T21:52:33+01:00</nam:ReceiptDateTime>
        <nam:ReceiptNumber TypeCode="Interleaved2Of5">02890093684021</nam:ReceiptNumber>
        <nam:RetailTransaction>
            <nam:ItemCount>3</nam:ItemCount>
            <nam:LineItem>
                <nam:Return>
                    <nam:POSIdentity>
                        <nam:POSItemID>923048</nam:POSItemID>
                    </nam:POSIdentity>
                    <nam:Description>Headset hörlurar - SLR</nam:Description>
                    <nam:UnitCostPrice Currency="SEK" Quantity="1" UnitOfMeasureCode="EA">49.00</nam:UnitCostPrice>
                    <nam:ActualSalesUnitPrice Currency="SEK">49.00</nam:ActualSalesUnitPrice>
                    <nam:ExtendedAmount Currency="SEK">49.00</nam:ExtendedAmount>
                    <nam:Quantity Units="1" UnitOfMeasureCode="EA">1</nam:Quantity>
                    <nam:Tax>
                        <nam:Percent>25</nam:Percent>
                    </nam:Tax>
                    <nam:TransactionLink>
                        <nam:TransactionID>cd5d49f0-56e8-11e1-b86c-0800200c9a66</nam:TransactionID>
                    </nam:TransactionLink>
                    <nam:Reason>NA</nam:Reason>
                </nam:Return>
                <nam:SequenceNumber>1</nam:SequenceNumber>
            </nam:LineItem>
            <nam:LineItem>
                <nam:Tax>
                    <nam:TaxableAmount Currency="SEK" TaxIncludedInTaxableAmountFlag="false">39.20</nam:TaxableAmount>
                    <nam:Amount Currency="SEK">9.80</nam:Amount>
                    <nam:Percent>25</nam:Percent>
                </nam:Tax>
                <nam:SequenceNumber>2</nam:SequenceNumber>
            </nam:LineItem>
            <nam:LineItem>
                <nam:Tender TypeCode="Refund" TenderType="Cash">
                    <nam:Amount Currency="SEK">49.00</nam:Amount>
                    <se:Text>Åter</se:Text>
                </nam:Tender>
                <nam:SequenceNumber>3</nam:SequenceNumber>
            </nam:LineItem>
            <nam:Total TotalType="TransactionGrandAmount" TypeCode="Return">49.00</nam:Total>            
            ...
        </nam:RetailTransaction>
        <se:ReceiptCode>1202151240570529</se:ReceiptCode>
    </nam:Transaction>
</nam:DigitalReceipt>                                
```
In this example the link between the sales and return receipt is done by the *TransactionLink*. *TransactionLink* is the standard ARTS way.
The *TransactionLink* can be built in different ways. See the ARTS DR schema. In this case the *TransationID* used in the *TransactionLink* is provided by the digital receipt provider.

### Alternative scenario - two<a name="alternative-scenario-two"></a>

The merchant has no system that can verify the purchase. 
The customers show the digital sales receipt to verify the purchase. 
The merchant POS can not produce a return receipt connected to the sales receipt.
The customer will end up with one sales receipt and one return receipt without connection. 

1. The customer enters the store with the merchandise to return and a digital receipt to prove the purchase.
2. The customer hands over the merchandise to the operator. 
3. The customer shows the digital receipt to verify the purchase.
4. The operator registers the merchandise returned, returns the money paid. 
5. A digital return receipt is created and is sent to the customer.

#### Data
The receipt samples below only shows the parts of the receipt relevant to the return. 
#### Sales receipt
```xml
<nam:DigitalReceipt...">
    <nam:Transaction>
    ...
        <nam:RetailTransaction>
            <nam:ItemCount>7</nam:ItemCount>
            ...
            <nam:LineItem>
                <nam:Sale ItemType="Stock" TaxableFlag="true" ReturnableFlag="true">
                    <nam:POSIdentity>
                        <nam:POSItemID>923048</nam:POSItemID>
                    </nam:POSIdentity>
                    <nam:POSIdentity POSIDType="mainGroup">
                        <nam:POSItemID>070700</nam:POSItemID>
                        <nam:Qualifier>kontor-teknik</nam:Qualifier>
                    </nam:POSIdentity>
                    <nam:POSIdentity POSIDType="articleGroup">
                        <nam:POSItemID>0870773</nam:POSItemID>
                        <nam:Qualifier>hörlurar</nam:Qualifier>
                    </nam:POSIdentity>
                    <nam:Description>Headset hörlurar - SLR</nam:Description>
                    <nam:UnitCostPrice Currency="SEK" Quantity="1" UnitOfMeasureCode="EA">49.00</nam:UnitCostPrice>
                    <nam:ActualSalesUnitPrice Currency="SEK" Action="Add">49.00</nam:ActualSalesUnitPrice>
                    <nam:ExtendedAmount Currency="SEK" Action="Add">49.00</nam:ExtendedAmount>
                    <nam:Quantity Units="1" UnitOfMeasureCode="EA">1</nam:Quantity>
                    <nam:Tax>
                        <nam:Percent>25</nam:Percent>
                    </nam:Tax>
                </nam:Sale>
                <nam:SequenceNumber>2</nam:SequenceNumber>
            </nam:LineItem>
            ...
        </nam:RetailTransaction>
        <se:ReceiptCode>1281111260543493</se:ReceiptCode>
    </nam:Transaction>
</nam:DigitalReceipt>                                
```

#### Return receipt
```xml
<nam:DigitalReceipt...">
    <nam:Transaction>
    ...
        <nam:ReceiptDateTime>2017-12-05T21:52:33+01:00</nam:ReceiptDateTime>
        <nam:ReceiptNumber TypeCode="Interleaved2Of5">02890093684021</nam:ReceiptNumber>
        <nam:RetailTransaction>
            <nam:ItemCount>3</nam:ItemCount>
            <nam:LineItem>
                <nam:Return>
                    <nam:POSIdentity>
                        <nam:POSItemID>923048</nam:POSItemID>
                    </nam:POSIdentity>
                    <nam:Description>Headset hörlurar - SLR</nam:Description>
                    <nam:UnitCostPrice Currency="SEK" Quantity="1" UnitOfMeasureCode="EA">49.00</nam:UnitCostPrice>
                    <nam:ActualSalesUnitPrice Currency="SEK">49.00</nam:ActualSalesUnitPrice>
                    <nam:ExtendedAmount Currency="SEK">49.00</nam:ExtendedAmount>
                    <nam:Quantity Units="1" UnitOfMeasureCode="EA">1</nam:Quantity>
                    <nam:Tax>
                        <nam:Percent>25</nam:Percent>
                    </nam:Tax>
                    <nam:Reason>NA</nam:Reason>
                </nam:Return>
                <nam:SequenceNumber>1</nam:SequenceNumber>
            </nam:LineItem>
            <nam:LineItem>
                <nam:Tax>
                    <nam:TaxableAmount Currency="SEK" TaxIncludedInTaxableAmountFlag="false">39.20</nam:TaxableAmount>
                    <nam:Amount Currency="SEK">9.80</nam:Amount>
                    <nam:Percent>25</nam:Percent>
                </nam:Tax>
                <nam:SequenceNumber>2</nam:SequenceNumber>
            </nam:LineItem>
            <nam:LineItem>
                <nam:Tender TypeCode="Refund" TenderType="Cash">
                    <nam:Amount Currency="SEK">49.00</nam:Amount>
                    <se:Text>Åter</se:Text>
                </nam:Tender>
                <nam:SequenceNumber>3</nam:SequenceNumber>
            </nam:LineItem>
            <nam:Total TotalType="TransactionGrandAmount" TypeCode="Return">49.00</nam:Total>            
            ...
        </nam:RetailTransaction>
        <se:ReceiptCode>1202151240570529</se:ReceiptCode>
    </nam:Transaction>
</nam:DigitalReceipt>                                
```
In this case there is nothing in the return receipt that links to the sale receipt (No *TransactionLink* or *ReceiptCodeLink*). 