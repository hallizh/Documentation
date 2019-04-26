# Basic Setup

At this time, please engage with Arta directly regarding any setup, including obtaining access tokens.

## Authentication
Authentication takes place via a grant token. We will provide this token to you directly.
The token should be included in the `Authentication` header and is required for all requests.

#### Example Call
```curl
curl https://api.shiparta.com/metadata/carriers -H "Authorization: ARTAToken arta_1234567890"
```
In this example, you would replace `arta_1234567890` with the token we provided.
