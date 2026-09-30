# ClassWallet cXML Integration Guide

cXML is a protocol format that exchanges data between an eCommerce system, eProcurement
system, and other business software to streamline B2B sales processes. The protocol defines
how organizations transmit information and the steps involved in communicating documents and
verifying that they have been received. The format defines the structure and content of cXML
documents via a set of XML Document Type Definitions (DTD).

XML is a widely used markup format, and XML DTDs define the rules that encode a specific
type of document. If you’re familiar with HTML, you’ll recognize the format of XML documents,
which use tags such as to enclose and describe the data. cXML DTDs cover most of the
information that businesses want to exchange during procurement, as well as documents that
support synchronization and integration throughout the TradeCentric process.

ClassWallet has implemented a cXML workflow to allow vendors to create orders on their
platform with ClassWallet working as the payment gateway. Orders are created on your system
and when it is time to place the order, the user will punchout to ClassWallet to allocate funds to
pay for the items provided in the order request punchout. Below is an overview of the cXML
request response workflow as implemented by ClassWallet:

<img width="294" height="327" alt="image" src="https://github.com/user-attachments/assets/cb3ba45c-8d9a-4771-bf6c-a92fafd85c5e" />

### Credentials and URLs
Please provide the following credentials during your setup

| Name      | Example | Description     |
| :----:        |    :---:   |         :----: |
| **Setup Request URL**      | `https://<your_domain>/api/punchin` | This is the URL where ClassWallet will send the cXML. Respond with a URL to your start page.   |
| **Purchase Order URL**   | `https://<your_domain>/api/purchase`        | Here you will receive the approved order request from ClassWallet.   |
| **Identity**   | `<your_identity>` | 	Can be anything (e.g. cxml-test). Used to identify your cXML requests      |
| **Shared Secret**   | `<shared_secret>`        | Extra check for security (e.g.base64 string).      |
| **User Agent**   | `<user_agent>`        | Identity + Shared Secret + User Agent uniquely identifies your account with ClassWallet (e.g. cxml-myshop).      |

### Workflow Example:
When a customer browses the Marketplace and clicks on your tile, you will receive a POST to
your setup request URL. Below is a Postman script to use in development to simulate customers
clicking on your tile.

**Receive POST from ClassWallet**\
Parse out customer details and begin a shopping session in your store.
Extract the:
```
…
<BrowserFormPost>
    <URL>https://app.classwallet.com/api/external/get_cart/650e020829a2ea34fc680fac</URL>
</BrowserFormPost>
…
```
**_Save this URL_**. You will use it as the destination to post your cXML order request

**Respond with Shop URL**
```
…
<PunchOutSetupResponse>
    <StartPage>
        <URL>https://<your_domain>/api/shop</URL>
    </StartPage>
</PunchOutSetupResponse>
…
```
ClassWallet will then send the user to this URL to start shopping.

### Checkout:
Once the user has added their items to your cart, construct a form to submit to ClassWallet to
start the checkout process.

Base 64 encode the cXML order message and submit as the field named “cxml-base64” to the
browser form post URL you saved from the setup request.

Base 64 encode the XML and submit as the field name “cxml-base64” as shown below

**Example Form:**
```
<form method="post"
action="https://app.classwallet.com/api/external/get_cart/650e020829a
2ea34fc680fac" "="">
<input type="hidden" name="cxml-base64"
value="<base_64_cxml>">
<input type="submit" value="checkout">
</form>
```

### Finalize Order
Once the order has been approved on ClassWallet’s side, an order request will be sent to your
purchase order URL you provided during setup.

**Pay close attention to the line:**
```
…
<OrderRequestHeader orderID="12548349"
orderDate="2023-09-26T16:54:04.076Z" type="new">
…
```
It contains the order Id you will use to invoice ClassWallet.

### cXML Payloads

**PunchOutSetupRequest**

** Tax exemption status **

```<Extrinsic name="TaxExempt">true</Extrinsic>```

_**NOTE:** You will need to request to have this field enabled for your implementation.

