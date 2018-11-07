---
title: API Reference

language_tabs:
  - shell: Curl
  - ruby: Ruby

search: true
---

# Introduction

Welcome to the elopage API.  <a target='blank' href='https://elopagecom.typeform.com/to/IVLKH6'>Please fill out this form to get access to the API.</a>

The elopage API is based on REST. To get started and explore as much as possible, you can apply for the API credentials and get both test and live credentials. Live credentials can be accessed via your dashboard. To switch between the test and live environment, simply change the API credentials and the (sub)domain-URL. Our sandbox is run on staging.elopage.com. Requests in the staging environment will never touch the banking networks and will not create costs on your side. You can test Paypal, Sofort, Bank Wire and Credit Card payments.

Be sure to subscribe to our <a target='blank' href='https://elopage.com'>API-updates list</a> to receive updates on changes, additions or deprecations.

<b>On the right side you can use sample requests.</b> To go live, make sure to use your API key that is linked to your elopage account.  

<aside class='notice'>To use the API to create sales pages, payment plans and payment sessions, you don’t need to create products via your dashboard on elopage.com. However, make sure to complete registration and legitimate your account before starting your sales. </aside>

<b>Which steps have to be completed via elopage.com?</b>
<ul>
<li>Registration and profile creation</li>
<li>Creation and activation of seller account: Imprint, Payment methods activation, General VAT settings</li>
<li>Legitimation process</li>
<li>Refunds (for now only via dashboard, will be added soon)</li>
</ul>

Feel free to <a href='mailto:support@elopage.com'>contact us</a> or visit our <a href='http://support.elopage.com' target='blank'>Help Center.</a>

# Authentication
Authenticate your account by using your secret API key in the request. You can create or renew your API keys in your dashboard settings. Keep your API and secret keys secret. Please do not share your API keys publicly on Github and so forth.

On production you must create requests over HTTPS, otherwise calls will fail. Sandbox requests can be with HTTP.

# Sandbox
Please access our sandbox environment using 'http://staging.elopage.com/'. API key and secret for the sandbox environment will be sent to you per email after your approval of your registration to our developer community.

Please fill in the information here to get access to the elopage API.

## Test payments credentials

Please access our sandbox environment using http://staging.elopage.com/ to test payments. Credentials for test payments will not work on the production environment. For now, we don’t have the option to test payments on production except for real payments.

### Credit Card

The following credit card numbers can be used to test Visa or Mastercard payments.

Parameter | Value
---- | -----------
CVV | Any 3 digits
Date | any date in the future, before 2030
Number | 5017670000005900 <br> 5017670000006700 <br> 5017670000007500 <br> 5017670000008300

### Sofort Überweisung (Klarna)

The following online banking and account credentials can be used to test Sofort Überweisung (Klarna) payments.

Parameter | Value
---- | -----------
Country of your bank | Germany
Sort code or BIC | 88888888
Account number | 234567
PIN | 12345
TAN | 12345

### SEPA

Please use the following payment information to test both instant successful and failed payments. Be aware that the initial payment takes 5 days to process. During that time the payment will have the “pending” status. SEPA payments are available for reseller-type-accounts.

Description | IBAN
---- | -----------
To do an instant successful transaction | DE89370400440532013000
To do an instant failed transaction | DE62370400440532013001

### Paypal

Please use the following Paypal credentials to test payments on staging. Beware that you do not have an option to check the payment status on a Paypal account (by logging in to Paypal) unless you connect your own Paypal account on production.

Parameter | Value
---- | -----------
Username | info-buyer@elopage.com
Password | 123elopage

<aside class='notice'>
  NOTE: ACTIVATING PAYMENT METHODS ON PRODUCTION


  On production, please connect your Paypal account in your elopage Account - Settings - Payment methods. Watch video here: <a target='blank' href='https://www.youtube.com/watch?v=qAVf2vlGIHY'=>https://www.youtube.com/watch?v=qAVf2vlGIHY</a>.

  To get paid with Credit Cards, Sofort Überweisung and Bank wire, please activate your elopage payment account under Account - Settings - Payment. SEPA payments are only available for sellers that use the reseller program.
</aside>

## Best practice for testing

<b>Regardless of sandbox or production testing, we recommend applying the following best practice example for testing payments:</b>
<ol>
  <li>Create a sales page with one or two payment plans, for example a one-time payment option and an installment payment option.</li>
  <li>Get payment link info (URL to access payment page)</li>
  <li>Process a payment for a sales page using one of the payment methods for test credentials</li>
  <li>Get payment info using the Payment ID</li>
</ol>

<b>Check the following with your payment:</b>
<ol type='a'>
  <li>All customer information (address for example) are correct</li>
  <li>Payment method and amounts incl. VAT are correct</li>
  <li>You received the Invoice_ID and URL to display or send with an email</li>
  <li>Check status of payments: successful, pending, waiting, cancelled or error</li>
</ol>

We recommend testing each payment method and status.

# Product

## Create product

> Example usage:

```shell
curl -X POST \
  https://elopage.com/api/products \
  -H 'content-type: application/json' \
  -d '{
  "key":"{api_key}",
  "secret":"{api_secret}",
  "name":"product name",
  "success_url": "elopage.com",
  "cancel_url": "elopage.com",
  "error_url": "elopage.com",
  "webhook_url": "elopage.com",
  "pricing_plans": [
    {
      "form": "one_time",
      "preferences": {
        "price": "199.9",
        "old_price": "200"
       }
     }
   ],
   "authors": [
      {
        "id": 4,
        "custom_commission_enabled": true,
        "commission": 10
      }
    ],
   "success_email": {
	  "subject_de": "test",
	  "body_de": "<p>Hallo %{first_name} %{last_name},</p>\n<p><br></p>\n<p>vielen Dank f&uuml;r die Bestellung.</p>\n<p><br></p>\n<p>Produktname: %{product_name}</p>\n<p>Betrag: %{amount}</p>\n<p>Zahlung: %{recurring_type}</p>\n<p><br></p>\n<p>Bitte jetzt hier klicken:</p>\n<p>%{next_button}</p>\n<p><br></p>\n<p>Sch&ouml;ne Gr&uuml;&szlig;e,</p>",
	  "subject_en": "test",
	  "body_en": "<p>Hello %{first_name} %{last_name},</p>\n<p><br></p>\n<p>thanks for your order.</p>\n<p><br></p>\n<p>Product name: %{product_name}</p>\n<p>Amount: %{amount}</p>\n<p>Plan: %{recurring_type}</p>\n<p><br></p>\n<p>Now click here:</p>\n<p>%{next_button}</p>\n<p><br></p><p>Best regards,</p>"
   }
}'
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.post URI('https://elopage.com/api/products'),
               { "q" => "ruby", "max" => "50",
                 {
                  "key":"{api_key}",
                  "secret":"{api_secret}",
                  "name":"product name",
                  "success_url": "{success_url}",
                  "cancel_url": "{cancel_url}",
                  "error_url": "{error_url}",
                  "webhook_url": "{webhook_url}",
                  "pricing_plans": [
                    {
                      "form": "one_time",
                      "preferences": {
                        "price": "199.9",
                        "old_price": "200"
                       }
                     }
                   ],
                   "authors": [
                      {
                        "id": 4,
                        "custom_commission_enabled": true,
                        "commission": 10
                      }
                    ],
                    "success_email": {
                      "subject_de": "test",
                      "body_de": "<p>Hallo %{first_name} %{last_name},</p>
                                  <p><br></p>
                                  <p>vielen Dank f&uuml;r die Bestellung.</p>
                                  <p><br></p>
                                  <p>Produktname: %{product_name}</p>
                                  <p>Betrag: %{amount}</p>
                                  <p>Zahlung: %{recurring_type}</p>
                                  <p><br></p>
                                  <p>Bitte jetzt hier klicken:</p>
                                  <p>%{next_button}</p>
                                  <p><br></p>
                                  <p>Sch&ouml;ne Gr&uuml;&szlig;e,</p>",
                      "subject_en": "test",
                      "body_en": "<p>Hello %{first_name} %{last_name},</p>
                                  <p><br></p>
                                  <p>thanks for your order.</p>
                                  <p><br></p>
                                  <p>Product name: %{product_name}</p>
                                  <p>Amount: %{amount}</p>
                                  <p>Plan: %{recurring_type}</p>
                                  <p><br></p>
                                  <p>Now click here:</p>
                                  <p>%{next_button}</p>
                                  <p><br></p>
                                  <p>Best regards,</p>"
                      }
                    } }.to_json,
                    "Content-Type" => "application/json"
```

> Example response:

```
{
    "id": 1087,
    "url_to_pay": "https://elopage.com/s/elopage/product-name-1/payment"
}
```


The elopage API allows to initiate and complete payments using optimized checkout interfaces. In order to process payments you first need to create a product. After the product creation you will receive a 'url_to_pay' parameter to which you must redirect your customer to complete payment. Thereafter you need to redirect the customer back to your success URL and pass on parameters and information. Using webhooks (webhook_url), you will get instant notifications about the status of the payment.

### POST

`https://elopage.com/api/products`

### Parameter

