# BigCommerce Vendor Integration Guide

### Step 1. Create the API account that will allow the ClassWallet service to be connected. 
- Go to settings → Store-level API Accounts
- Click “Create API account”
- Name: ClassWallet Connector
- Token Type: V2/V3 API token
- Select the following OAuth scopes

| Scope | Permission |
| :---- | :----: |
|Content| Modify|
|Checkout Content| Modify|
|Customers|Modify|
|Customers Login|Login|
|Information & Settings|Read-only|
|Marketing|None|
|Orders|Modify|
|Order Transactions|Modify|
|Create Payments|None|
|Get Payment Methods|None|
|Stored Payment Instruments|None
|Products|Read-only|
|Themes|None
|Carts|Modify|
|Checkouts|Modify|
|Sites & Routes|Read-only|
|Channel Settings|Read-only|
|Channel Listings|Read-only|
|Storefront API Tokens|Manage| 
|Storefront API Customer impersonation tokens|None|
|Store Logs|None|
|Store Locations|None|
|Store Inventory|Read-only|
|Fulfillment Methods|Read-only|
|Order Fulfillment|None|
|Metafields Ownership|Manage|
|Metafields Access|Full|
|Store Translations|None|

Click save and provide the credentials to ClassWallet 
