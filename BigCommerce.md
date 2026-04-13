# BigCommerce Vendor Integration Guide

### Step 1. Create the API account that will allow the ClassWallet service to be connected. 
- Go to settings → Store-level API Accounts
- Click “Create API account”
- Name: ClassWallet Connector
- Token Type: V2/V3 API token
- Select the following OAuth scopes

| Scope | Permission |
| :---- | :----: |
| Content | Modify|
|Checkout Content| Modify|
|Customers|Modify|
|Customers Login|Token Login|
|Information & Settings|None|
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
|Sites & Routes|Modify|
|Channel Settings|Modify|
|Channel Listings|Modify|
|Storefront API Tokens|Generate Tokens/Manage| 
|Storefront API Customer impersonation tokens|Generate Tokens/Manage|
|Store Logs|None|
|Store Locations|None|
|Store Inventory|Read-only|
|Fulfillment Methods|None|
|Order Fulfillment|None|
|Metafields Ownership|Manage|
|Metafields Access|Full|

Click save and provide the credentials to ClassWallet 

### Step 2. Create the script for the storefront:
- Go to Storefront → Script manager → Create script
  - If using multi-storefront, Script Manager and other storefront-specific settings can be found within Channel Manager.
 
**Script name:** ClassWallet JS

**Placement:** Footer

**Location:** Checkout

**Script category:** Functional

**Script type:** URL

**Load method:** Defer

**Script URL:** `https://store-{store_hash}.appsody.co/api/v1/js_lib.js`

**Integrity Hash:**  Paste in the integrity hash provided by ClassWallet

### Step 3: Create the web page for ClassWallet to start the session.
- Go to Storefront →  Web Pages, then click Create a Web Page.
  - If using multi-storefront, Web Pages can be found within their respective storefront settings within Channel Manager
- For _“This Page Will”_, select “Contain raw HTML entered in the text area below.”
**Web Page Details:**\
**Page Name:** Any name you choose. For clarity, choose something descriptive. Example: "ClassWallet Start Page"

**Page URL:** `/start-classwallet`

**Page Content**:
```
<script type="text/javascript">
const url_params=new URLSearchParams(window.location.search);let cw_decoded=atob(url_params.get("cw_data")),cw_params=JSON.parse(cw_decoded);sessionStorage.setItem("classwallet:vars:json:b64",btoa(JSON.stringify(cw_params.cw_data)));let login_url=cw_params.login_url;window.location.href=login_url;
</script>
```
**Navigation Menu:** Un-check the box “Yes, show this web page on the navigation menu”. We do not want the page showing in the menu.
