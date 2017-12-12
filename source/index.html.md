---
title: API Reference

language_tabs:
  - shell: Curl
  - ruby: Ruby

search: true
---

# Introduction

Create sales pages and process payments through them easily with the elopage API. Use it to sell your products via our sales page using one time payments, subscriptions or installment payments.

# Get started now

Create sales pages, get paid, receive webhooks, create or delete pricing plans, find and search transaction ID / payment IDs and more is following. For questions please contact support@elopage.com.

# Sandbox
For using our test enviroment, please use `http://staging.elopage.com/`. API key and secret are given after the user approval.

## Test payments credentials
To do payments inside test enviroment, please use this credentials

### Credit Card

Parameter | Value
---- | -----------
CVV | Any 3 digits
Date | any date in the future, before 2030
Number | 5017670000005900 <br> 5017670000006700 <br> 5017670000007500 <br> 5017670000008300

### SOFORT

Parameter | Value
---- | -----------
Country of your bank | Germany
Sort code or BIC | 88888888
Account number | 234567
PIN | 12345
TAN | 12345

### Paypal

Parameter | Value
---- | -----------
Username | info-buyer@elopage.com
Password | 123elopage

### SEPA

For SEPA, it usually takes about 5 days to process payment. To do an instant successful or error payments, please use next creds:

Description | IBAN
---- | -----------
To do an instant successful transaction | DE89370400440532013000
To do an instant failed transaction | DE62370400440532013001

# SalesPages

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


The elopage API allows to initiative payments and complete payments on its optimized payment interfaces. In order to process payments you first need to create a payment link. With the payment link creation you receive the 'url_to_pay' parameter to which you must redirect the payer to complete payment. There after you can redirect the user back to your URL and forward information. After payer proceed with payment, or after the payment state changes from pending/waiting to success/error, you will get webhook to the 'webhook_url', which you sent to sales page referenced to this payment.

### POST

`https://elopage.com/api/sales_pages`

### Parameter

Field       | Type | Description
----------- | --- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Settings > Integrations
key | String | Your personal elopage API key which can be generated and found in the sellers dashboard under Settings > Integrations
name | String | What is the payer paying for? Name the payment by entering a product name, plan name or similar. This name will be visible to your payer during the checkout process.
success_url | String | URL to which the payer or payer will be redirected after successfully completing payment. Attention: Payments with some payment methods require time to process. Due to this reason you must attach parameters to the redirection URLs to fetch updates of payment IDs. First is Payment ID `payment_id` which you can you fetch in order to receive the payment status `/api/payments/:id`.
cancel_url | String | URL to which payer is redirected to in case payment process was cancelled by payer.
error_url | String | URL to which payer is redirected in case of errors during payment process (e.g. Bad credit card info, rejected by pop etc.)
webhook_url | String | elopage API sends POST request to this URL if payment for payment of product/name once it changes status to successful (e.g. bank wire updates). Also we will add transation id and payment id to POST request.
price | Decimal | Will be deprecated after 01.01.2018 use pricing_plans instead
recurring_prices | Object | Will be deprecated after 01.01.2018 including all its subparams - use pricing_plans instead
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required): <br> 'one_time' example pricing_plans: `[{ "form": "one_time", "preferences": { "price": "199.9", "old_price": "200" } }]`<br> 'subscription' example pricing_plans: `[{ "form": "subscription", "preferences": { "first_interval": "1w", "first_amount": "20.0", "next_interval": "1m", "next_amount": "10.0" } }]`<br> 'splited' example (installment) pricing_plans: `[{ "form": "splited", "preferences": { "p_count": "5", "first_interval": "1w", "first_amount": "20.0", "next_interval": "1m", "next_amount": "10.0", "spliting_type": "installment" } }]`<br> 'splited' example (limited_subscription) pricing_plans: `[{ "form": "splited", "preferences": { "p_count": "5", "first_interval": "1w", "first_amount": "20.0", "next_interval": "1m", "next_amount": "10.0", "spliting_type": "limited_subscription" } }]` Warning! maximum pricing plans allowed for one sales page is 5
&nbsp;&nbsp; form | String | Allowed values: `"one_time"`, `"subscription"`, `"splited"`
&nbsp;&nbsp; preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;&nbsp;&nbsp; price | Decimal | price by current pricing plan (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; old_price | Decimal | old product price (only for one_time pricing plan)
&nbsp;&nbsp;&nbsp;&nbsp; p_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp;&nbsp;&nbsp; first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp; first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp; next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp; next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp; spliting_type | String | spliting type (required for pricing plan form: splited); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payment payer will get seperate invoice <br> Allowed values: `["installment", "limited_subscription"]`
forward | Object | we can forward params with url_to_pay to prefill payment form, check this blog [post](http://blog.elopage.com/hilfebereich/was-bedeutet-die-url-parameter-weiterleitung/)
&nbsp;&nbsp; first_name | String | first name of payer
&nbsp;&nbsp; last_name | String | last name of payer
&nbsp;&nbsp; email | String | email of payer
&nbsp;&nbsp; coupon | String | coupon to automatically apply
&nbsp;&nbsp; campaign_id | String |
success_email | Object | Sucess email object contains data for custom email which is sent to payer on newly created payment for \n the product, if `success_email` is not specified then by default will be used standart template.<br> In custom email you are able to create email body with your own html code in `body_en/body_de` parameter, also you can specify the place of several variables in your html, which values are generated dynamically:<br> `first_name` - payer first name, `last_name` - payer last name, `product_name` - name of the product, `amount` - amount for the payment, `recurring_type` - type for recurring payments, `next_button` - button to continue with the payment process
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
            "form": "one_time",
            "preferences": {
                "price": "199.9",
                "old_price": "200.0"
            }
        }
    ]
}
```

### GET

`https://elopage.com/api/sales_pages/:id`


### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | elopage api_secret which can be generated and found in the sellers dashboard under Settings > Integrations
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
webhook_url | String | elopage API sends POST request to this URL if payment for payment of product/name once it changes status to successful (e.g. bank wire updates).
pricing_plans | Array | Array of objects which represents pricing plans of the product (at least one object required)
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "splited"`
&nbsp;&nbsp;preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: splited)
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

# Pricing plans
Pricing plan are being created with sales page, but you can still create, view or delete pricing plans.

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

Fetch pricing plan info by ID

### GET

`https://elopage.com/api/pricing_plans/:id`

### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | Elopage api_secret, you can find them inside your Elopage cabinet: Settings > Integrations
key | String | Your personal elopage API key which can be generated and found in the sellers dashboard under Settings > Integrations
id | Number | Pricing plan ID is sent with the get payment link info request


### Success 200

Field | Type | Description
----- | ---- | -----------
id    | Integer | Pricing plan id
form | String | Allowed values: `"one_time", "subscription", "splited"`
preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;spliting_type | String | spliting type (required for pricing plan form: splited); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payments payer will get seperate invoice <br> Allowed values: ["installment", "limited_subscription"]


### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated Sellers can access the data.
UserNotFound | The key of the User was not found.


## Create pricing plan
Create pricing plan by sending POST request with parameters to the server.
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
		"form": "splited",
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
    "form" => "splited",
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
sales_page_id | Integer | ID of sales page, for which you create this pricing plan
pricing_plan | Object | Pricing plan object
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "splited"`
&nbsp;&nbsp;preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;spliting_type | String | spliting type (required for pricing plan form: splited); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payments payer will get seperate invoice <br> Allowed values: ["installment", "limited_subscription"]

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Integer | Created pricing plan ID
form | String | Allowed values: `"one_time", "subscription", "splited"`
preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;spliting_type | String | spliting type (required for pricing plan form: splited); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payments payer will get seperate invoice <br> Allowed values: ["installment", "limited_subscription"]

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated Sellers can access the data.
UserNotFound | The key of the User was not found.


## Delete pricing plan
Delete pricing plan
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
sales_page_id | Integer | ID of sales page, for which you create this pricing plan
pricing_plan | Object | Pricing plan object
&nbsp;&nbsp;form | String | Allowed values: `"one_time", "subscription", "splited"`
&nbsp;&nbsp;preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp;&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;&nbsp;&nbsp;spliting_type | String | spliting type (required for pricing plan form: splited); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payments payer will get seperate invoice <br> Allowed values: ["installment", "limited_subscription"]

### Success 200

Field | Type | Description
----- | ---- | -----------
id | Integer | Created pricing plan ID
form | String | Allowed values: `"one_time", "subscription", "splited"`
preferences | Object | preferences object is differs for pricing forms
&nbsp;&nbsp;price | Decimal | price by current pricing plan
&nbsp;&nbsp;old_price | Decimal | old product price
&nbsp;&nbsp;p_count | Integer | total count of payments (required for pricing plan form: splited)
&nbsp;&nbsp;first_interval | String | first payment interval (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;first_amount | Decimal | first payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;next_interval | String | next payment interval startdate (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;next_amount | Decimal | next payment amount (required for pricing plan form: subscription/splited)
&nbsp;&nbsp;spliting_type | String | spliting type (required for pricing plan form: splited); notes: "installment" - one invoice for all payments, "limited_subscription" - for each payments payer will get seperate invoice <br> Allowed values: ["installment", "limited_subscription"]

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated Sellers can access the data.
UserNotFound | The key of the User was not found.


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
    "state": "successful",
    "created_date": "2017-12-11T15:47Z",
    "success_date": "2017-12-11T15:47Z",
    "success_date_short": "2017-12-11",
    "invoice_link": "https://elopage.com/common/invoices/2879?token=uVUai-84pyDhdz8K3nNd",
    "success_link": "https://elopage.com/s/elopage/product-name-6/payment_success?token=uVUai-84pyDhdz8K3nNd"
}
```
Fetch transfer info by ID

### GET

`https://elopage.com/api/payments/:id`

### Parameter

Field | Type | Description
----- | ---- | -----------
secret | String | Elopage api_secret, you can find them inside your Elopage cabinet: Settings > Integrations
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
&nbsp;&nbsp;company | String | payer company name
&nbsp;&nbsp;vat_id | String | payer VAT id
&nbsp;&nbsp;phone | String | payer phone
revenue | Decimal | revenue
amount | Decimal | amount
fee | Decimal | fee
recurring | Boolean | Returns true if the payment is recurring
recurring_form | String | Returns the recurring form of the transaction. This value is being taken from the pricing plan form(one_time, subscription, splited), which was chosen by payer on sales page
payment_method | String | card, bank_account, paypal, sofort, bank_wire
state | String | waiting, successful, success_av, canceled or error
created_date | String | created_date
success_date | String | success_date with time
success_date_short | String | success_date without time
invoice_link | String | link to download invoice, you should setup invoice generation inside cabinet.

### Error 4xx

Name | Description
---- | -----------
NoAccessRight | Only authenticated Sellers can access the data.
UserNotFound | The key of the User was not found.

# Thanks for using the elopage API!
As we are currently in the beta version we are looking forward to your feedback and support requests. Please contact us via support@elopage.com. Thank you!
