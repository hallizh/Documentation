# Tracking API

This document describes the use of the Arta Tracking API.

## Process Flow and Calls

### Obtaining Carrier Name tokens
Before you can begin making tracking calls, you will need to know the tokenized name of the carrier the package was
shipped though. This will be used in the url of the actual tracking call below. To obtain a list of the supported
carriers and their tokens, you can simply make a `GET` request to the the `metadata/carriers` route.

#### Example Call
```curl
curl https://api.shiparta.com/metadata/carriers -H "Authorization: ARTAToken arta_1234567890"
```
#### Example Response
```JSON
[
    {
        "token": "fedex",
        "carrier_name": "FedEx"
    },
    {
        "token": "new_zealand_post",
        "carrier_name": "New Zealand Post",
        "notes": "also used for Pace and CourierPost"
    },
    {
        "token": "ups",
        "carrier_name": "UPS"
    },
    {
        "token": "usps",
        "carrier_name": "USPS"
    }
]
```

### Making a Shipment Track Call

Shipment tracking calls are a `GET` request. You will need to provide the name of carrier in the token format provided
by the metadata call above and the tracking number as part of the url.

#### Example call
```curl
curl https://api.shiparta.com/tracking/ups/1ZA9T4567890123450 -H "Authorization: ARTAToken arta_1234567890"
```

#### Example response
```json
[
    {
        "status": "TRANSIT",
        "status_details": "Your shipment has departed from the origin.",
        "status_date": "2019-04-24T19:42:30.716000Z",
        "city": "San Francisco",
        "state": "CA",
        "country": "US",
        "zip": "94103",
        "eta": "2019-04-28T21:47:30.695000Z",
        "substatus": null,
        "excepted": null
    },
    {
        "status": "UNKNOWN",
        "status_details": "The carrier has received the electronic shipment information.",
        "status_date": "2019-04-23T17:37:30.716000Z",
        "city": "San Francisco",
        "state": "CA",
        "country": "US",
        "zip": "94103",
        "eta": "2019-04-28T21:47:30.695000Z",
        "substatus": null,
        "excepted": null
    }
]
```

## JSON Data Schemas

[Carrier metadata schema](../json_schemas/metadata-carriers.schema.json)

[Tracking response schema](../json_schemas/tracking-response.schema.json)
