# Tax Exempt Quick Reference Guide
When a vendor is launched in a tax-exempt eligible market, the ClassWallet team will need to enable their vendor account to begin receiving the tax status of users when a shopper starts a session from the ClassWallet Marketplace.

### Plugin Vendors (Shopify, WooCommerce, Magento, BigCommerce)

No action needed by plugin vendors. The plugins we have developed are listening for these tax-exempt flags and will action the order to be created without tax if tax-exempt user status is detected.

---

### API Vendors

Once enabled, we will begin sending a tax-exempt property in the JSON payload to your start URL to indicate the tax status of the user:

```json
"taxExempt": true

```

```json
"taxExempt": false

```

This should be parsed from the payload and used to create the order without tax if the user is `"taxExempt": true`.

> **Note:** Full payload examples are available in the API docs: [`PayByAPI.md`](https://github.com/devinlejeune-cw/classwallet-docs/blob/main/PayByAPI.md)

---

### cXML Vendors

Once enabled, we will include an `Extrinsic` tag in the `PunchOutSetupRequest`, as well as an `Extrinsic` tag with the Order Request in the `OrderRequestHeader` containing the tax status of the user:

```xml
<Extrinsic name="TaxExempt">false</Extrinsic>

```

```xml
<Extrinsic name="TaxExempt">true</Extrinsic>

```

These tags should be identified and used accordingly to create the order without taxes when `<Extrinsic name="TaxExempt">true</Extrinsic>` is detected.

> **Note:** Full document examples are provided in the cXML docs: [`cXML.md`](https://github.com/devinlejeune-cw/classwallet-docs/blob/main/cXML.md)
