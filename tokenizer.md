# Tokenizer API Documentation

NOTICE: This API is a work in progress and may change until finalized.

## URL

Production: `API_URL=https://tokenizer-api.chainparency.com`

## Authorization

To obtain your API token, navigate to [Tokeniezer](https://tokenizer.chainparency.com), select an organization, open Admin tab and generate api key.

Add the following header to all of your requests:

```
Authorization: Bearer $API_TOKEN
```

## Assets

### POST new Asset

```sh
curl "$API_URL/v1/orgs/$ORG_ID/assets" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"name":"Octocat", "description":"" }' \
```

Example Response

```json
{
  "asset": {
    "updatedAt": "2020-07-15T10:40:08.105771227-05:00",
    "createdAt": "2020-07-15T10:40:08.105771093-05:00",
    "id": "LkiEUtJNndhbbz8qfFQW",
    "orgID": "eboThjQLWfAd79dvQ05O",
    "name": "Name of asset",
    "description": "",
    "externalID": "",
    "createdBy": "tnbOrxNaHwGqh9gwDv0y",
    "category": "company",
    "image": "https://somewhere.com/abc.png",
    "tokenURI": "https://somewhere.com/token",
    "contractAddress": "0xABC",
    "tokenID": 123,
    "mintTx": "0xABC",
    "owner": "0xABC",
    "documents": [],
    "customFields": [],
    "contractAddressTokens": "0xABC",
  }
}
```

### GET Assets, owned by Organization

List assets for an organization.

```sh
curl "$API_URL/v1/orgs/$ORG_ID/assets" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
```

Example Response

```json
{
  "assets": [
    {
        "updatedAt": "2020-07-15T10:40:08.105771227-05:00",
        "createdAt": "2020-07-15T10:40:08.105771093-05:00",
        "id": "LkiEUtJNndhbbz8qfFQW",
        "orgID": "eboThjQLWfAd79dvQ05O",
        "name": "Name of asset",
        "description": "",
        "externalID": "",
        "createdBy": "tnbOrxNaHwGqh9gwDv0y",
        "category": "company",
        "image": "https://somewhere.com/abc.png",
        "tokenURI": "https://somewhere.com/token",
        "contractAddress": "0xABC",
        "tokenID": 123,
        "mintTx": "0xABC",
        "owner": "0xABC",
        "documents": [],
        "customFields": [],
        "contractAddressTokens": "0xABC",
    }
  ],
  "role": "admin"
}
```

### GET Assets, created by Organization

List assets that was created by organization.

```sh
curl "$API_URL/v1/orgs/$ORG_ID/assets" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
```

Example Response

```json
{
  "assets": [
    {
        "updatedAt": "2020-07-15T10:40:08.105771227-05:00",
        "createdAt": "2020-07-15T10:40:08.105771093-05:00",
        "id": "LkiEUtJNndhbbz8qfFQW",
        "orgID": "eboThjQLWfAd79dvQ05O",
        "name": "Name of asset",
        "description": "",
        "externalID": "",
        "createdBy": "tnbOrxNaHwGqh9gwDv0y",
        "category": "company",
        "image": "https://somewhere.com/abc.png",
        "tokenURI": "https://somewhere.com/token",
        "contractAddress": "0xABC",
        "tokenID": 123,
        "mintTx": "0xABC",
        "owner": "0xABC",
        "documents": [],
        "customFields": [],
        "contractAddressTokens": "0xABC",
    }
  ],
  "role": "admin"
}
```

### GET Asset by ID

```sh
curl "$API_URL/v1/assets/$ASSET_ID" \
  -H 'Content-Type: application/json' \  
  -H "Authorization: Bearer $API_TOKEN" \
```

Example Response

```json
{
  "asset": {
    "updatedAt": "2020-07-15T10:40:08.105771227-05:00",
    "createdAt": "2020-07-15T10:40:08.105771093-05:00",
    "id": "LkiEUtJNndhbbz8qfFQW",
    "orgID": "eboThjQLWfAd79dvQ05O",
    "name": "Name of asset",
    "description": "",
    "externalID": "",
    "createdBy": "tnbOrxNaHwGqh9gwDv0y",
    "category": "company",
    "image": "https://somewhere.com/abc.png",
    "tokenURI": "https://somewhere.com/token",
    "contractAddress": "0xABC",
    "tokenID": 123,
    "mintTx": "0xABC",
    "owner": "0xABC",
    "documents": [],
    "customFields": [],
    "contractAddressTokens": "0xABC",
  },
  "role": "admin"
}
```

To be documented

* Transfer Asset: POST "$API_URL/v1/assets/$ASSET_ID/transfer"
* Asset history: GET "$API_URL/v1/assets/$ASSET_ID/history

## Tokens / Shares
 
* Mint tokens for an asset: POST "$API_URL/v1/assets/$ASSET_ID/tokens"
* Transfer tokens: POST "$API_URL/v1/assets/$ASSET_ID/tokens/transfer"
* Get token holders: GET "$API_URL/v1/assets/$ASSET_ID/tokens"

## Organizations

* Create organization: POST "$API_URL/v1/orgs"
* Get organization information: POST "$API_URL/v1/orgs/$ORG_ID"
* Add member to an organization: POST "$API_URL/v1/orgs/$ORG_ID/members"
* Remove member from an organization: DELETE POST "$API_URL/v1/orgs/$ORG_ID/members/$MEMBER_ID"
* Get organization assets: GET "$API_URL/v1/orgs/$ORG_ID/assets"
* Get organization tokens/shares: GET "$API_URL/v1/orgs/$ORG_ID/tokens"
* Get organization history: GET "$API_URL/v1/orgs/$ORG_ID/history"
