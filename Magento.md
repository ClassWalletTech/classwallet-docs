# Magento 2 ClassWallet Extension

### Installation:
Unzip into app/code. Run:
```
rm -rf var/cache/*
rm -rf var/page_cache/*
rm -rf var/session/*
rm -rf var/di/*
rm -rf var/generator/*
php bin/magento maintenance:enable
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento setup:static-content:deploy -f
php bin/magento cache:flush
php bin/magento maintenance:disable
php bin/magento cache:enable
```

### Start URL:
`https://<domain_name>/classwallet/create/account`

### Setup: 
Login to the Magento admin and select stores > configuration\
<img width="163" height="271" alt="image" src="https://github.com/user-attachments/assets/c6325189-676b-457c-b3c9-18000b18e95d" />\
Then sales > Payment Methods\
<img width="164" height="247" alt="image" src="https://github.com/user-attachments/assets/5640f24e-2874-49ed-8fd5-aacc9663b5de" />\
Find ClassWallet payment gateway\
<img width="385" height="239" alt="image" src="https://github.com/user-attachments/assets/d7951ac8-f94a-4e0e-8536-866fa49d15cf" />\
**Enabled**\
Toggle ClassWallet as the only available payment method for ClassWallet users. Regular site
users will not see ClassWallet as a payment method even when this is enabled.

**Store Code**\
Determines which store the payment gateway will apply to

**Default Customer Group**\
This sets the effective customer group for the ClassWallet session. Use this setting to set
specific rules for ClassWallet customers, e.g., create a group called ClassWallet that gets a 50%
reduction in shipping. Changes are only effective for the current ClassWallet session. No
permanent changes are made to any existing users’ group membership.

**Payment Complete Status**\
Sets the status of the order after it has been approved by ClassWallet. Note: some orders may
take up to 10 business days to complete.

**Title**\
The title to be displayed with the payment method at checkout

**Vendor ID, API Username, API Password**\
These remaining three fields need to be filled with what is provided to you during onboarding.

**Enable Security Token**\
This is an optional new feature allowing the extension to utilize ClassWallet’s new token based
session authentication. IMPORTANT. _Please do not enable this feature before contacting
ClassWallet and informing them that you would like to enable the security token on your
account._

### Checkout Workflow: 
Shipping address will be pre-populated with the data from ClassWallet.\
<img width="177" height="172" alt="image" src="https://github.com/user-attachments/assets/e5a386a0-4265-4f5b-86a5-439e29bcf1f7" />\
Checkout will function as normal except payment options will be restricted to ClassWallet only
for users coming from ClassWallet.\
<img width="239" height="149" alt="image" src="https://github.com/user-attachments/assets/5c2e1801-9b45-4be9-a2af-b843dfe925a4" />


### Admin Orders Backend: 
The orders will be in a “Pending” status until the order is approved or rejected, the order will
move to “Processing” or “Canceled” respectively. Upon completion, the order will be marked
with a transition id and ClassWallet order id. They are both the same and will be used for
invoicing ClassWallet.\
<img width="276" height="102" alt="image" src="https://github.com/user-attachments/assets/6a8e77a4-e80b-4165-abcd-a82a21499c42" />












