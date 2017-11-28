---
title: API Reference

language_tabs:
  - shell
  - ruby
  - python
search: true
---

# elopage API (beta)

Process payments easily with the elopage API and collect amounts in your account - payouts only via dashboard

# Get started now

Create payment links, get paid, find and search transaction ID / payment IDs and more is following. For questions please contact support@elopage.com.

# PaymentLinks

## Create payment link

> Example usage:

```shell
curl -X POST -H "Content-Type: application/json" "https://elopage.com/api/payment_links"
-d '{
"key":"{your API key}",
"secret":"{API secret}",
"name":"product name",
"success_url": "{success_url}",
"cancel_url": "{cancel_url}",
"error_url": "{error_url}",
"ping_url": "{ping_url}",
"pricing_plans": [
  {
    "form": "one_time",
    "preferences": {
      "price": "199.9",
      "old_price": "200"
     }
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
}'
```

```python
require 'net/http'
require 'uri'

Net::HTTP.post URI('https://elopage.com/api/payment_links'),
               { "q" => "ruby", "max" => "50",
                 {
                  "key":"{your API key}",
                  "secret":"{API secret}",
                  "name":"product name",
                  "success_url": "{success_url}",
                  "cancel_url": "{cancel_url}",
                  "error_url": "{error_url}",
                  "ping_url": "{ping_url}",
                  "pricing_plans": [
                    {
                      "form": "one_time",
                      "preferences": {
                        "price": "199.9",
                        "old_price": "200"
                       }
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



```ruby
require 'net/http'
require 'uri'

