# Tracking API

This document describes the use of the ARTA Tracking API.

## Process Flow and Calls

Before you can begin making tracking calls, you will need to know the tokenized name of the carrier the package was
shipped though. This tokenized name, along with the tracking number, will then be used as part of the url scheme when
making the tracking call. You will then receive back an array of the known tracking history of the shipment.


### Obtaining Carrier Name tokens
To obtain a list of the supported carriers and their tokens, make a `GET` request to the the
`metadata/carriers` route.

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
        "token": "ups",
        "carrier_name": "UPS"
    }
]
```
For more information regarding this response, please view the full [Carrier metadata schema](../../json_schemas/metadata-carriers.schema.json)

### Making a Shipment Track Call

Shipment tracking calls are a `GET` request. You will need to provide the name of carrier in the token format provided
by the metadata call above and the tracking number as part of the url. The url scheme for track call is
`/tracking/<tokenized_carrier_name>/<tracking_number>`

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
For more information regarding this response, please view the full
[tracking response schema](../../json_schemas/tracking-response.schema.json)
