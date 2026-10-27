Omnifi Payment Gateway for Magento 2
Take payments in Magento 2 / Adobe Commerce through Omnifi, with four providers:
Tyro, Worldline, Worldline ANZ and WPAY (MX51).
Features
Hosted checkout redirect for every provider, and inline card entry (tokenization)
with saved cards for Worldline and Worldline ANZ.
Order status from three sources (the customer's return, webhooks and polling), so a
payment is applied even if one of them never arrives.
Refunds in both directions: from the Magento admin, and refunds made in Omnifi
arriving as credit memos. Full and partial.
Surcharge and processing fee options.
Connect with Omnifi: connect Tyro, Worldline or Worldline ANZ by signing in to
Omnifi, with no keys to copy. Includes a test webhook, disconnect, a reference for
every attempt, an activity log in the admin, automatic problem reporting to Omnifi
and a Send diagnostics button. WPAY uses a manually entered merchant key.
Requirements
Magento Open Source or Adobe Commerce 2.4.x
PHP 8.2 or 8.3
Magento cron running (order status polling and problem reporting run on it)
Install
The module is a composer package (`omnifi/module-gateway`). In a Magento 2.4.x app:
```bash
composer config repositories.omnifi-gateway path /path/to/omnifi-magento-gateway
composer require omnifi/module-gateway
bin/magento module:enable Omnifi_Gateway
bin/magento setup:upgrade && bin/magento setup:di:compile
bin/magento cache:flush
```
Or copy the module to `app/code/Omnifi/Gateway` and run the same last three commands.
Configure
Go to Stores > Configuration > Sales > Payment Methods > Omnifi Payment Gateway.
Enable the method and choose the provider used for new checkouts.
To connect with Omnifi, enter the Omnifi API Key (SSO) and External Store ID
(SSO), then click Connect with Omnifi in the provider's section and sign in.
The merchant key and webhook secret are saved for you, and a test webhook runs
straight away.
Without connecting (and always for WPAY), enter the provider's merchant key and
webhook secret by hand.
Endpoints, relative to the store URL:
Webhooks: `/omnifi/webhook`
Customer return: `/omnifi/checkout/complete`
Connect callback: `/omnifi/sso/callback`
Logs go to `var/log/omnifi.log`. Errors are always logged; turn on Debug Logging
for more detail. The Connect Activity (SSO) panel on the same settings page shows
the latest connect activity with a reference to quote to Omnifi support.
Test
The pure-logic tests run without a Magento installation:
```bash
composer install
vendor/bin/phpunit -c Test/Unit/phpunit.xml --exclude-group magento
```
Tests tagged `@group magento` need the framework and run from inside a Magento
application:
```bash
vendor/bin/phpunit -c app/code/Omnifi/Gateway/Test/Unit/phpunit.xml
```
The coding standard must report zero errors and zero warnings:
```bash
vendor/bin/phpcs
```
Delete `vendor/` afterwards if this module is mounted into a Magento application; a
second PHPUnit under `app/code` makes `setup:di:compile` fail.
To check that every class in the module constructs through Magento's object manager,
run from the Magento root:
```bash
php app/code/Omnifi/Gateway/dev/omnifi-wiring.php
```
Licence
Open Software License 3.0 (OSL-3.0). See LICENSE.txt.