Net::HTTP.post URI('https://elopage.com/api/payment_links'),
               { "q" => "ruby", "max" => "50",
                 {
                  "key":"{your API key}",
                  "secret":"{API secret}",
                  "name":"product name",
                  "success_url": "{success_url}",
                  "cancel_url": "{cancel_url}",
                  "error_url": "{error_url}",
                  "ping_url": "{ping_url}",
                  "pricing_plans": [
                    {
                      "form": "one_time",
                      "preferences": {
                        "price": "199.9",
                        "old_price": "200"
                       }
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

The elopage API allows to initiative payments and complete payments on its optimized payment interfaces. In order to process payments you first need to create a payment link. With the payment link creation you receive the 'url_to_pay' parameter to which you must redirect the payer to complete payment. There after you can redirect the user back to your URL and forward information.

<aside class="warning">WARNING: please do not use this payment_link as created product - you can create payment_link for only one payment, and after the payment it won't be avaliable</aside>

### POST

`https://elopage.com/api/payment_links`

### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage API secret which can be generated and found in the sellers dashboard under Settings > Integrations
key | String | Your personal elopage API key which can be generated and found in the sellers dashboard under Settings > Integrations
name | String | What is the payer paying for? Name the payment by entering a product name, plan name or similar. This name will be visible to your payer during the checkout process.
success_url | String | URL to which the payer or payer will be redirected after successfully completing payment. Attention: Payments with some payment methods require time to process. Due to this reason you must attach parameters to the redirection URLs to fetch updates of payment IDs. First is Payment ID `payment_id` which you can you fetch in order to receive the payment status `/api/payments/:id`.
cancel_url | String | URL to which payer is redirected to in case payment process was cancelled by payer.
error_url | String | URL to which payer is redirected in case of errors during payment process (e.g. Bad credit card info, rejected by pop etc.)
ping_url | String | elopage API sends POST request to this URL if payment for payment of product/name once it changes status to successful (e.g. bank wire updates). Also we will add transation id and payment id to POST request.
price | Decimal | Will be deprecated after 01.01.2018 use pricing_plans instead
recurring_prices | Object | Will be deprecated after 01.01.2018 including all its subparams - use pricing_plans instead
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required): <br> 'one_time' example pricing_plans: `[{ "form": "one_time", "preferences": { "price": "199.9", "old_price": "200" } }]`<br> 'subscription' example pricing_plans: `[{ "form": "subscription", "preferences": { "first_interval": "1w", "first_amount": "20.0", "next_interval": "1m", "next_amount": "10.0" } }]`<br> 'splited' example (installment) pricing_plans: `[{ "form": "splited", "preferences": { "payments_count": "5", "first_interval": "1w", "first_amount": "20.0", "next_interval": "1m", "next_amount": "10.0", "spliting_type": "installment" } }]`<br> 'splited' example (limited_subscription) pricing_plans: `[{ "form": "splited", "preferences": { "payments_count": "5", "first_interval": "1w", "first_amount": "20.0", "next_interval": "1m", "next_amount": "10.0", "spliting_type": "limited_subscription" } }]`
form | String | Allowed values: `"one_time"`, `"subscription"`, `"splited"`
preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp; price | Decimal | price by current pricing plan (only for one_time pricing plan)
&nbsp;&nbsp; old_price | Decimal | old product price (only for one_time pricing plan)
&nbsp;&nbsp; payments_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp; first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp; first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp; next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp; next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp; spliting_type | String | spliting type (required for pricing plan form: splited); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payment payer will get seperate invoice <br> Allowed values: `["installment", "limited_subscription"]`
forward | Object | we can forward params with url_to_pay to prefill payment form, check this blog [post](http://blog.elopage.com/hilfebereich/was-bedeutet-die-url-parameter-weiterleitung/)
&nbsp;&nbsp; first_name | String | first name of payer
&nbsp;&nbsp; last_name | String | last name of payer
&nbsp;&nbsp; email | String | email of payer
&nbsp;&nbsp; coupon | String | coupon to automatically apply
&nbsp;&nbsp; campaign_id | String |
success_email | Object | Sucess email object contains data for custom email which is sent to payer on newly created payment for the product, if `success_email` is not specified then by default will be used standart template.<br> In custom email you are able to create email body with your own html code in `body_en/body_de` parameter, also you can specify the place of several variables in your html, which values are generated dynamically:<br> `first_name` - payer first name, `last_name` - payer last name, `product_name` - name of the product, `amount` - amount for the payment, `recurring_type` - type for recurring payments, `next_button` - button to continue with the payment process
&nbsp;&nbsp; subject_en | String | Subjet of the email for en locale version
&nbsp;&nbsp; subject_de | String | Subjet of the email for de locale version
&nbsp;&nbsp; body_en | String | Body of the email for en locale version
&nbsp;&nbsp; body_de | String | Body of the email for de locale version

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Number | Payment Link ID.
url_to_pay | String | Payment Link URL to which payer must be redirected

### Error 4xx

Name | Description
-----| -----------
NoAccessRight | Only authenticated sellers can access the data.
UserNotFound | The key of the user was not found.

## Get payment link info

Fetch payment link info by payment link ID

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/payment_links/{id}?key={youe API key}&secret={API secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/payment_links/{id}?key={youe API key}&secret={API secret}'))
```

### GET

`https://elopage.com/api/payment_links/:id`


### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage API secret which can be generated and found in the sellers dashboard under Settings > Integrations
key | String | Your personal Elopage API key, you can find or generate them inside your Elopage cabinet: Settings > Integrations
id | Number | Payment link ID

### Success 200

Field | Type | Description
----- | ---- | -----------
url_to_pay | String | Payment Link URL to which payer must be redirected
name | String | What is the payer paying for? Name the payment by entering a product name, plan name or similar. This name will be visible to your payer during the checkout process.
price | Decimal | The final price or amount you want to charge your payer incl. taxes, fees etc.
free | Boolean | Payment link for a free product
success_url | String | URL to which the payer or payer will be redirected after successfully completing payment. Attention: Payments with some payment methods require time to process. Due to this reason you must attach parameters to the redirection URLs to fetch updates of payment IDs. First is Payment ID `payment_id` which you can you fetch in order to receive the payment status `/api/payments/:id`.
cancel_url | String | URL to which payer is redirected to in case payment process was cancelled by payer.
error_url | String | URL to which payer is redirected in case of errors during payment process (e.g. Bad credit card info, rejected by pop etc.)
ping_url | String | elopage API sends POST request to this URL if payment for payment of product/name once it changes status to successful (e.g. bank wire updates).
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required)
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "splited"`
&nbsp;&nbsp;preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;payments_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;spliting_type | String | spliting type (required for pricing plan form: splited); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payments payer will get seperate invoice <br> Allowed values: ["installment", "limited_subscription"]

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated Sellers can access the data.
UserNotFound | The key of the User was not found.

# Payments

## Get payment info

> Example usage:

```shell
curl -X GET -H "Content-Type: application/json" "https://elopage.com/api/payments/{id}?key={youe API key}&secret={API secret}"
```

```ruby
require 'net/http'
require 'uri'

Net::HTTP.get(URI('https://elopage.com/api/payments/{id}?key={youe API key}&secret={API secret}'))
```

Fetch transfer info by ID

### GET

`https://elopage.com/api/payments/:id`

### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | Elopage API secret, you can find them inside your Elopage cabinet: Settings > Integrations
key | String | Your personal elopage API key which can be generated and found in the sellers dashboard under Settings > Integrations
id | Number | Payment ID is sent with the success_url redirection after payment

### Success 200

Field | Type | Description
----- | ---- | -----------
payer | Object | payer object
&nbsp;&nbsp;email | String | payer email
&nbsp;&nbsp;first_name | String | payer first name
&nbsp;&nbsp;last_name | String | payer last name
&nbsp;&nbsp;country | String | payer country
&nbsp;&nbsp;city | String | payer city
&nbsp;&nbsp;street | String | payer street
&nbsp;&nbsp;zip | String | payer zip
publisher | Object | publisher object
&nbsp;&nbsp;id | Integer | publisher id
&nbsp;&nbsp;email | String | publisher email
&nbsp;&nbsp;first_name | String | publisher first name
&nbsp;&nbsp;last_name | String | publisher last name
&nbsp;&nbsp;street | String | publisher street
&nbsp;&nbsp;zip | String | publisher zip
&nbsp;&nbsp;city | String | publisher city
&nbsp;&nbsp;country | String | publisher country
&nbsp;&nbsp;phone | String | publisher phone
&nbsp;&nbsp;company | String | publisher company
product | Object | product object
&nbsp;&nbsp;id | Integer | product id
&nbsp;&nbsp;slug | String | product slug
&nbsp;&nbsp;name | String | product name
&nbsp;&nbsp;type | String | product type
&nbsp;&nbsp;price | Decimal | product price
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required)
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "splited"`
&nbsp;&nbsp;preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;payments_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;spliting_type | String | spliting type (required for pricing plan form: installment); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payments payer will get seperate invoice <br> Allowed values: `["installment", "limited_subscription"]`
event | Object | event object
&nbsp;&nbsp;id | Integer | event id
&nbsp;&nbsp;name | String | event name
&nbsp;&nbsp;price | Decimal | event price
&nbsp;&nbsp;location_short | String | event location_short
&nbsp;&nbsp;location_long | String | event location_long
&nbsp;&nbsp;current_code | String | event current_code
&nbsp;&nbsp;code_prefix | String | event code_prefix
&nbsp;&nbsp;date | String | event date
tickets | Object | tickets object
&nbsp;&nbsp;count | Integer | tickets count
&nbsp;&nbsp;codes | Array | tickets codes
revenue | Decimal | revenue
amount | Decimal | amount
fee | Decimal | fee
bill_number | String | bill_number
campaign_id | String | campaign_id
coupon_code | String | code of coupon if applied
shift_in_days | Integer | shift_in_days
payment_method | String | card, bank_account, paypal, sofort, bank_wire or free
state | String | waiting, successful, success_av, canceled or error
created_date | String | created_date
success_date | String | success_date with time
success_date_short | String | success_date without time
tickets_count | Integer | tickets count - Warning! Deprecated. Please use tickets[count] instead.
event_id | Integer | tickets count - Warning! Deprecated. Please use event[id] instead.
invoice_link | String | link to download invoice, you should setup invoice generation inside cabinet.

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated Sellers can access the data.
UserNotFound | The key of the User was not found.

# Thanks for using the elopage API!
As we are currently in the beta version we are looking forward to your feedback and support requests. Please contact us via support@elopage.com. Thank you!