```
<?xml version="1.0"?>
<!DOCTYPE cXML SYSTEM "http://xml.cxml.org/schemas/cXML/1.2.014/cXML.dtd">
<cXML xml:lang="en-US" payloadID="465139514923"
	timestamp="2002-08-15T08:36:47-07:00">
	<Header>
		<From>
			<Credential domain="NetworkId">
				<Identity>cxml-ren-dev</Identity>
			</Credential>
		</From>
		<To>
			<Credential domain="DUNS">
				<Identity>cxml-ren-dev</Identity>
			</Credential>
		</To>
		<Sender>
			<Credential domain="NetworkId">
				<Identity>cxml-ren-dev</Identity>
				<SharedSecret>********************</SharedSecret>
			</Credential>
			<UserAgent>test-cxml-vendor</UserAgent>
		</Sender>
	</Header>
	<Request>
		<PunchOutSetupRequest operation="create">
			<BuyerCookie>cookie</BuyerCookie>
			<Extrinsic name="UserEmail">youremail@yourdomain.com</Extrinsic>
			<Extrinsic name="UniqueName">Dev Testing</Extrinsic>
			<Extrinsic name="CostCenter">610</Extrinsic>
			<Extrinsic name="TaxExempt">true</Extrinsic>
			<BrowserFormPost>
				<URL>https://app.classwallet.com/api/external/get_cart/650e020829a2ea
					34fc680fac</URL>
			</BrowserFormPost>
			<SupplierSetup>
				<URL>https://yourdomain.com/api/punchin</URL>
			</SupplierSetup>
			<ShipTo>
				<Address addressID="1000467">
					<Name xml:lang="en-US">ClassWallet University East</Name>
					<PostalAddress>
						<DeliverTo>Dev Testing</DeliverTo>
						<Street>6100 Hollywood Blvd Suite 409 </Street>
						<City>Hollywood</City>
						<State>FL</State>
						<PostalCode>33024</PostalCode>
						<Country isoCountryCode="US">United States</Country>
					</PostalAddress>
					<Phone>
						<TelephoneNumber>
							<CountryCode isoCountryCode="US">1</CountryCode>
							<AreaOrCityCode>877</AreaOrCityCode>
							<Number>9695536</Number>
						</TelephoneNumber>
					</Phone>
				</Address>
			</ShipTo>
		</PunchOutSetupRequest>
	</Request>
</cXML>
```

**PunchOutSetupResponse**
```
<cXML version="1.0"
    payloadID="1550595591.4444240804.0569@demo.punchoutexpress.com"
    timestamp="2023-06-15T07:14:33+00:00">
    <Response>
        <Status code="200" text="TEXT" />
        <PunchOutSetupResponse>
            <StartPage>
                <URL>https://yourdomain.com/api/shop</URL>
            </StartPage>
        </PunchOutSetupResponse>
    </Response>
</cXML>
```
**PunchOutOrderMessage**
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE cXML SYSTEM "http://xml.cxml.org/schemas/cXML/1.1.007/cXML.dtd">
<cXML version="1.1" payloadID="15be0730-d1a8-4bb6-aac5-46ca1da337a1"
    timestamp="2023-09-26T12:14:51-04:00" xml:lang="en-US">
    <Header>
        <From>
            <Credential domain="NetworkId">
                <Identity>cxml-ren-dev</Identity>
            </Credential>
        </From>
        <To>
            <Credential domain="NetworkId">
                <Identity>cxml-ren-dev</Identity>
            </Credential>
        </To>
        <Sender>
            <Credential domain="NetworkId">
                <Identity>cxml-ren-dev</Identity>
                <SharedSecret>********************</SharedSecret>
            </Credential>
            <UserAgent></UserAgent>
        </Sender>
    </Header>
    <Message>
        <PunchOutOrderMessage>
            <BuyerCookie>********************************</BuyerCookie>
            <PunchOutOrderMessageHeader operationAllowed="create">
                <Total>
                    <Money currency="USD">1.94</Money>
                </Total>
                <Shipping>
                    <Money currency="USD">0.95</Money>
                    <Description xml:lang="en">Shipping</Description>
                </Shipping>
                <Tax>
                    <Money currency="USD">0</Money>
                    <Description xml:lang="en">Taxes</Description>
                </Tax>
            </PunchOutOrderMessageHeader>
            <ItemIn quantity="1">
                <ItemID>
                    <SupplierPartID>UG-53946</SupplierPartID>
                    <SupplierPartAuxiliaryID>76432</SupplierPartAuxiliaryID>
                </ItemID>
                <ItemDetail>
                    <UnitPrice>
                        <Money currency="USD">1.99</Money>
                    </UnitPrice>
                    <Description xml:lang="en">Double Ladderball,
                        Classic</Description>
                    <UnitOfMeasure>EA</UnitOfMeasure>
                    <Classification domain="SPSC"></Classification>
                    <ManufacturerPartID></ManufacturerPartID>
                    <ManufacturerName xml:lang="en">University
                        Games</ManufacturerName>
                    <URL>//images.salsify.com/image/upload/s--RlFf2o6C--/c_lpad,w_400,h_4
                        45,q_60,fl_progressive/rikcgayep5smwnbfvlen.jpg</URL>
                </ItemDetail>
            </ItemIn>
        </PunchOutOrderMessage>
    </Message>
