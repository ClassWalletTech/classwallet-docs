# Shopify Integration Overview

The ClassWallet-Shopify integration allows vendors to offer their products directly to ClassWallet users. By installing a lightweight Shopify app, vendors enable a "Punch-out" shopping experience where orders are initiated on the vendor's site, approved within the ClassWallet ecosystem, and then automatically pushed back to Shopify for fulfillment.
The purpose of this document, is to detail the different features and setup steps for the ClassWallet-Shopify integration.


## The Integration Flow 
 ### 1. Connection and Setup
 ClassWallet provides a dedicated Shopify App installation link. Installation of the app grants ClassWallet the necessary permissions to:
  - **Create Customers:** The App will look for a customer based on the email sent from ClassWallet, and will create one if none exists. The purpose of this is to have a customer record to attach to orders as they are created.
  - **Create Orders:** Creating draft orders (pre-approval), converting draft orders to complete (approval)
  - **Inject Snippets:** A small code snippet is added to the storefront to detect ClassWallet sessions and manage the UI and checkout flow (hiding standard checkout buttons, directing the shopper to ClassWallet)

### 2. The Shopping Experience (Punch-out) 
- **Entry:** Shoppers start at the ClassWallet Marketplace and "punch out" to the vendor’s Shopify store.
- **Session Tracking:** The shopper’s identity and session details are stored in secure session variables upon landing.
- **Store UI:** The ClassWallet snippet hides standard "Buy Now" or "PayPal" buttons to ensure the user follows the approved procurement path.
- **Checkout:** When the user clicks "Checkout," they are directed to a ClassWallet proxy page to review their cart before proceeding to ClassWallet for order finalization, and funds selection.

### 3. Draft Order Creation
Once the shopper confirms their cart and proceeds to the ClassWallet checkout, we create a **Draft Order** in the vendor's Shopify admin. We calculate order totals by:
- Querying the active cart for Product/Variant IDs and quantities, and passing those into the draft order.
- Polling the Shopify store for available shipping options based on the shopper's address.
- Applying relevant taxes.

### 4. Approval & Fulfillment
- **Pending State** The order remains as a "Draft" in Shopify while it goes through the ClassWallet approval workflow (timing varies).
- **Conversion:** Once approved, ClassWallet makes an API call to the Shopify store to convert the Draft Order into a Completed Order.
- **Inventory:** Inventory is only officially decremented when the Draft Order is converted to a Completed Order.
---

## Technical Constraints & Considerations
To ensure a smooth partnership, please note the following technical parameters:

| Feature      | Support Status | Detail     |
| :----:        |    :---:   |         :----: |
| **Automatic Discounts**      | Not Supported       | We pull the base product price. Shopify-side automatic discounts or coupon codes are not currently applied to ClassWallet orders.   |
| **Inventory Buffers**   | Vendor Managed        | If an item goes out of stock between the time of the "Draft" and the "Approval," the conversion will fail, and the order will not be placed.    |
| **Shipping Rules**   | Standard Only | 	We poll for "Available Shipping Options." Custom third-party shipping rules or logic may not be reflected.      |
| **Payment Handling**   | ClassWallet        | Payment is handled via the ClassWallet platform; no credit card processing occurs on the vendor's Shopify checkout page.      |

---
## Installation 
1. Log into your Shopify store `(https://shopify.com/login)`.
2. Once logged in, paste the app link provided to you by ClassWallet in your browser address bar and Go\
 _`example: (https://admin.shopify.com/store/store-name/oauth/install_custom_app?client_id=…)`_
3. If prompted, select the desired store you wish to install the app on.
   <img width="80%" height="auto" alt="Shopify Login Screen" src="https://github.com/user-attachments/assets/07cd8b4a-63f5-475e-ba47-b2db03a5a5db" />
4. Review the requested access. Click Install\
   <img width="80%" height="auto" alt="ClassWallet App Scopes Authorization Page" src="https://github.com/user-attachments/assets/47894a30-79ad-4aaf-8a6f-f226c57792cb" />

The ClassWallet app is now installed on your store. No additional configuration is needed. 
