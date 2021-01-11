---
title: Windspeed API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

> BASE URL

```shell
export BASE_URL=https://api.windspeed.io
```

```python
BASE_URL = "https://api.windspeed.io"
```

```javascript
const baseURL = "https://api.windspeed.io";
```

The Windspeed API is organized around [REST](https://en.wikipedia.org/wiki/Representational_state_transfer). The API expects JSON request bodies while also returning responses of the same format. We also use standard HTTP [verbs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) and [response codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status).

For testing purposes, you may use the test environment - `https://staging.api.windspeed.io`.

# Authentication

> POST /v1/token/auth-token:

```python
import requests

data = {
    "username": "floralco",
    "password": "secret123"
}

response = requests.post("https://api.windspeed.io/v1/token/auth-token", json=data)
```

```shell
curl -X POST \
     -H "Content-Type: application/json" \
     -d '{"username": "floralco", "password": "secret123"}' \
     "https://api.windspeed.io/v1/token/auth-token"
```

```javascript
const axios = require('axios');

axios.post("https://api.windspeed.io/v1/token/auth-token", {
    username: "floralco",
    password: "secret123"
  }, {
    headers: {
      'Content-Type': 'application/json'
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(err);
  });
```

> RESPONSE

```json
{
  "token": "bcbd52db8c6431f6b398fc96444a381353193kjs",
  "account_id":	"bc9373b5-669f-4204-a02d-09341d8ab1df"
}
```

Requests made to Windspeed's API should be made with the API token attached in the request headers. The API token can be retrieved by making a request to the auth token endpoint.

After retrieving the API token, it can be included in subsequent requests with the header `Authorization: Token API_TOKEN`.

<aside class="notice">
  Take note to replace API_TOKEN with the token from the response.
</aside>

<aside class="warning">
  It is important not to expose your API token publicly.
</aside>

# Orders

## The Order object

> THE ORDER OBJECT:

```json
{
  "id": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
  "custom_id": "ORDER00001",
  "tracking_id": "MTOYNGYWYWNJMC0ZZDHLLTQ5M2UTOWE5OS1IZDNJNGQ1YZG1ZMIW",
  "company": 342,
  "company_name": "Summer Florals Co.",
  "delivery_address": "10 Bayfront Ave, Singapore 018956",
  "delivery_timeslot": {
    "lower": "2020-07-24T06:00:00Z",
    "upper": "2020-07-24T09:00:00Z",
    "bounds": "[)"
  },
  "sender_name": "John Doe",
  "sender_email": "john@example.org",
  "sender_contact": "91234567",
  "removed": null,
  "recipient_name": "Jane Smith",
  "recipient_contact": "97654321",
  "notes": "Make sure it is an anonymous delivery please.",
  "items": [
    {
      "id": 67184,
      "name": "Custom Bouquet",
      "quantity": 1,
      "metadata": {},
      "removed": null
    }
  ],
  "latitude": "1.285285460109807",
  "longitude": "103.86083121238575",
  "created": "2020-07-20T06:49:17.587611Z",
  "updated": "2020-07-20T06:49:17.587638Z",
  "status": "pending",
  "verbose_status": "Pending",
  "logs": [
    {
      "id": 185786,
      "order": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
      "order_custom_id": "ORDER00001",
      "order_delivery_address": "10 Bayfront Ave, Singapore 018956",
      "order_delivery_timeslot": {
        "lower": "2020-07-24T06:00:00Z",
        "upper": "2020-07-24T09:00:00Z",
        "bounds": "[)"
      },
      "message": "Order was created.",
      "failure_reason": null,
      "success_code": null,
      "success_description": null,
      "created": "2020-07-20T06:49:17.658913Z",
      "removed": null
    },
    {
      "id": 185787,
      "order": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
      "order_custom_id": "ORDER00001",
      "order_delivery_address": "10 Bayfront Ave, Singapore 018956",
      "order_delivery_timeslot": {
        "lower": "2020-07-24T06:00:00Z",
        "upper": "2020-07-24T09:00:00Z",
        "bounds": "[)"
      },
      "message": "Order is pending.",
      "failure_reason": null,
      "success_code": null,
      "success_description": null,
      "created": "2020-07-20T06:49:17.754210Z",
      "removed": null
    }
  ],
  "reschedule_requests": [],
  "signature": null
}
```

### Attributes
#### `id` string
Unique identifier for the Order.
#### `custom_id` string
Custom identifier for the Order. This is unique for each company.

#### `tracking_id` string
Unique tracking identifier for the Order. It can be used to [track](https://tracker.windspeed.io) the Order's current location once it is 'Out for Delivery'.

#### `company` number
Unique identifier of the company to which the Order belongs to.

#### `company_name` string
Name of the company.

#### `delivery_address` string
Address where the Order will be delivered to.

#### `delivery_timeslot` [DeliveryTimeslot](#the-deliverytimeslot-object)
The timeslot when the Order will be delivered. Timestamps are expressed in UTC.

#### `sender_name` string
Sender's name.

#### `sender_email` string
Sender's email address.

#### `sender_contact` string
Sender's contact number.

#### `removed` timestamp
Timestamp which this Order was removed. The value will be `null` by default, which means it is not removed.

#### `recipient_name` string
Recipient's name.

#### `recipient_contact` string
Recipient's contact number.

#### `notes` string
Additional notes.

#### `items` [OrderItem](#the-orderitem-object)[]
Array of `OrderItem` objects.

#### `latitude` string
Latitude of delivery address.

#### `longitude` string
Longitude of delivery address.

#### `created` string
Timestamp of Order creation. Timestamp is expressed in UTC.

#### `updated` string
Timestamp of last Order update. Timestamp is expressed in UTC.

#### `status` string
Current status of the Order.

#### `verbose_status` string
Human-readable format of the Order status.

## The DeliveryTimeslot object

> THE DELIVERYTIMESLOT OBJECT

```json
{
  "lower": "2020-07-24T06:00:00Z",
  "upper": "2020-07-24T09:00:00Z",
  "bounds": "[)"
}
```

### Attributes

#### `lower` required string
Lower bound of the delivery timeslot.

#### `upper` required string
Upper bound of the delivery timeslot.

#### `bounds` optional string
Whether the timeslot is inclusive or exclusive from either bound. Defaults to '[)' which includes the lower bound but not the upper bound.

## The OrderItem object

> THE ORDERITEM OBJECT

```json
{
  "id": 1,
  "name": "Bouquet",
  "quantity": 2,
  "metadata": {},
  "removed": null
}
```

### Attributes

#### `id` read-only number
Unique identifier of the OrderItem.

#### `name` required string
Name of the OrderItem.

#### `quantity` required number
Quantity of the OrderItem.

#### `metadata` optional dictionary
Custom key-value attributes.

#### `removed` string read-only
If the OrderItem is removed, then it will be the timestamp of the removal in UTC. Defaults to `null`.

## Create an Order

Creates an Order object.

> POST /v1/orders

```shell
curl https://api.windspeed.io/v1/orders \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Token YOUR-API-TOKEN" \
  -d '{
    "custom_id": "ORDER0001",
    "delivery_address": "10 Bayfront Ave, Singapore 018956",
    "delivery_timeslot": {
      "lower": "2020-07-24T06:00:00Z",
      "upper": "2020-07-24T09:00:00Z"
    },
    "sender_name": "John Doe",
    "sender_email": "john@example.org",
    "sender_contact": "91234567",
    "recipient_name": "Jane Smith",
    "recipient_contact": "97654321",
    "notes": "Make sure it is an anonymous delivery please.",
    "items": [
      {
        "name": "Custom Bouquet",
        "quantity": 1
      }
    ]
  }'
```

```python
import requests

data = {
    "custom_id": "ORDER0001",
    "delivery_address": "10 Bayfront Ave, Singapore 018956",
    "delivery_timeslot": {
      "lower": "2020-07-24T06:00:00Z",
      "upper": "2020-07-24T09:00:00Z"
    },
    "sender_name": "John Doe",
    "sender_email": "john@example.org",
    "sender_contact": "91234567",
    "recipient_name": "Jane Smith",
    "recipient_contact": "97654321",
    "notes": "Make sure it is an anonymous delivery please.",
    "items": [
      {
        "name": "Custom Bouquet",
        "quantity": 1
      }
    ]
}

response = requests.post("https://api.windspeed.io/v1/orders",
                         json=data,
                         headers={"Authorization": "Token YOUR-API-TOKEN"})
```

```javascript
const axios = require('axios');

const data = {
  "custom_id": "ORDER0001",
  "delivery_address": "10 Bayfront Ave, Singapore 018956",
  "delivery_timeslot": {
    "lower": "2020-07-24T06:00:00Z",
    "upper": "2020-07-24T09:00:00Z"
  },
  "sender_name": "John Doe",
  "sender_email": "john@example.org",
  "sender_contact": "91234567",
  "recipient_name": "Jane Smith",
  "recipient_contact": "97654321",
  "notes": "Make sure it is an anonymous delivery please.",
  "items": [
    {
      "name": "Custom Bouquet",
      "quantity": 1
    }
  ]
};

axios.post("https://api.windspeed.io/v1/orders", data, {
    headers: {
      "Content-Type": "application/json",
      "Authorization": "Token YOUR-API-TOKEN"
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(err);
  });
```

> RESPONSE

```json
{
  "id": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
  "custom_id": "ORDER00001",
  "tracking_id": "MTOYNGYWYWNJMC0ZZDHLLTQ5M2UTOWE5OS1IZDNJNGQ1YZG1ZMIW",
  "company": 342,
  "company_name": "Summer Florals Co.",
  "delivery_address": "10 Bayfront Ave, Singapore 018956",
  "delivery_timeslot": {
    "lower": "2020-07-24T06:00:00Z",
    "upper": "2020-07-24T09:00:00Z",
    "bounds": "[)"
  },
  "sender_name": "John Doe",
  "sender_email": "john@example.org",
  "sender_contact": "91234567",
  "removed": null,
  "recipient_name": "Jane Smith",
  "recipient_contact": "97654321",
  "notes": "Make sure it is an anonymous delivery please.",
  "items": [
    {
      "id": 67184,
      "name": "Custom Bouquet",
      "quantity": 1,
      "metadata": {},
      "removed": null
    }
  ],
  "latitude": "1.285285460109807",
  "longitude": "103.86083121238575",
  "created": "2020-07-20T06:49:17.587611Z",
  "updated": "2020-07-20T06:49:17.587638Z",
  "status": "pending",
  "verbose_status": "Pending",
  "logs": [
    {
      "id": 185786,
      "order": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
      "order_custom_id": "ORDER00001",
      "order_delivery_address": "10 Bayfront Ave, Singapore 018956",
      "order_delivery_timeslot": {
        "lower": "2020-07-24T06:00:00Z",
        "upper": "2020-07-24T09:00:00Z",
        "bounds": "[)"
      },
      "message": "Order was created.",
      "failure_reason": null,
      "success_code": null,
      "success_description": null,
      "created": "2020-07-20T06:49:17.658913Z",
      "removed": null
    },
    {
      "id": 185787,
      "order": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
      "order_custom_id": "ORDER00001",
      "order_delivery_address": "10 Bayfront Ave, Singapore 018956",
      "order_delivery_timeslot": {
        "lower": "2020-07-24T06:00:00Z",
        "upper": "2020-07-24T09:00:00Z",
        "bounds": "[)"
      },
      "message": "Order is pending.",
      "failure_reason": null,
      "success_code": null,
      "success_description": null,
      "created": "2020-07-20T06:49:17.754210Z",
      "removed": null
    }
  ],
  "reschedule_requests": [],
  "signature": null
}
```

> POSSIBLE ERRORS

```json
{
  "delivery_address": "Postal code does not exist: 123456"
}
```

```json
{
  "delivery_timeslot": "Delivery timeslot cannot be earlier than current time."
}
```

### Parameters

#### `custom_id` required string
Custom identifier for the Order. This is must be unique for your account.

#### `delivery_address` required string
Address where the Order will be delivered to. It must contain a valid 6-digital postal code.

#### `delivery_timeslot` required [DeliveryTimeslot](#the-deliverytimeslot-object)
The timeslot when the Order will be delivered. Timestamps can be expressed in any timezone, though the response will express the timestamps in UTC.

#### `sender_name` required string
Sender's name.

#### `sender_email` optional string
Sender's email address.

#### `sender_contact` required string
Sender's contact number. Accepted numbers include: valid 8-digit Singapore phone numbers, else it must contain a valid country code prefix (e.g. +61 for Australia, +1 for America etc).

#### `recipient_name` required string
Recipient's name.

#### `recipient_contact` required string
Recipient's contact number.

#### `notes` optional string
Additional notes.

#### `items` required OrderItem[]
Items that are part of the Order. An Order must contain at least one item.

### Returns
An [Order](#the-order-object) object.

### Possible Errors
Errors will usually be returned with the offending parameter as shown in the possible errors on the right. For issues not related to any parameter will be shown with the key `non_field_errors`.


## Update an Order

Updates an existing Order object.

> PATCH /v1/orders/:id

```shell
curl https://api.windspeed.io/v1/orders/24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb \
  -X PATCH \
  -H "Content-Type: application/json" \
  -H "Authorization: Token YOUR-API-TOKEN" \
  -d '{"sender_contact": "81234567"}'
```

```python
import requests

data = {"sender_contact": "81234567"}

response = requests.patch("https://api.windspeed.io/v1/orders/24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
                          json=data,
                          headers={"Authorization": "Token YOUR-API-TOKEN"})
```

```javascript
const axios = require('axios');

const data = {"sender_contact": "81234567"};

axios.patch("https://api.windspeed.io/v1/orders/24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb", data, {
    headers: {
      "Content-Type": "application/json",
      "Authorization": "Token YOUR-API-TOKEN"
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(err);
  });
```


> RESPONSE

```json
{
  "id": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
  "custom_id": "ORDER00001",
  "tracking_id": "MTOYNGYWYWNJMC0ZZDHLLTQ5M2UTOWE5OS1IZDNJNGQ1YZG1ZMIW",
  "company": 342,
  "company_name": "Summer Florals Co.",
  "delivery_address": "10 Bayfront Ave, Singapore 018956",
  "delivery_timeslot": {
    "lower": "2020-07-24T06:00:00Z",
    "upper": "2020-07-24T09:00:00Z",
    "bounds": "[)"
  },
  "sender_name": "John Doe",
  "sender_email": "john@example.org",
  "sender_contact": "91234567",
  "removed": null,
  "recipient_name": "Jane Smith",
  "recipient_contact": "97654321",
  "notes": "Make sure it is an anonymous delivery please.",
  "items": [
    {
      "id": 67184,
      "name": "Custom Bouquet",
      "quantity": 1,
      "metadata": {},
      "removed": null
    }
  ],
  "latitude": "1.285285460109807",
  "longitude": "103.86083121238575",
  "created": "2020-07-20T06:49:17.587611Z",
  "updated": "2020-07-20T06:49:17.587638Z",
  "status": "pending",
  "verbose_status": "Pending",
  "logs": [
    {
      "id": 185786,
      "order": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
      "order_custom_id": "ORDER00001",
      "order_delivery_address": "10 Bayfront Ave, Singapore 018956",
      "order_delivery_timeslot": {
        "lower": "2020-07-24T06:00:00Z",
        "upper": "2020-07-24T09:00:00Z",
        "bounds": "[)"
      },
      "message": "Order was created.",
      "failure_reason": null,
      "success_code": null,
      "success_description": null,
      "created": "2020-07-20T06:49:17.658913Z",
      "removed": null
    },
    {
      "id": 185787,
      "order": "24f0acc0-3d8e-493e-9a99-bd3c4d5c85fb",
      "order_custom_id": "ORDER00001",
      "order_delivery_address": "10 Bayfront Ave, Singapore 018956",
      "order_delivery_timeslot": {
        "lower": "2020-07-24T06:00:00Z",
        "upper": "2020-07-24T09:00:00Z",
        "bounds": "[)"
      },
      "message": "Order is pending.",
      "failure_reason": null,
      "success_code": null,
      "success_description": null,
      "created": "2020-07-20T06:49:17.754210Z",
      "removed": null
    }
  ],
  "reschedule_requests": [],
  "signature": null
}
```

### Parameters

#### `custom_id` optional string
Custom identifier for the Order. This is must be unique for your account.

#### `delivery_address` optional string
Address where the Order will be delivered to. It must contain a valid 6-digital postal code.

#### `delivery_timeslot` optional [DeliveryTimeslot](#the-deliverytimeslot-object)
The timeslot when the Order will be delivered. Timestamps can be expressed in any timezone, though the response will express the timestamps in UTC.

#### `sender_name` optional string
Sender's name.

#### `sender_email` optional string
Sender's email address.

#### `sender_contact` optional string
Sender's contact number. Accepted numbers include: valid 8-digit Singapore phone numbers, else it must contain a valid country code prefix (e.g. +61 for Australia, +1 for America etc).

#### `recipient_name` optional string
Recipient's name.

#### `recipient_contact` optional string
Recipient's contact number.

#### `notes` optional string
Additional notes.

#### `items` optional OrderItem[]
Items that are part of the Order. An Order must contain at least one item.

### Returns
An [Order](#the-order-object) object.