</cXML>
```
**OrderRequest**
```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE cXML SYSTEM "http://xml.cxml.org/schemas/cXML/1.2.019/cXML.dtd">
<cXML payloadID="12548349481915137680@classwallet.com"
    timestamp="2023-09-26T16:54:04.076Z" xml:lang="en-US">
    <Header>
        <From>
            <Credential domain="NetworkId">
                <Identity>cxml-ren-dev</Identity>
            </Credential>
        </From>
        <To>
            <Credential domain="DUNS">
                <Identity>cxml-ren-dev</Identity>
            </Credential>
        </To>
        <Sender>
            <Credential domain="NetworkId">
                <Identity>cxml-ren-dev</Identity>
                <SharedSecret>********************</SharedSecret>
            </Credential>
            <UserAgent>test-cxml-vendor</UserAgent>
        </Sender>
    </Header>
    <Request deploymentMode="production">
        <OrderRequest>
            <OrderRequestHeader orderID="12548349"
                orderDate="2023-09-26T16:54:04.076Z" type="new">
                <Extrinsic name="TaxExempt">true</Extrinsic>
                <Total>
                    <Money currency="USD">2.94</Money>
                </Total>
                <ShipTo>
                    <Address addressID="">
                        <Name xml:lang="en-US">Rendahl Testing</Name>
                        <PostalAddress name="Rendahl Testing">
                            <DeliverTo>Rendahl Testing</DeliverTo>
                            <DeliverTo>ClassWallet University East</DeliverTo>
                            <Street>6100 Hollywood Blvd Suite 409 </Street>
                            <City>Hollywood</City>
                            <State>FL</State>
                            <PostalCode>33024</PostalCode>
                            <Country isoCountryCode="US">United States</Country>
                        </PostalAddress>
                        <Email name="Rendahl
Testing">youremail@yourdomain.com</Email>
                        <Phone>
                            <TelephoneNumber>
                                <CountryCode isoCountryCode="US">1</CountryCode>
                                <AreaOrCityCode>877</AreaOrCityCode>
                                <Number>9695536</Number>
                            </TelephoneNumber>
                        </Phone>
                    </Address>
                </ShipTo>
                <BillTo>
                    <Address addressID="88">
                        <Name xml:lang="en-US">ClassWallet.com Attn: Accounts
                            Payable</Name>
                        <PostalAddress name="ClassWallet.com Attn: Accounts
Payable">
                            <DeliverTo>Executive Director</DeliverTo>
                            <Street>6100 Hollywood Blvd Suite 108</Street>
                            <City>Hollywood</City>
                            <State>FL</State>
                            <PostalCode>33024</PostalCode>
                            <Country isoCountryCode="US">United States</Country>
                        </PostalAddress>
                        <Email name="Willis
Fernandez">vendorinvoices@classwallet.com</Email>
                        <Phone>
                            <TelephoneNumber>
                                <CountryCode isoCountryCode="US">1</CountryCode>
                                <AreaOrCityCode>877</AreaOrCityCode>
                                <Number>9695536</Number>
                            </TelephoneNumber>
                        </Phone>
                    </Address>
                </BillTo>
                <Shipping>
                    <Money currency="USD">0.95</Money>
                    <Description xml:lang="en-us">Standard</Description>
                </Shipping>
                <Tax>
                    <Money currency="USD">0.00</Money>
                    <Description xml:lang="en-US">Standard</Description>
                </Tax>
                <PaymentTerm payInNumberOfDays="30" />
                <Contact role="purchasingAgent">
                    <Name xml:lang="en-US">Willis Fernandez</Name>
                    <Email>vendorinvoices@classwallet.com</Email>
                    <Phone name="Office">
                        <TelephoneNumber>
                            <CountryCode isoCountryCode="US">1</CountryCode>
                            <AreaOrCityCode>877</AreaOrCityCode>
                            <Number>9695536</Number>
                        </TelephoneNumber>
                    </Phone>
                </Contact>
                <Comments xml:lang="en-US" />
            </OrderRequestHeader>
            <ItemOut quantity="1" lineNumber="1">
                <ItemID>
                    <SupplierPartID>UG-53946</SupplierPartID>
                    <SupplierPartAuxiliaryID>76432</SupplierPartAuxiliaryID>
                </ItemID>
                <ItemDetail>
                    <UnitPrice>
                        <Money currency="USD">1.99</Money>
                    </UnitPrice>
                    <Description xml:lang="en-US">Double Ladderball,
                        Classic</Description>
                    <UnitOfMeasure>EA</UnitOfMeasure>
                    <Classification domain="SPSC">null</Classification>
                </ItemDetail>
            </ItemOut>
        </OrderRequest>
    </Request>
</cXML>
```




