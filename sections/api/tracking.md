# Tracking API

This document describes the use of the ARTA Tracking API.

## Process Flow and Calls

There are 2 ways to track a shipment, with the carrier name or without it. If you do not provide the carrier, the system will auto-detect the carrier from the tracking number. For some smaller carriers, this does not always work and it is therefore recommended that you use the carrier whenever possible. To use the carrier, you will need to know the tokenized name of the carrier the package was shipped with. This tokenized name, along with the tracking number, will then be used as part of the url scheme when making the tracking call. You will then receive back an array of the known tracking history of the shipment, ordered in time based descending order.

### Obtaining Carrier Name tokens
To obtain a list of the supported carriers and their tokens, make a `GET` request to the the
`metadata/carriers/` route.

#### Example Call
```curl
curl https://api.shiparta.com/metadata/carriers/ -H "Authorization: ARTAToken arta_1234567890"
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

Shipment tracking calls are a `GET` request. You can use the auto-detect feature if the carrier is unknown, however, this works best for larger carrier companies as smaller carriers often have much overlap in the formatting of their tracking numbers and therefore this is not always precise and may return the incorrect information. It is then recommended that for smaller carriers, you provide the name in the token format provided by the metadata call above as well as the tracking number in order to obtain the proper tracking information from the desired carrier. The output they provide is the same.

The url scheme for an auto-detect track call is
`/tracking/<tracking_number>/`

The url scheme for a carrier provided track call is:
`/tracking/<tokenized_carrier_name>/<tracking_number>/`

#### Example calls
**Without Carrier:**
```curl
curl https://api.shiparta.com/tracking/1ZA9T4567890123450/ -H "Authorization: ARTAToken arta_1234567890"
```

**With Carrier:**
```curl
curl https://api.shiparta.com/tracking/ups/1ZA9T4567890123450/ -H "Authorization: ARTAToken arta_1234567890"
```


#### Example response
```json
[
    {
        "carrier": "ups",
        "tracking_number": "1ZA9T4567890123450",
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
        "carrier": "ups",
        "tracking_number": "1ZA9T4567890123450",
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
