# WooCommerce Integration Overview

ClassWallet has a service called Pay By ClassWallet. This service enables Vendors to seamlessly
integrate our closed loop payments platform into your shopping cart.

You can easily perform the integration using our APIs or you can simply install a simple
WordPress WooCommerce plugin.\
The Plugin can be found at:
https://github.com/ClassWalletTech/classwallet-wordpress-plugin/releases

Please validate the hash of the plugin to ensure it is genuine and has not been tampered with.
Hash validation ensures that you have received a valid file from ClassWallet.

We use a MD5 Hash algorithm and you can validate our Hash using this online tool ( or any
other tool you choose): https://emn178.github.io/online-tools/md5_checksum.html. The MD5
for the current plugin can be found on the plugin download page.

To install this plugin, you need to be running WordPress 6.0 or later and WooCommerce 9.0 or
later.
The plugin has been tested up to WordPress 6.8.1 and WooCommerce 9.8.5

ClassWallet users enter your store from the ClassWallet application and will be
authenticated. Once our users access your site, they will only be presented with the ClassWallet
payment option. Your normal users will not be presented with ClassWallet as a payment
method.

## Installation

1. Log into your Wordpress dashboard
2. Select 'Plugins'
<img width="410" height="353" alt="image" src="https://github.com/user-attachments/assets/2d022705-49cd-4b4d-9265-dae4cce3fcbd" />

3. Click 'Upload Plugin'
<img width="371" height="296" alt="image" src="https://github.com/user-attachments/assets/b6c2a1be-09c4-41e2-bf61-3cb1a5b25bdf" /> 

4. Navigate to where you have stored the WooCommerce plugin. And follow the instructions to install our plugin.
<img width="672" height="206" alt="image" src="https://github.com/user-attachments/assets/dd87b8c2-038c-41fc-aba0-2f6e98e750ad" />

5. After installation, you will see the plugin in the list. But you will still need to configure Pay By ClassWallet and this is done inside of WooCommerce. Go to WooCommerce -> Settings -> Payments -> ClassWallet
<img width="75%" height="auto" alt="image" src="https://github.com/user-attachments/assets/81deef74-1a88-42d3-9fa9-7542bd668af9" />

<img width="505" height="288" alt="image" src="https://github.com/user-attachments/assets/580ec5ed-5457-427b-b092-437be0e7417a" />

**Start URL**\
Used by ClassWallet to start the shopping session

**Enable/Disable Checkout With ClassWallet Gateway**\
Toggle the payment gateway on/off

**Enable/Disable Debug Mode**\
Add extra debugging info. Should always be disabled unless troubleshooting an issue.

**Redirect URL**\
Sets where the ClassWallet user is redirected to once they arrive on the site.

**Remove coupon code**\
Suppresses the coupon code input at checkout for ClassWallet customers.

**ClassWallet Customer Role**\
Choose the role ClassWallet customers will be assigned when they browse your site.\
_NOTE: Users with administrative roles will NOT be able to use their account for extra security._



**Order Status Flow**\
Select to use the default WooCommerce order status flow OR use a custom order status flow. 
- __Default WooCommerce order status flow__ will set the initial order status as 'pending payment'. Once the ClassWallet order is approved, the order will be set to 'Processing' and the default `woocommerce_payment_complete` action will be fired.
- __Custom__ will allow you to select a desired order status for ClassWallet orders when the order is initially created, and optionally set a completed order status once the order is approved.\
*Note: Using the Custom order status flow will not trigger the default `woocommerce_payment_complete` action when an order is approved. If you have any additional 3rd party functionality that hooks into this WooCommerce action, they will not be triggered.* 

| Custom | Default WooCommerce Flow |
| :----: | :----: |
| <img width="408" height="263" alt="image" src="https://github.com/user-attachments/assets/81a36073-24df-4bd6-8805-fe36f405999a" /> | <img width="496" height="197" alt="image" src="https://github.com/user-attachments/assets/919fe7af-5fff-46d2-82a4-ef441d18aa27" />| 

**Initial order status** *(if using custom)*  \
Select the status you would like when a new order is created. By default, the custom flow will use the “Processing” status.\
Choose the initial order status that best fits your workflow. “CW Processing” is a non standard WooCommerce order status, and therefore, will not trigger WooCommerce status email triggers. If you need your normal emails to trigger, you may want to choose an initial order status from the available WooCommerce order statuses in the dropdown.

**Payment Complete Status** *(if using custom)* \
Set a custom status for completed ClassWallet orders, e.g. “ClassWallet Complete.” Leave
blank to use the default “Processing” status.

**Title**\
Title to be displayed on the checkout page.

**Description**\
Displayed along with title on checkout page.

**ClassWallet Vendor Id**\
Provided by ClassWallet during onboarding.

**Basic HTTP Auth Username**\
Provided by ClassWallet during onboarding.

**Basic HTTP Auth Password**\
Provided by ClassWallet during onboarding

## Logging and Debugging
Many operations that the plugin makes leave a log entry trail for verification and validation +
general debugging. You can view these logs at:
`WooCommerce -> Status -> Logs`
### Sample log output
<img width="443" height="253" alt="image" src="https://github.com/user-attachments/assets/bb9e56ba-f1b4-4bb3-a33a-f936d9d07192" />\
Enable Debugging mode, and attempt to replicate the error or behavior. Please provide the entire log dump for the day in question when contacting support if possible.

All transactions need to be approved. We will notify your plugin when the sale is either “Approved” or “Canceled.” You can look at orders and see their status under the WooCommerce orders area. You can only ship or deliver when the payment transaction is
complete.