Field       | Type | Description
----------- | --- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
name | String | What is the customer paying for? Enter a product name, plan name or similar. This name will be visible to your customer during the checkout process.
success_url | String | <b>Success URL</b> to which the customer will be redirected after completing the payment. Important: Updates for payments with payment methods that require time to process (for example Bank Wire / Vorkasse) must be continuously fetched using Payment IDs (payment_id) in order to receive the payment status <b>(/api/payments/:id)</b>.
cancel_url | String | <b>Cancel URL</b> to which your customer is redirected in case of payment process was cancelled by the customer.
error_url | String | <b>Error URL</b> to which your customer is redirected in case of error occurrences during the payment processing (for example bad credit card information, rejected card etc.).
webhook_url | String | The elopage API sends POST requests to this URL when there are updates related to the payment (for example bank wire payment updates to status successful). Transaction IDs and Payment IDs are included in the POST requests.
page_header | String | Use the page header to add content above the payment form (testimonials, campaign information etc.)  using html.
page_footer | String | Use the page footer to add further information and content below the payment form using html.
pricing_plans | Array | Array of objects which represent pricing plans of the product (at least one object required): <br> 'one_time' example pricing_plan:<br> `[{ "form": "one_time",`<br>`"preferences": `<br>`{ "price": "199.9", `<br>` "old_price": "200" } }]`<br> 'subscription' example pricing_plan: <br>`[{ "form": "subscription",`<br>` "preferences": `<br>`{ "first_interval": "1w", `<br>`"first_amount": "20.0",`<br>` "next_interval": "1m",`<br>` "next_amount": "10.0" } }]`<br> 'split' example (installment) pricing_plan:<br> `[{ "form": "split",`<br>` "preferences": `<br>`{ "p_count": "5",`<br>` "first_interval": "1w", `<br>`"first_amount": "20.0", `<br>`"next_interval": "1m",`<br>` "next_amount": "10.0",`<br>` "splitting_type": "installment" } }]`<br> 'split' example (limited_subscription) pricing_plan:<br> `[{ "form": "split", `<br>`"preferences": `<br>`{ "p_count": "5",`<br>` "first_interval": "1w",`<br>` "first_amount": "20.0", `<br>`"next_interval": "1m", `<br>`"next_amount": "10.0",`<br>` "splitting_type": "limited_subscription" } }]`<br> <span style='background-color: lightpink'>Note: Per product you are allowed to use maximum 5 pricing plans </span>.
&nbsp;&nbsp; form | String | Allowed values: `"one_time"`, `"subscription"`, `"split"`
&nbsp;&nbsp; preferences | Object | Apply different intervals for pricing plans with subscription and installments.
&nbsp;&nbsp;&nbsp;&nbsp; price | Decimal | price by current pricing plan (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; old_price | Decimal | old product price (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp; first_interval | String | first payment interval (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp; first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp; next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp; next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp; splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br> Allowed values: `["installment", "limited_subscription"]`
Authors | Array | Array of objects which represent the authors, that will be connected to the product
&nbsp;&nbsp; id | Integer | ID of the author, which you want to connect to the product (IDs of authors can be found in your dashboard under Team Members)
&nbsp;&nbsp; c_c_enabled | Boolean | Pass 'true' here if you want to overwrite general commission with this one, you should pass 'commission' for it to take effect
&nbsp;&nbsp; commission | Integer | Field which represents the commisison of author, measured in %
forward | Object |  [we can forward params with url_to_pay to prefill payment form, check this blog](http://blog.elopage.com/hilfebereich/was-bedeutet-die-url-parameter-weiterleitung/)
&nbsp;&nbsp; first_name | String | first name of customer
&nbsp;&nbsp; last_name | String | last name of customer
&nbsp;&nbsp; email | String | email of customer
&nbsp;&nbsp; coupon | String | coupon code to be applied automatically
&nbsp;&nbsp; campaign_id | String | campaign IDs can be freely added
success_email | Object | Payment confirmation or success email object contains data for the creation of custom emails which is automatically sent to the customer after successful payment. If success_email is not specified, elopage is sending out the standard email template which you can check in the product settings on elopage. For API usage, we do not recommend the usage of the standard template. Please customize your success email.  In the custom email you have the option to add your own email body using html in body_en for the English version and body_de for the German version. In addition, you can add several variables that will be replaced automatically with your customers or purchase information: First name of customer `first_name`, last name `last_name`, name of the product `product_name`, price paid amount and recurring payment type `recurring_type`.
&nbsp;&nbsp; subject_en | String | Subject of email in English
&nbsp;&nbsp; subject_de | String | Subject of email in German
&nbsp;&nbsp; body_en | String | Body of email in English
&nbsp;&nbsp; body_de | String | Body of email in German

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Number | Product ID.
url_to_pay | String | Product URL to which customer must be redirected

### Error 4xx

Name | Description
-----| -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

## Get product

Fetch product and all relevant information such as product name, pricing plans, authors and more.

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/products/{id}?key={api_key}&secret={api_secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/products/{id}?key={api_key}&secret={api_secret}'))
```

> Example response:

```
{
    "name": "product name",
    "url_to_pay": "https://elopage.com/s/elopage/product-name-1/payment",
    "success_url": "http://elopage.com",
    "cancel_url": "elopage.com",
    "error_url": "elopage.com",
    "webhook_url": "elopage.com",
    "free": false,
    "pricing_plans": [
        {
            id: "1",
            "form": "one_time",
            "preferences": {
                "price": "199.9",
                "old_price": "200.0"
            }
        }
    ],
    "authors": [
      {
          "author_id": 4,
          "c_c_enabled": true,
          "commission": "10.0"
      }
    ]
}
```

### GET

`https://elopage.com/api/products/:id`


### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key, you can find or generate them inside your elopage cabinet: Apps > Integrations
id | Number | Product ID

### Success 200

Field | Type | Description
----- | ---- | -----------
url_to_pay | String | Product URL to which customer must be redirected
name | String | What is the customer paying for? Enter a product name, plan name or similar. This name will be visible to your customer during the checkout process.
price | Decimal | The final price or amount you want to charge your customer incl. taxes, fees etc.
free | Boolean | Product for a free product
success_url | String | <b>Success URL</b> to which the customer will be redirected after completing the payment. Important: Updates for payments with payment methods that require time to process (for example Bank Wire / Vorkasse) must be continuously fetched using Payment IDs (payment_id) in order to receive the payment status <b>(/api/payments/:id)</b>.
cancel_url | String | <b>Cancel URL</b> to which your customer is redirected in case of payment process was cancelled by the customer.
error_url | String | <b>Error URL</b> to which your customer is redirected in case of error occurrences during the payment processing (for example bad credit card information, rejected card etc.).
webhook_url | String | elopage API sends POST request to this URL if payment for payment of product/name changes status to successful (e.g. bank wire updates).
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required)
&nbsp;&nbsp;ID | Integer | ID of the pricing plan
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "split"`
&nbsp;&nbsp;preferences | Object | Apply different intervals for pricing plans with subscription and installments.
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br> Allowed values: `["installment", "limited_subscription"]`
Authors | Array | Array of objects which represent the authors, that will be connected to the product
&nbsp;&nbsp; id | Integer | ID of the author, which you want to connect to the product (IDs of authors can be found in your dashboard under Team Members)
&nbsp;&nbsp; c_c_enabled | Boolean | Pass 'true' here if you want to overwrite general commission with this one, you should pass 'commission' for it to take effect
&nbsp;&nbsp; commission | Integer | Field which represents the commisison of author, measured in %

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.


## Get products

Fetch products list.

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/products?key={api_key}&secret={api_secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/products?key={api_key}&secret={api_secret}'))
```

> Example resppnse:

```
[
  {
      "id": 1646,
      "name": "Test digital",
      "url_to_pay": "https://elopage.com/s/testsellerregistration/test-digital-2/payment",
      "webhook_url": "https://webhook.site/e6f6c835-1786-448e-bc12-58048ecfcdc8",
      "active": true,
      "sold_count": 34,
      "created_at": "2018-07-31T11:48:15.789Z"
  },
  {
      "id": 1679,
      "name": "test product",
      "url_to_pay": "https://elopage.com/s/testsellerregistration/test-58/payment",
      "active": true,
      "sold_count": 0,
      "created_at": "2018-08-17T14:06:25.806Z"
  },
  {
      "id": 1682,
      "name": "test",
      "url_to_pay": "https://elopage.com/s/testsellerregistration/test-w4rwerwe/payment",
      "active": true,
      "sold_count": 0,
      "created_at": "2018-08-17T14:35:14.205Z"
  }
]

```

### GET

`https://elopage.com/api/products`


### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key, you can find or generate them inside your elopage cabinet: Apps > Integrations
id | Number | Product ID

### Success 200

Field | Type | Description
----- | ---- | -----------
url_to_pay | String | Product URL to which customer must be redirected
name | String | What is the customer paying for? Enter a product name, plan name or similar. This name will be visible to your customer during the checkout process.
price | Decimal | The final price or amount you want to charge your customer incl. taxes, fees etc.
free | Boolean | Product for a free product
success_url | String | <b>Success URL</b> to which the customer will be redirected after completing the payment. Important: Updates for payments with payment methods that require time to process (for example Bank Wire / Vorkasse) must be continuously fetched using Payment IDs (payment_id) in order to receive the payment status <b>(/api/payments/:id)</b>.
cancel_url | String | <b>Cancel URL</b> to which your customer is redirected in case of payment process was cancelled by the customer.
error_url | String | <b>Error URL</b> to which your customer is redirected in case of error occurrences during the payment processing (for example bad credit card information, rejected card etc.).
webhook_url | String | elopage API sends POST request to this URL if payment for payment of product/name changes status to successful (e.g. bank wire updates).
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required)
&nbsp;&nbsp;ID | Integer | ID of the pricing plan
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "split"`
&nbsp;&nbsp;preferences | Object | Apply different intervals for pricing plans with subscription and installments.
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br> Allowed values: `["installment", "limited_subscription"]`
Authors | Array | Array of objects which represent the authors, that will be connected to the product
&nbsp;&nbsp; id | Integer | ID of the author, which you want to connect to the product (IDs of authors can be found in your dashboard under Team Members)
&nbsp;&nbsp; c_c_enabled | Boolean | Pass 'true' here if you want to overwrite general commission with this one, you should pass 'commission' for it to take effect
&nbsp;&nbsp; commission | Integer | Field which represents the commisison of author, measured in %

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.





## Update product

> Example usage:

```shell
curl -X PUT \
  https://elopage.com/api/products/1115 \
  -H 'content-type: application/json' \
  -d '{
  "key":"{api_key}",
  "secret":"{api_secret}",
  "name":"product name",
  "success_url": "elopage.com",
  "cancel_url": "elopage.com",
  "error_url": "elopage.com",
  "webhook_url": "elopage.com",
    "pricing_plans": [
        {
            "id": 1340,
            "form": "one_time",
            "preferences": {
                "price": "220",
                "old_price": "200.0"
            }
        }
    ],
   "success_email": {
	  "subject_de": "test",
	  "body_de": "<p>Hallo %{first_name} %{last_name},</p>\n<p><br></p>\n<p>vielen Dank f&uuml;r die Bestellung.</p>\n<p><br></p>\n<p>Produktname: %{product_name}</p>\n<p>Betrag: %{amount}</p>\n<p>Zahlung: %{recurring_type}</p>\n<p><br></p>\n<p>Bitte jetzt hier klicken:</p>\n<p>%{next_button}</p>\n<p><br></p>\n<p>Sch&ouml;ne Gr&uuml;&szlig;e,</p>",
	  "subject_en": "test",
	  "body_en": "<p>Hello %{first_name} %{last_name},</p>\n<p><br></p>\n<p>thanks for your order.</p>\n<p><br></p>\n<p>Product name: %{product_name}</p>\n<p>Amount: %{amount}</p>\n<p>Plan: %{recurring_type}</p>\n<p><br></p>\n<p>Now click here:</p>\n<p>%{next_button}</p>\n<p><br></p><p>Best regards,</p>"
   }
}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://elopage.com/api/products/1115")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Put.new(url)
request["content-type"] = 'application/json'
request.body = {
                "key":"{api_key}",
                "secret":"{api_secret}",
                "name":"product name",
                "success_url": "elopage.com",
                "cancel_url": "elopage.com",
                "error_url": "elopage.com",
                "webhook_url": "elopage.com",
                "pricing_plans": [
                    {
                        "id": 1340,
                        "form": "one_time",
                        "preferences": {
                            "price": "220",
                            "old_price": "200.0"
                        }
                    }
                 ],
                 "success_email": {
                    "subject_de": "test",
                    "body_de": "<p>Hallo %{first_name} %{last_name},</p>\n<p><br></p>\n<p>vielen Dank f&uuml;r die Bestellung.</p>\n<p><br></p>\n<p>Produktname: %{product_name}</p>\n<p>Betrag: %{amount}</p>\n<p>Zahlung: %{recurring_type}</p>\n<p><br></p>\n<p>Bitte jetzt hier klicken:</p>\n<p>%{next_button}</p>\n<p><br></p>\n<p>Sch&ouml;ne Gr&uuml;&szlig;e,</p>",
                    "subject_en": "test",
                    "body_en": "<p>Hello %{first_name} %{last_name},</p>\n<p><br></p>\n<p>thanks for your order.</p>\n<p><br></p>\n<p>Product name: %{product_name}</p>\n<p>Amount: %{amount}</p>\n<p>Plan: %{recurring_type}</p>\n<p><br></p>\n<p>Now click here:</p>\n<p>%{next_button}</p>\n<p><br></p><p>Best regards,</p>"
                 }
                }

response = http.request(request)
puts response.read_body

```

> Example response:

```
{
    "id": 1087,
    "url_to_pay": "https://elopage.com/s/elopage/product-name-1/payment"
}
```


The elopage API allows creating products. To change or update the product information like price or name, please use put actions.

### PUT

`https://elopage.com/api/products/:id`

### Parameter

Field       | Type | Description
----------- | --- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
name | String | What is the customer paying for? Enter a product name, plan name or similar. This name will be visible to your customer during the checkout process.
success_url | String | <b>Success URL</b> to which the customer will be redirected after completing the payment. Important: Updates for payments with payment methods that require time to process (for example Bank Wire / Vorkasse) must be continuously fetched using Payment IDs (payment_id) in order to receive the payment status <b>(/api/payments/:id)</b>.
cancel_url | String | <b>Cancel URL</b> to which your customer is redirected in case of payment process was cancelled by the customer.
error_url | String | <b>Error URL</b> to which your customer is redirected in case of error occurrences during the payment processing (for example bad credit card information, rejected card etc.).
webhook_url | String | The elopage API sends POST requests to this URL when there are updates related to the payment (for example bank wire payment updates to status successful). Transaction IDs and Payment IDs are included in the POST requests.
page_header | String | Use the page header to add content above the payment form (testimonials, campaign information etc.)  using html.
page_footer | String | Use the page footer to add further information and content below the payment form using html.
pricing_plans | Array | Array of objects which represent pricing plans of the product (at least one object required): <br> 'one_time' example pricing_plan:<br> `[{ "form": "one_time",`<br>`"preferences": `<br>`{ "price": "199.9", `<br>` "old_price": "200" } }]`<br> 'subscription' example pricing_plan: <br>`[{ "form": "subscription",`<br>` "preferences": `<br>`{ "first_interval": "1w", `<br>`"first_amount": "20.0",`<br>` "next_interval": "1m",`<br>` "next_amount": "10.0" } }]`<br> 'split' example (installment) pricing_plan:<br> `[{ "form": "split",`<br>` "preferences": `<br>`{ "p_count": "5",`<br>` "first_interval": "1w", `<br>`"first_amount": "20.0", `<br>`"next_interval": "1m",`<br>` "next_amount": "10.0",`<br>` "splitting_type": "installment" } }]`<br> 'split' example (limited_subscription) pricing_plan:<br> `[{ "form": "split", `<br>`"preferences": `<br>`{ "p_count": "5",`<br>` "first_interval": "1w",`<br>` "first_amount": "20.0", `<br>`"next_interval": "1m", `<br>`"next_amount": "10.0",`<br>` "splitting_type": "limited_subscription" } }]`<br> <span style='background-color: lightpink'>Note: Per product you are allowed to use maximum 5 pricing plans </span>.
&nbsp;&nbsp; ID | Integer | ID of the pricing plan you want to edit
&nbsp;&nbsp; form | String | Allowed values: `"one_time"`, `"subscription"`, `"split"`
&nbsp;&nbsp; preferences | Object | Apply different intervals for pricing plans with subscription and installments.
&nbsp;&nbsp;&nbsp;&nbsp; price | Decimal | price by current pricing plan (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; old_price | Decimal | old product price (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp; first_interval | String | first payment interval (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp; first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp; next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp; next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp; splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b> One invoice for the entire amount will be issued </b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br>Allowed values: `["installment", "limited_subscription"]`
Authors | Array | Array of objects which represent the authors, that will be connected to the product
&nbsp;&nbsp; id | Integer | ID of the author, which you want to connect to the product (IDs of authors can be found in your dashboard under Team Members)
&nbsp;&nbsp; c_c_enabled | Boolean | Pass 'true' here if you want to overwrite general commission with this one, you should pass 'commission' for it to take effect
&nbsp;&nbsp; commission | Integer | Field which represents the commisison of author, measured in %
forward | Object | [we can forward params with url_to_pay to prefill payment form, check this blog](http://blog.elopage.com/hilfebereich/was-bedeutet-die-url-parameter-weiterleitung/)
&nbsp;&nbsp; first_name | String | first name of customer
&nbsp;&nbsp; last_name | String | last name of customer
&nbsp;&nbsp; email | String | email of customer
&nbsp;&nbsp; coupon | String | coupon code to be applied automatically
&nbsp;&nbsp; campaign_id | String | campaign IDs can be freely added
success_email | Object | Payment confirmation or success email object contains data for the creation of custom emails which is automatically sent to the customer after successful payment. If success_email is not specified, elopage is sending out the standard email template which you can check in the product settings on elopage. For API usage, we do not recommend the usage of the standard template. Please customize your success email.  In the custom email you have the option to add your own email body using html in body_en for the English version and body_de for the German version. In addition, you can add several variables that will be replaced automatically with your customers or purchase information: First name of customer `first_name`, last name `last_name`, name of the product `product_name`, price paid amount and recurring payment type `recurring_type`.
&nbsp;&nbsp; subject_en | String | Subject of email in English
&nbsp;&nbsp; subject_de | String | Subject of email in German
&nbsp;&nbsp; body_en | String | Body of email in English
&nbsp;&nbsp; body_de | String | Body of email in German

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Number | Product ID.
url_to_pay | String | Product URL to which customer must be redirected

### Error 4xx

Name | Description
-----| -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.



# Sales Page(Deprecated)

## Create sales page

> Example usage:

```shell
curl -X POST \
  https://elopage.com/api/sales_pages \
  -H 'content-type: application/json' \
  -d '{
  "key":"{api_key}",
  "secret":"{api_secret}",
  "name":"product name",
  "success_url": "elopage.com",
  "cancel_url": "elopage.com",
  "error_url": "elopage.com",
  "webhook_url": "elopage.com",
  "pricing_plans": [
    {
      "form": "one_time",
      "preferences": {
        "price": "199.9",
        "old_price": "200"
       }
     }
   ],
   "authors": [
      {
        "id": 4,
        "custom_commission_enabled": true,
        "commission": 10
      }
    ],
   "success_email": {
	  "subject_de": "test",
	  "body_de": "<p>Hallo %{first_name} %{last_name},</p>\n<p><br></p>\n<p>vielen Dank f&uuml;r die Bestellung.</p>\n<p><br></p>\n<p>Produktname: %{product_name}</p>\n<p>Betrag: %{amount}</p>\n<p>Zahlung: %{recurring_type}</p>\n<p><br></p>\n<p>Bitte jetzt hier klicken:</p>\n<p>%{next_button}</p>\n<p><br></p>\n<p>Sch&ouml;ne Gr&uuml;&szlig;e,</p>",
	  "subject_en": "test",
	  "body_en": "<p>Hello %{first_name} %{last_name},</p>\n<p><br></p>\n<p>thanks for your order.</p>\n<p><br></p>\n<p>Product name: %{product_name}</p>\n<p>Amount: %{amount}</p>\n<p>Plan: %{recurring_type}</p>\n<p><br></p>\n<p>Now click here:</p>\n<p>%{next_button}</p>\n<p><br></p><p>Best regards,</p>"
   }
}'
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.post URI('https://elopage.com/api/sales_pages'),
               { "q" => "ruby", "max" => "50",
                 {
                  "key":"{api_key}",
                  "secret":"{api_secret}",
                  "name":"product name",
                  "success_url": "{success_url}",
                  "cancel_url": "{cancel_url}",
                  "error_url": "{error_url}",
                  "webhook_url": "{webhook_url}",
                  "pricing_plans": [
                    {
                      "form": "one_time",
                      "preferences": {
                        "price": "199.9",
                        "old_price": "200"
                       }
                     }
                   ],
                   "authors": [
                      {
                        "id": 4,
                        "custom_commission_enabled": true,
                        "commission": 10
                      }
                    ],
                    "success_email": {
                      "subject_de": "test",
                      "body_de": "<p>Hallo %{first_name} %{last_name},</p>
                                  <p><br></p>
                                  <p>vielen Dank f&uuml;r die Bestellung.</p>
                                  <p><br></p>
                                  <p>Produktname: %{product_name}</p>
                                  <p>Betrag: %{amount}</p>
                                  <p>Zahlung: %{recurring_type}</p>
                                  <p><br></p>
                                  <p>Bitte jetzt hier klicken:</p>
                                  <p>%{next_button}</p>
                                  <p><br></p>
                                  <p>Sch&ouml;ne Gr&uuml;&szlig;e,</p>",
                      "subject_en": "test",
                      "body_en": "<p>Hello %{first_name} %{last_name},</p>
                                  <p><br></p>
                                  <p>thanks for your order.</p>
                                  <p><br></p>
                                  <p>Product name: %{product_name}</p>
                                  <p>Amount: %{amount}</p>
                                  <p>Plan: %{recurring_type}</p>
                                  <p><br></p>
                                  <p>Now click here:</p>
                                  <p>%{next_button}</p>
                                  <p><br></p>
                                  <p>Best regards,</p>"
                      }
                    } }.to_json,
                    "Content-Type" => "application/json"
```

> Example response:

```
{
    "id": 1087,
    "url_to_pay": "https://elopage.com/s/elopage/product-name-1/payment"
}
```


The elopage API allows to initiate and complete payments using optimized checkout interfaces. In order to process payments you first need to create a sales page. After the sales page creation you will receive a 'url_to_pay' parameter to which you must redirect your customer to complete payment. Thereafter you need to redirect the customer back to your success URL and pass on parameters and information. Using webhooks (webhook_url), you will get instant notifications about the status of the payment.

### POST

`https://elopage.com/api/sales_pages`

### Parameter

Field       | Type | Description
----------- | --- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
name | String | What is the customer paying for? Enter a product name, plan name or similar. This name will be visible to your customer during the checkout process.
success_url | String | <b>Success URL</b> to which the customer will be redirected after completing the payment. Important: Updates for payments with payment methods that require time to process (for example Bank Wire / Vorkasse) must be continuously fetched using Payment IDs (payment_id) in order to receive the payment status <b>(/api/payments/:id)</b>.
cancel_url | String | <b>Cancel URL</b> to which your customer is redirected in case of payment process was cancelled by the customer.
error_url | String | <b>Error URL</b> to which your customer is redirected in case of error occurrences during the payment processing (for example bad credit card information, rejected card etc.).
webhook_url | String | The elopage API sends POST requests to this URL when there are updates related to the payment (for example bank wire payment updates to status successful). Transaction IDs and Payment IDs are included in the POST requests.
page_header | String | Use the page header to add content above the payment form (testimonials, campaign information etc.)  using html.
page_footer | String | Use the page footer to add further information and content below the payment form using html.
pricing_plans | Array | Array of objects which represent pricing plans of the product (at least one object required): <br> 'one_time' example pricing_plan:<br> `[{ "form": "one_time",`<br>`"preferences": `<br>`{ "price": "199.9", `<br>` "old_price": "200" } }]`<br> 'subscription' example pricing_plan: <br>`[{ "form": "subscription",`<br>` "preferences": `<br>`{ "first_interval": "1w", `<br>`"first_amount": "20.0",`<br>` "next_interval": "1m",`<br>` "next_amount": "10.0" } }]`<br> 'split' example (installment) pricing_plan:<br> `[{ "form": "split",`<br>` "preferences": `<br>`{ "p_count": "5",`<br>` "first_interval": "1w", `<br>`"first_amount": "20.0", `<br>`"next_interval": "1m",`<br>` "next_amount": "10.0",`<br>` "splitting_type": "installment" } }]`<br> 'split' example (limited_subscription) pricing_plan:<br> `[{ "form": "split", `<br>`"preferences": `<br>`{ "p_count": "5",`<br>` "first_interval": "1w",`<br>` "first_amount": "20.0", `<br>`"next_interval": "1m", `<br>`"next_amount": "10.0",`<br>` "splitting_type": "limited_subscription" } }]`<br> <span style='background-color: lightpink'>Note: Per sales page you are allowed to use maximum 5 pricing plans </span>.
&nbsp;&nbsp; form | String | Allowed values: `"one_time"`, `"subscription"`, `"split"`
&nbsp;&nbsp; preferences | Object | Apply different intervals for pricing plans with subscription and installments.
&nbsp;&nbsp;&nbsp;&nbsp; price | Decimal | price by current pricing plan (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; old_price | Decimal | old product price (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp; first_interval | String | first payment interval (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp; first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp; next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp; next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp; splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br> Allowed values: `["installment", "limited_subscription"]`
Authors | Array | Array of objects which represent the authors, that will be connected to the sales page
&nbsp;&nbsp; id | Integer | ID of the author, which you want to connect to the sales page (IDs of authors can be found in your dashboard under Team Members)
&nbsp;&nbsp; c_c_enabled | Boolean | Pass 'true' here if you want to overwrite general commission with this one, you should pass 'commission' for it to take effect
&nbsp;&nbsp; commission | Integer | Field which represents the commisison of author, measured in %
forward | Object |  [we can forward params with url_to_pay to prefill payment form, check this blog](http://blog.elopage.com/hilfebereich/was-bedeutet-die-url-parameter-weiterleitung/)
&nbsp;&nbsp; first_name | String | first name of customer
&nbsp;&nbsp; last_name | String | last name of customer
&nbsp;&nbsp; email | String | email of customer
&nbsp;&nbsp; coupon | String | coupon code to be applied automatically
&nbsp;&nbsp; campaign_id | String | campaign IDs can be freely added
success_email | Object | Payment confirmation or success email object contains data for the creation of custom emails which is automatically sent to the customer after successful payment. If success_email is not specified, elopage is sending out the standard email template which you can check in the product settings on elopage. For API usage, we do not recommend the usage of the standard template. Please customize your success email.  In the custom email you have the option to add your own email body using html in body_en for the English version and body_de for the German version. In addition, you can add several variables that will be replaced automatically with your customers or purchase information: First name of customer `first_name`, last name `last_name`, name of the product `product_name`, price paid amount and recurring payment type `recurring_type`.
&nbsp;&nbsp; subject_en | String | Subject of email in English
&nbsp;&nbsp; subject_de | String | Subject of email in German
&nbsp;&nbsp; body_en | String | Body of email in English
&nbsp;&nbsp; body_de | String | Body of email in German

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Number | Sales page ID.
url_to_pay | String | Sales page URL to which customer must be redirected

### Error 4xx

Name | Description
-----| -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

## Get sales page

Fetch sales page and all relevant information such as product name, pricing plans, authors and more.

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/sales_pages/{id}?key={api_key}&secret={api_secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/sales_pages/{id}?key={api_key}&secret={api_secret}'))
```

> Example resppnse:

```
{
    "name": "product name",
    "url_to_pay": "https://elopage.com/s/elopage/product-name-1/payment",
    "success_url": "http://elopage.com",
    "cancel_url": "elopage.com",
    "error_url": "elopage.com",
    "webhook_url": "elopage.com",
    "free": false,
    "pricing_plans": [
        {
            id: "1",
            "form": "one_time",
            "preferences": {
                "price": "199.9",
                "old_price": "200.0"
            }
        }
    ],
    "authors": [
      {
          "author_id": 4,
          "c_c_enabled": true,
          "commission": "10.0"
      }
    ]
}
```

### GET

`https://elopage.com/api/sales_pages/:id`


### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key, you can find or generate them inside your elopage cabinet: Apps > Integrations
id | Number | Sales page ID

### Success 200

Field | Type | Description
----- | ---- | -----------
url_to_pay | String | Sales page URL to which customer must be redirected
name | String | What is the customer paying for? Enter a product name, plan name or similar. This name will be visible to your customer during the checkout process.
price | Decimal | The final price or amount you want to charge your customer incl. taxes, fees etc.
free | Boolean | Sales page for a free product
success_url | String | <b>Success URL</b> to which the customer will be redirected after completing the payment. Important: Updates for payments with payment methods that require time to process (for example Bank Wire / Vorkasse) must be continuously fetched using Payment IDs (payment_id) in order to receive the payment status <b>(/api/payments/:id)</b>.
cancel_url | String | <b>Cancel URL</b> to which your customer is redirected in case of payment process was cancelled by the customer.
error_url | String | <b>Error URL</b> to which your customer is redirected in case of error occurrences during the payment processing (for example bad credit card information, rejected card etc.).
webhook_url | String | elopage API sends POST request to this URL if payment for payment of product/name changes status to successful (e.g. bank wire updates).
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required)
&nbsp;&nbsp;ID | Integer | ID of the pricing plan
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "split"`
&nbsp;&nbsp;preferences | Object | Apply different intervals for pricing plans with subscription and installments.
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br> Allowed values: `["installment", "limited_subscription"]`
Authors | Array | Array of objects which represent the authors, that will be connected to the sales page
&nbsp;&nbsp; id | Integer | ID of the author, which you want to connect to the sales page (IDs of authors can be found in your dashboard under Team Members)
&nbsp;&nbsp; c_c_enabled | Boolean | Pass 'true' here if you want to overwrite general commission with this one, you should pass 'commission' for it to take effect
&nbsp;&nbsp; commission | Integer | Field which represents the commisison of author, measured in %

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.


## Get sales pages

Fetch sales pages list.

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/sales_pages?key={api_key}&secret={api_secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/sales_pages?key={api_key}&secret={api_secret}'))
```

> Example resppnse:

```
[
  {
      "id": 1646,
      "name": "Test digital",
      "url_to_pay": "https://elopage.com/s/testsellerregistration/test-digital-2/payment",
      "webhook_url": "https://webhook.site/e6f6c835-1786-448e-bc12-58048ecfcdc8",
      "active": true,
      "sold_count": 34,
      "created_at": "2018-07-31T11:48:15.789Z"
  },
  {
      "id": 1679,
      "name": "test product",
      "url_to_pay": "https://elopage.com/s/testsellerregistration/test-58/payment",
      "active": true,
      "sold_count": 0,
      "created_at": "2018-08-17T14:06:25.806Z"
  },
  {
      "id": 1682,
      "name": "test",
      "url_to_pay": "https://elopage.com/s/testsellerregistration/test-w4rwerwe/payment",
      "active": true,
      "sold_count": 0,
      "created_at": "2018-08-17T14:35:14.205Z"
  }
]

```

### GET

`https://elopage.com/api/sales_pages`


### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key, you can find or generate them inside your elopage cabinet: Apps > Integrations
id | Number | Sales page ID

### Success 200

Field | Type | Description
----- | ---- | -----------
url_to_pay | String | Sales page URL to which customer must be redirected
name | String | What is the customer paying for? Enter a product name, plan name or similar. This name will be visible to your customer during the checkout process.
price | Decimal | The final price or amount you want to charge your customer incl. taxes, fees etc.
free | Boolean | Sales page for a free product
success_url | String | <b>Success URL</b> to which the customer will be redirected after completing the payment. Important: Updates for payments with payment methods that require time to process (for example Bank Wire / Vorkasse) must be continuously fetched using Payment IDs (payment_id) in order to receive the payment status <b>(/api/payments/:id)</b>.
cancel_url | String | <b>Cancel URL</b> to which your customer is redirected in case of payment process was cancelled by the customer.
error_url | String | <b>Error URL</b> to which your customer is redirected in case of error occurrences during the payment processing (for example bad credit card information, rejected card etc.).
webhook_url | String | elopage API sends POST request to this URL if payment for payment of product/name changes status to successful (e.g. bank wire updates).
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required)
&nbsp;&nbsp;ID | Integer | ID of the pricing plan
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "split"`
&nbsp;&nbsp;preferences | Object | Apply different intervals for pricing plans with subscription and installments.
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br> Allowed values: `["installment", "limited_subscription"]`
Authors | Array | Array of objects which represent the authors, that will be connected to the sales page
&nbsp;&nbsp; id | Integer | ID of the author, which you want to connect to the sales page (IDs of authors can be found in your dashboard under Team Members)
&nbsp;&nbsp; c_c_enabled | Boolean | Pass 'true' here if you want to overwrite general commission with this one, you should pass 'commission' for it to take effect
&nbsp;&nbsp; commission | Integer | Field which represents the commisison of author, measured in %

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.





## Update sales page

> Example usage:

```shell
curl -X PUT \
  https://elopage.com/api/sales_pages/1115 \
  -H 'content-type: application/json' \
  -d '{
  "key":"{api_key}",
  "secret":"{api_secret}",
  "name":"product name",
  "success_url": "elopage.com",
  "cancel_url": "elopage.com",
  "error_url": "elopage.com",
  "webhook_url": "elopage.com",
    "pricing_plans": [
        {
            "id": 1340,
            "form": "one_time",
            "preferences": {
                "price": "220",
                "old_price": "200.0"
            }
        }
    ],
   "success_email": {
	  "subject_de": "test",
	  "body_de": "<p>Hallo %{first_name} %{last_name},</p>\n<p><br></p>\n<p>vielen Dank f&uuml;r die Bestellung.</p>\n<p><br></p>\n<p>Produktname: %{product_name}</p>\n<p>Betrag: %{amount}</p>\n<p>Zahlung: %{recurring_type}</p>\n<p><br></p>\n<p>Bitte jetzt hier klicken:</p>\n<p>%{next_button}</p>\n<p><br></p>\n<p>Sch&ouml;ne Gr&uuml;&szlig;e,</p>",
	  "subject_en": "test",
	  "body_en": "<p>Hello %{first_name} %{last_name},</p>\n<p><br></p>\n<p>thanks for your order.</p>\n<p><br></p>\n<p>Product name: %{product_name}</p>\n<p>Amount: %{amount}</p>\n<p>Plan: %{recurring_type}</p>\n<p><br></p>\n<p>Now click here:</p>\n<p>%{next_button}</p>\n<p><br></p><p>Best regards,</p>"
   }
}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://elopage.com/api/sales_pages/1115")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Put.new(url)
request["content-type"] = 'application/json'
request.body = {
                "key":"{api_key}",
                "secret":"{api_secret}",
                "name":"product name",
                "success_url": "elopage.com",
                "cancel_url": "elopage.com",
                "error_url": "elopage.com",
                "webhook_url": "elopage.com",
                "pricing_plans": [
                    {
                        "id": 1340,
                        "form": "one_time",
                        "preferences": {
                            "price": "220",
                            "old_price": "200.0"
                        }
                    }
                 ],
                 "success_email": {
                    "subject_de": "test",
                    "body_de": "<p>Hallo %{first_name} %{last_name},</p>\n<p><br></p>\n<p>vielen Dank f&uuml;r die Bestellung.</p>\n<p><br></p>\n<p>Produktname: %{product_name}</p>\n<p>Betrag: %{amount}</p>\n<p>Zahlung: %{recurring_type}</p>\n<p><br></p>\n<p>Bitte jetzt hier klicken:</p>\n<p>%{next_button}</p>\n<p><br></p>\n<p>Sch&ouml;ne Gr&uuml;&szlig;e,</p>",
                    "subject_en": "test",
                    "body_en": "<p>Hello %{first_name} %{last_name},</p>\n<p><br></p>\n<p>thanks for your order.</p>\n<p><br></p>\n<p>Product name: %{product_name}</p>\n<p>Amount: %{amount}</p>\n<p>Plan: %{recurring_type}</p>\n<p><br></p>\n<p>Now click here:</p>\n<p>%{next_button}</p>\n<p><br></p><p>Best regards,</p>"
                 }
                }

response = http.request(request)
puts response.read_body

```

> Example response:

```
{
    "id": 1087,
    "url_to_pay": "https://elopage.com/s/elopage/product-name-1/payment"
}
```


The elopage API allows creating sales pages. To change or update the sales page information like price or name, please use put actions.

### PUT

`https://elopage.com/api/sales_pages/:id`

### Parameter

Field       | Type | Description
----------- | --- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
name | String | What is the customer paying for? Enter a product name, plan name or similar. This name will be visible to your customer during the checkout process.
success_url | String | <b>Success URL</b> to which the customer will be redirected after completing the payment. Important: Updates for payments with payment methods that require time to process (for example Bank Wire / Vorkasse) must be continuously fetched using Payment IDs (payment_id) in order to receive the payment status <b>(/api/payments/:id)</b>.
cancel_url | String | <b>Cancel URL</b> to which your customer is redirected in case of payment process was cancelled by the customer.
error_url | String | <b>Error URL</b> to which your customer is redirected in case of error occurrences during the payment processing (for example bad credit card information, rejected card etc.).
webhook_url | String | The elopage API sends POST requests to this URL when there are updates related to the payment (for example bank wire payment updates to status successful). Transaction IDs and Payment IDs are included in the POST requests.
page_header | String | Use the page header to add content above the payment form (testimonials, campaign information etc.)  using html.
page_footer | String | Use the page footer to add further information and content below the payment form using html.
pricing_plans | Array | Array of objects which represent pricing plans of the product (at least one object required): <br> 'one_time' example pricing_plan:<br> `[{ "form": "one_time",`<br>`"preferences": `<br>`{ "price": "199.9", `<br>` "old_price": "200" } }]`<br> 'subscription' example pricing_plan: <br>`[{ "form": "subscription",`<br>` "preferences": `<br>`{ "first_interval": "1w", `<br>`"first_amount": "20.0",`<br>` "next_interval": "1m",`<br>` "next_amount": "10.0" } }]`<br> 'split' example (installment) pricing_plan:<br> `[{ "form": "split",`<br>` "preferences": `<br>`{ "p_count": "5",`<br>` "first_interval": "1w", `<br>`"first_amount": "20.0", `<br>`"next_interval": "1m",`<br>` "next_amount": "10.0",`<br>` "splitting_type": "installment" } }]`<br> 'split' example (limited_subscription) pricing_plan:<br> `[{ "form": "split", `<br>`"preferences": `<br>`{ "p_count": "5",`<br>` "first_interval": "1w",`<br>` "first_amount": "20.0", `<br>`"next_interval": "1m", `<br>`"next_amount": "10.0",`<br>` "splitting_type": "limited_subscription" } }]`<br> <span style='background-color: lightpink'>Note: Per sales page you are allowed to use maximum 5 pricing plans </span>.
&nbsp;&nbsp; ID | Integer | ID of the pricing plan you want to edit
&nbsp;&nbsp; form | String | Allowed values: `"one_time"`, `"subscription"`, `"split"`
&nbsp;&nbsp; preferences | Object | Apply different intervals for pricing plans with subscription and installments.
&nbsp;&nbsp;&nbsp;&nbsp; price | Decimal | price by current pricing plan (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; old_price | Decimal | old product price (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp; first_interval | String | first payment interval (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp; first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp; next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split), possible values: `‘1w’, ‘2w’, ‘3w’, ‘1m’, ‘2m’, ‘3m’, ‘6m’, ‘1y’, ‘2y’`
&nbsp;&nbsp;&nbsp;&nbsp; next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp; splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b> One invoice for the entire amount will be issued </b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br>Allowed values: `["installment", "limited_subscription"]`
Authors | Array | Array of objects which represent the authors, that will be connected to the sales page
&nbsp;&nbsp; id | Integer | ID of the author, which you want to connect to the sales page (IDs of authors can be found in your dashboard under Team Members)
&nbsp;&nbsp; c_c_enabled | Boolean | Pass 'true' here if you want to overwrite general commission with this one, you should pass 'commission' for it to take effect
&nbsp;&nbsp; commission | Integer | Field which represents the commisison of author, measured in %
forward | Object | [we can forward params with url_to_pay to prefill payment form, check this blog](http://blog.elopage.com/hilfebereich/was-bedeutet-die-url-parameter-weiterleitung/)
&nbsp;&nbsp; first_name | String | first name of customer
&nbsp;&nbsp; last_name | String | last name of customer
&nbsp;&nbsp; email | String | email of customer
&nbsp;&nbsp; coupon | String | coupon code to be applied automatically
&nbsp;&nbsp; campaign_id | String | campaign IDs can be freely added
success_email | Object | Payment confirmation or success email object contains data for the creation of custom emails which is automatically sent to the customer after successful payment. If success_email is not specified, elopage is sending out the standard email template which you can check in the product settings on elopage. For API usage, we do not recommend the usage of the standard template. Please customize your success email.  In the custom email you have the option to add your own email body using html in body_en for the English version and body_de for the German version. In addition, you can add several variables that will be replaced automatically with your customers or purchase information: First name of customer `first_name`, last name `last_name`, name of the product `product_name`, price paid amount and recurring payment type `recurring_type`.
&nbsp;&nbsp; subject_en | String | Subject of email in English
&nbsp;&nbsp; subject_de | String | Subject of email in German
&nbsp;&nbsp; body_en | String | Body of email in English
&nbsp;&nbsp; body_de | String | Body of email in German

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Number | Sales page ID.
url_to_pay | String | Sales page URL to which customer must be redirected

### Error 4xx

Name | Description
-----| -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

# Publishers

## Get publishers list

Fetch publishers list.

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/publishers?key={api_key}&secret={api_secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/publishers?key={api_key}&secret={api_secret}'))
```

> Example resppnse:

```
[
    {
        "id": 306,
        "first_name": "Test",
        "last_name": "Publisher",
        "email": "testpublisher@gmail.com",
        "program_ids": "129"
    },
    {
        "id": 266,
        "first_name": "New",
        "last_name": "Publisher",
        "email": "newpublisher@gmail.com",
        "program_ids": "129"
    }
]

```

### GET

`https://elopage.com/api/publishers`


### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key, you can find or generate them inside your elopage cabinet: Apps > Integrations

### Success 200

Field | Type | Description
----- | ---- | -----------
id | String | Publisher ID
first_name | String | Publisher first name
last_name | String | Publisher last name
email | String | Publisher email
program_ids | Array | Affiliate program ids, in which publisher is currently enrolled
address | String | Publisher address
zip | String | Publisher zip
city | String | Publisher city
phone | String | Publisher phone
tax_no | String | Publisher tax number
vat_no | String | Publisher vat number

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.


## Enroll publisher to program

> Example usage:

```shell
curl -X POST \
  https://elopage.com/api/publishers/{id}/enroll \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F key={api_key} \
  -F secret={api_secret} \
  -F affiliate_program_id="{affiliate_program_id}"
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://elopage.com/api/publishers/{id}/enroll")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["content-type"] = 'multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW'
request["cache-control"] = 'no-cache'
request.body = {
  "key":"{api_key}",
  "secret":"{api_secret}",
  "affiliate_program_id": "{affiliate_program_id}"
}

response = http.request(request)
puts response.read_body
```

> Example response:

```
{
    "success": true,
    "msg": "Publisher successfully enrolled in program 15 Multilevel program (no bonus)"
}
```


The elopage API allows unenrolling publishers from your affiliate programs.

### POST

`https://elopage.com/api/publishers/:id/enroll`

### Parameter

Field       | Type | Description
----------- | --- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
affiliate_program_id | String | Program, to which publisher should be enrolled after a successful call

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Number | Product ID.
success | Boolean | Field which determines if the call was successful or not
msg | String | Server answer to the request

### Error 4xx

Name | Description
-----| -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

## Unenroll publisher from program

> Example usage:

```shell
curl -X POST \
  https://elopage.com/api/publishers/{id}/unenroll \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F key={api_key} \
  -F secret={api_secret} \
  -F affiliate_program_id="{affiliate_program_id}"
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://elopage.com/api/publishers/{id}/unenroll")

http = Net::HTTP.new(url.host, url.port)

request = Net::HTTP::Post.new(url)
request["content-type"] = 'multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW'
request["cache-control"] = 'no-cache'
request.body = {
  "key":"{api_key}",
  "secret":"{api_secret}",
  "affiliate_program_id": "{affiliate_program_id}"
}

response = http.request(request)
puts response.read_body
```

> Example response:

```
{
    "success": true,
    "msg": "Publisher successfully unenrolled from program 15 Multilevel program (no bonus)"
}
```


The elopage API allows unenrolling publishers for your affiliate programs.

### POST

`https://elopage.com/api/publishers/:id/unenroll`

### Parameter

Field       | Type | Description
----------- | --- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
affiliate_program_id | String | Program, to which publisher should be enrolled after a successful call

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Number | Product ID.
success | Boolean | Field which determines if the call was successful or not
msg | String | Server answer to the request

### Error 4xx

Name | Description
-----| -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

# Pricing plans
Pricing plans are created during the creation of sales. In addition, you can create pricing plans separately, view them and delete pricing plans. elopage pricing plans allow the setup of multiple options such as time-limited subscriptions, installment payments and one time payments. Furthermore you can create and attach up to 5 pricing plans for one sales page.

## Get pricing plan
> Example usage:

```shell
curl -X GET -H 'https://elopage.com/api/pricing_plans/{id}?key={api_key}&secret={api_secret}'
```

```ruby
  require 'uri'
  require 'net/http'

  Net::HTTP.get(URI("https://elopage.com/api/pricing_plans/{id}?key={api_key}&secret={api_secret}"))
```

> Example response:

```
{
    "id": 1306,
    "form": "one_time",
    "preferences": {
        "price": "199.9",
        "old_price": "200.0"
    }
}
```

Fetch pricing plan information by ID to get all relevant information about your pricing plan such as prices, intervals and splitting type.

### GET

`https://elopage.com/api/pricing_plans/:id`

### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in your dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
id | Number | Pricing plan ID is sent with the get sales page request.


### Success 200

Field | Type | Description
----- | ---- | -----------
id    | Integer | Pricing plan id
form | String | Allowed values: `"one_time", "subscription", "split"`
preferences | Object | Apply different intervals for pricing plans with subscription and installments.&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/split)
&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split)
&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued </b> <br> Allowed values: `["installment", "limited_subscription"]`


### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.


## Create pricing plan
Create pricing plans by sending POST requests with parameters. All pricing plans must be immediately attached to a sales page.
> Example usage:

```shell
curl -X POST \
  http://localhost:3000/api/pricing_plans \
  -H 'content-type: application/json' \
  -d '{
	"key": "{key}",
	"secret": "{secret}",
	"sales_page_id": 1087,
	"pricing_plan": {
		"form": "split",
		"preferences": {
			"p_count": 2,
			"first_amount": 0.5,
			"next_amount": 2
		}
	}
}'
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://elopage.com/api/pricing_plans")
request = Net::HTTP::Post.new(uri)
request.body = JSON.dump({
  "key" => "{api_key}",
  "secret" => "{api_secret}",
  "sales_page_id" => 1087,
  "pricing_plan" => {
    "form" => "split",
    "preferences" => {
      "p_count" => 2,
      "first_amount" => 0.5,
      "next_amount" => 2
    }
  }
})

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```

> Example response:

```
{
    "id": 1312,
    "form": "splited",
    "preferences": {
        "p_count": "2",
        "first_amount": "0.5",
        "next_amount": "2.0"
    }
}
```

### POST
`https://elopage.com/api/pricing_plans`

### Parameter

Field | Type | Description
----- | ---- | -----------
sales_page_id | Integer | ID of sales page for which you create this pricing plan
pricing_plan | Object | Pricing plan object
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "split"`
&nbsp;&nbsp;preferences | Object | Apply different intervals for pricing plans with subscription and installments.&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;&nbsp;&nbsp;splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br> Allowed values: `["installment", "limited_subscription"]`

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Integer | Created pricing plan ID
form | String | Allowed values: `"one_time", "subscription", "split"`
preferences | Object | Apply different intervals for pricing plans with subscription and installments.&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: split)
&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/split)
&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/split)
&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/split)
&nbsp;&nbsp;splitting_type | String | splitting type (required for pricing plan form: split); notes: "installment" - <b>One invoice for the entire amount will be issued</b>, "limited_subscription" - <b>For each payment a new invoice will be issued</b> <br> Allowed values: `["installment", "limited_subscription"]`

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.


## Delete pricing plan
You can delete pricing plans with a simple delete request.
> Example usage:

```shell
curl -X DELETE \
  https://elopage.com/api/pricing_plans/:id \
  -d '{
	"key": "api_key",
	"secret": "api_secret"
}'
```

```ruby

require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://elopage.com/api/pricing_plans/:id")
request = Net::HTTP::Delete.new(uri)
request.body = JSON.dump({
  "key" => "api_key",
  "secret" => "api_secret"
})

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```

> Example response:

```
{
    "status": "success",
    "response": "Pricing plan 1313 was successfully destroyed"
}
```

### DELETE
`https://elopage.com/api/pricing_plans/:id`

### Parameter

Field | Type | Description
----- | ---- | -----------
id | Integer | ID of the pricing plan you want to delete

### Success 200

Field | Type | Description
----- | ---- | -----------
status | string | status of the request, possible values: `'success', 'error'`
response | string | response text

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.


# Payments

## Get payment info

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/payments/{id}?key={api_key}&secret={api_secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/payments/{id}?key={api_key}&secret={api_secret}'))
```

> Example response:

```
{
    "id": 540391,
    "payer": {
        "email": "jonhdoe@doe.com",
        "first_name": "John",
        "last_name": "Done",
        "country": "Berlin",
        "city": "Germany",
        "street": "Joachimsthaler Straße 21",
        "zip": "10719",
        "company": null,
        "vat_id": null,
        "phone": "+4930398204650"
    },
    "revenue": 199.9,
    "amount": 194.1,
    "fee": 5.8,
    "recurring": false,
    "recurring_form": "one_time",
    "payment_method": "paypal",
    "payment_session_id": "1234",
    "sales_page_id": "1087",
    "state": "successful",
    "created_date": "2017-12-11T15:47Z",
    "success_date": "2017-12-11T15:47Z",
    "success_date_short": "2017-12-11",
    "invoice_link": "https://elopage.com/common/invoices/2879?token=uVUai-84pyDhdz8K3nNd",
    "success_link": "https://elopage.com/s/elopage/product-name-6/payment_success?token=uVUai-84pyDhdz8K3nNd"
    "author_commissions": [
      {
          "id": 1,
          "rate": "10.0",
          "amount": "19.385",
          "payment_id": 542711
      }
    ]
}
```
Getting the payment information by fetching the transaction or payment info is a critical use of the API. In the following you can find out which requests are needed to get the necessary information to deliver your content, product or service.

### GET

`https://elopage.com/api/payments/:id`

### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in your dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
id | Number | Payment ID

### Success 200

Field | Type | Description
----- | ---- | -----------
payer | Object | customer's object
&nbsp;&nbsp;email | String | customer's email
&nbsp;&nbsp;first_name | String | customer's first name
&nbsp;&nbsp;last_name | String | customer's last name
&nbsp;&nbsp;country | String | customer's country
&nbsp;&nbsp;city | String | customer's city
&nbsp;&nbsp;street | String | customer's street
&nbsp;&nbsp;zip | String | customer's zip code
&nbsp;&nbsp;company | String | customer's company name
&nbsp;&nbsp;vat_id | String | customer's vat ID
&nbsp;&nbsp;phone | String | customer's phone number
revenue | Decimal | gross revenue
amount | Decimal | net revenue
fee | Decimal | fees
recurring | Boolean | Returns true if it is a recurring type payment
recurring_form | String | Returns the recurring type of the transaction. This value is being taken from the pricing plan form(`one_time, subscription, split`), which was chosen by customer during the checkout process.
payment_method | String | card, bank_account, paypal, sofort, bank_wire
payment_session_id | Integer | ID of payment session to which the payment belongs to. You should save it for the recurring payments as recurring payments (next or follow up payments) will have the same session.
sales_page_id | Integer | ID of sales page that is connected to this payment
state | String | <b>Waiting:</b> shown when a payment is not yet successful. The status waiting can appear when:</br><ul><li>A bank wire was initiated</li><li>A Pay Later payment was initiated</li></ul><b>Successful:</b> Shown when a payment was initiated and successfully processed</br><b>Pending:</b> Shown only when a Paypal payment was initiated, but not yet processed. Once the payment is successfully processed, the status becomes successful</br><ul><li>A SEPA payment was initiated</li><li>A Paypal payment was initiated but not finished. Once the payment is successfully processed, the state becomes successful</li></ul><b>Canceled:</b> Shown when a payment has been initiated, but canceled before being successfully processed</br><b>Error:</b> Shown when an issue appeared, preventing the payment from being initiated. After 10 days without being processed, waiting payments state changes to error.
created_date | String | Date of payment initiation
success_date | String | Date and time of completed payment
success_date_short | String | Date of completed payment
invoice_link | String | This is the invoice URL for automatically generated invoices. Make sure to setup the invoice generation in your dashboard (Setting - Invoice generation) to get the invoice URLs.
author_commissions | Array | Refunded commission that was previously paid to the author
&nbsp;&nbsp; id | Integer | Author commission ID
&nbsp;&nbsp; rate | Integer | Commission rate in percentage with which commission was calculated
&nbsp;&nbsp; amount | Integer | Amount earned by author with this payment
&nbsp;&nbsp; payment_id | Integer | ID of payment to which commissions are connected
### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

# Refunds

You can refund completed payments or transactions. Refunds will be processed via the same payment method that was preferred by the customer.

## Create a refund
Create a refund using the following method. An automated email from the elopage system will be sent to notify the customer about the refund. This email also includes an appropriate credit memo.

> Example usage:

```shell
curl -X POST \
  https://elopage.com/api/payments/{:id}/refund \
  -H 'content-type: application/json' \
  -d '{
  "key":"{API key}",
  "secret":"{API secret}"
}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://elopage.com/api/payments/{:id}/refund")
http = Net::HTTP.new(url.host, url.port)
request = Net::HTTP::Post.new(url)
request.body = {
                  key: "{API key}",
                  secret: "{API secret}"
               }
response = http.request(request)
puts response.read_body
```

> Example response:

```
{
  "payer": {
      "email": "jonhdoe@doe.com",
      "first_name": "John",
      "last_name": "Done",
      "country": "Berlin",
      "city": "Germany",
      "street": "Joachimsthaler Straße 21",
      "zip": "10719",
      "company": null,
      "vat_id": null,
      "phone": "+4930398204650"
  },
  "total": "-199.9",
  "amount": "-199.9",
  "fee": 0,
  "id": 542652,
  "payment_method": "credit_card",
  "payment_session_id": 3913,
  "sales_page_id": "1087",
  "refunded_tansfer_id": 542180,
  "state": "successful"
},
"author_commissions": [
    {
        "id": 2,
        "rate": "10.0",
        "amount": "-19.385",
        "payment_id": 542760
    }
]
```

### POST

`https://elopage.com/api/payments/:id/refund`

### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in your dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
id | Number | Payment ID

### Success 200

Field | Type | Description
----- | ---- | -----------
payer | Object | customer's object
&nbsp;&nbsp;email | String | customer's email
&nbsp;&nbsp;first_name | String | customer's first name
&nbsp;&nbsp;last_name | String | customer's last name
&nbsp;&nbsp;country | String | customer's country
&nbsp;&nbsp;city | String | customer's city
&nbsp;&nbsp;street | String | customer's street
&nbsp;&nbsp;zip | String | customer's zip code
&nbsp;&nbsp;company | String | customer's company name
&nbsp;&nbsp;vat_id | String | customer's vat ID
&nbsp;&nbsp;phone | String | pacustomeryer phone
total | Decimal | brutto amount
amount | Decimal | netto amount
fee | Decimal | fees
payment_method | String | card, bank_account, paypal, sofort, bank_wire
payment_session_id | Integer | ID of payment session to which the payment belongs to. You should save it for the recurring payments as recurring payments (next or follow up payments) will have the same session.
sales_page_id | Integer | ID of sales page that is connected to this payment
refunded_transfer_id | Integer | ID of the transfer which is being refunded
state | String | <b>Waiting:</b> shown when a payment is not yet successful. The status waiting can appear when:</br><ul><li>A bank wire was initiated</li><li>A Pay Later payment was initiated</li></ul><b>Successful:</b> Shown when a payment was initiated and successfully processed</br><b>Pending:</b> Shown only when a Paypal payment was initiated, but not yet processed. Once the payment is successfully processed, the status becomes successful</br><ul><li>A SEPA payment was initiated</li><li>A Paypal payment was initiated but not finished. Once the payment is successfully processed, the state becomes successful</li></ul><b>Canceled:</b> Shown when a payment has been initiated, but canceled before being successfully processed</br><b>Error:</b> Shown when an issue appeared, preventing the payment from being initiated. After 10 days without being processed, waiting payments state changes to error.
author_commissions | Array | Refunded commission that was previously paid to the author
&nbsp;&nbsp; id | Integer | Author commission ID
&nbsp;&nbsp; rate | Integer | Commission rate in percentage with which commission was calculated
&nbsp;&nbsp; amount | Integer | Amount earned by author with this payment which is being refunded
&nbsp;&nbsp; payment_id | Integer | ID of payment to which commissions are connected

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

## Get refund info

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/payments/{id}?key={api_key}&secret={api_secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/payments/{id}?key={api_key}&secret={api_secret}'))
```

> Example response:

```
{
  "payer": {
      "email": "jonhdoe@doe.com",
      "first_name": "John",
      "last_name": "Done",
      "country": "Berlin",
      "city": "Germany",
      "street": "Joachimsthaler Straße 21",
      "zip": "10719",
      "company": null,
      "vat_id": null,
      "phone": "+4930398204650"
  },
  "total": "-199.9",
  "amount": "-199.9",
  "fee": 0,
  "id": 542652,
  "payment_method": "credit_card",
  "payment_session_id": 3913,
  "sales_page_id": "1087",
  "refunded_tansfer_id": 542180,
  "state": "successful"
},
"author_commissions": [
    {
        "id": 2,
        "rate": "10.0",
        "amount": "-19.385",
        "payment_id": 542760
    }
]

```
You can fetch refund transactions by IDs. Please use the following method and parameters.

### GET

`https://elopage.com/api/payments/:id`

### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in your dashboard under Apps > Integrations
key | String | Your personal elopage API key which you can generate in your dashboard: Apps > Integrations
id | Number | Payment ID

### Success 200

Field | Type | Description
----- | ---- | -----------
payer | Object | customer's object
&nbsp;&nbsp;email | String | customer's email
&nbsp;&nbsp;first_name | String | customer's first name
&nbsp;&nbsp;last_name | String | customer's last name
&nbsp;&nbsp;country | String | customer's country
&nbsp;&nbsp;city | String | customer's city
&nbsp;&nbsp;street | String | customer's street
&nbsp;&nbsp;zip | String | customer's zip code
&nbsp;&nbsp;company | String | customer's company name
&nbsp;&nbsp;vat_id | String | customer's vat ID
&nbsp;&nbsp;phone | String | customer's phone number
revenue | Decimal | gross revenue
amount | Decimal | net revenue
fee | Decimal | fees
recurring | Boolean | Returns true if it is a recurring type payment
recurring_form | String | Returns the recurring type of the transaction. This value is being taken from the pricing plan form(`one_time, subscription, split`), which was chosen by customer during the checkout process.
payment_method | String | card, bank_account, paypal, sofort, bank_wire
payment_session_id | Integer | ID of payment session to which the payment belongs to. You should save it for the recurring payments as recurring payments (next or follow up payments) will have the same session.
sales_page_id | Integer | ID of sales page that is connected to this payment
state | String | <b>Waiting:</b> shown when a payment is not yet successful. The status waiting can appear when:</br><ul><li>A bank wire was initiated</li><li>A Pay Later payment was initiated</li></ul><b>Successful:</b> Shown when a payment was initiated and successfully processed</br><b>Pending:</b> Shown only when a Paypal payment was initiated, but not yet processed. Once the payment is successfully processed, the status becomes successful</br><ul><li>A SEPA payment was initiated</li><li>A Paypal payment was initiated but not finished. Once the payment is successfully processed, the state becomes successful</li></ul><b>Canceled:</b> Shown when a payment has been initiated, but canceled before being successfully processed</br><b>Error:</b> Shown when an issue appeared, preventing the payment from being initiated. After 10 days without being processed, waiting payments state changes to error.
created_date | String | Date of payment initiation
success_date | String | Date and time of completed payment
success_date_short | String | Date of completed payment
invoice_link | String | This is the invoice URL for automatically generated invoices. Make sure to setup the invoice generation in your dashboard (Setting - Invoice generation) to get the invoice URLs.
author_commissions | Array | Refunded commission that was previously paid to the author
&nbsp;&nbsp; id | Integer | Author commission ID
&nbsp;&nbsp; rate | Integer | Commission rate in percentage with which commission was calculated
&nbsp;&nbsp; amount | Integer | Amount earned by author with this payment which is being refunded
&nbsp;&nbsp; payment_id | Integer | ID of payment to which commissions are connected
### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

# Webhooks

Use webhooks to receive instant notification of events related to payments and statuses such as successful, error or waiting/pending payments. Depending on the payment method, the status can change from waiting to successul or error or from pending to successful or error. Check parameters and methods below.

### Message body

Field | Type | Description
----- | ---- | -----------
payer | Object | customer's object
&nbsp;&nbsp;email | String | customer's email
&nbsp;&nbsp;first_name | String | customer's first name
&nbsp;&nbsp;last_name | String | customer's last name
&nbsp;&nbsp;country | String | customer's country
&nbsp;&nbsp;city | String | customer's city
&nbsp;&nbsp;street | String | customer's street
&nbsp;&nbsp;zip | String | customer's zip code
&nbsp;&nbsp;company | String | customer's company name
&nbsp;&nbsp;vat_id | String | customer's vat ID
&nbsp;&nbsp;phone | String | customer's phone number
revenue | Decimal | gross revenue
amount | Decimal | net revenue
fee | Decimal | fees
recurring | Boolean | Returns true if it is a recurring type payment
recurring_form | String | Returns the recurring type of the transaction. This value is being taken from the pricing plan form(`one_time, subscription, split`), which was chosen by customer during the checkout process.
payment_method | String | card, bank_account, paypal, sofort, bank_wire
payment_session_id | Integer | ID of payment session to which the payment belongs to. You should save it for the recurring payments as recurring payments (next or follow up payments) will have the same session.
sales_page_id | Integer | ID of sales page that is connected to this payment
state | String | <b>Waiting:</b> shown when a payment is not yet successful. The status waiting can appear when:</br><ul><li>A bank wire was initiated</li><li>A Pay Later payment was initiated</li></ul><b>Successful:</b> Shown when a payment was initiated and successfully processed</br><b>Pending:</b> Shown only when a Paypal payment was initiated, but not yet processed. Once the payment is successfully processed, the status becomes successful</br><ul><li>A SEPA payment was initiated</li><li>A Paypal payment was initiated but not finished. Once the payment is successfully processed, the state becomes successful</li></ul><b>Canceled:</b> Shown when a payment has been initiated, but canceled before being successfully processed</br><b>Error:</b> Shown when an issue appeared, preventing the payment from being initiated. After 10 days without being processed, waiting payments state changes to error.
created_date | String | Date of payment initiation
success_date | String | Date and time of completed payment
success_date_short | String | Date of completed payment
invoice_link | String | This is the invoice URL for automatically generated invoices. Make sure to setup the invoice generation in your dashboard (Setting - Invoice generation) to get the invoice URLs.
author_commissions | Array | Refunded commission that was previously paid to the author
&nbsp;&nbsp; id | Integer | Author commission ID
&nbsp;&nbsp; rate | Integer | Commission rate in percentage with which commission was calculated
&nbsp;&nbsp; amount | Integer | Amount earned by author with this payment
&nbsp;&nbsp; payment_id | Integer | ID of payment to which commissions are connected


# Thanks!
We look forward to your questions and feedback. The elopage API is yet making its baby steps and we value all types of feedback from developers. Feel free to contact us via support@elopage.com. Thanks for using our API.
