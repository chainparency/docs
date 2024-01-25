# GoTrace API Documentation

## URL

Production: `GOTRACE_API=https://gotrace-api.chainparency.com`

## Authorization

Get your API TOKEN from the profile page (menu button on the top right)
Add the following header to all of your requests:

```
Authorization: Bearer $API_TOKEN
```

## Organization

### POST new event

Adds new event to organization feed. Multipart/form-data request that has next keys:
- 'event' required parameter, that contains json of new event
- '0' ('1', '2' etc) optional parameters, that contains files to upload/attach to event. Should start from '0' and increment by 1.
  If any key will be skipped - nex file won't be added. For example: request contains '0', '1', '3', '4' file parts. Only '0' and '1' files will be uploaded.

#### File attaching:
- for every file, needed to be attached add form record (-- form 'key=value')
- form record 'key' should be number of file (starting from 0)
- form record 'value' should be full path to file, starting with '@' symbol
- also file name can be saved, by adding ';filename=initialFileName' to value

#### Event Types

##### `note`

Adds text/geolocation/files to organization feed.

##### `form`

Adds form to organization feed. Requires `form_id` and `form_data` fields.

```sh
curl -X POST "$GOTRACE_API/v1/orgs/$ORG_ID/events" \
  -H "Authorization: Bearer $API_TOKEN" \
  --form-string 'event={"type": "note", "geo_point": {"latitude": 41.8781, "longitude": -87.6298}, "form": "text", "form_id": "bx3GtRgE90FLi1CeZCif", "form_data": { "first_field": "some text" }}' \
  --form '0=@fullPathToFile;filename=pic.png' \
```

<details>
    <summary>Example Response</summary>

```json
{
  "event": {
    "updated_at": "2020-07-15T10:40:08.105771227-05:00",
    "created_at": "2020-07-15T10:40:08.105771093-05:00",
    "id": "LkiEUtJNndhbbz8qfFQW",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "entity_id": "eboThjQLWfAd79dvQ05O",
    "entity": "org",
    "deleted_at": null,
    "type": "form",
    "geo_point": {
      "latitude": 41.8781,
      "longitude": -87.6298
    },
    "note": "text",
    "media": [
      {
        "url": "http://some_url/1.png",
        "type": "image/jpeg",
        "filename": "pic.png"
      }
    ],
    "form_id": "bx3GtRgE90FLi1CeZCif",
    "form_data": {
      "first_field": "some text"
    },
  }
}
```
</details>

### GET Organization Events by ID

List events for a organization.

```sh
curl "$GOTRACE_API/v1/orgs/$ORG_ID/events" \
  -H "Authorization: Bearer $API_TOKEN" \
```

<details>
    <summary>Example Response</summary>

```json
{
  "events": [
    {
      "updated_at": "2020-07-15T10:40:08.105771227-05:00",
      "created_at": "2020-07-15T10:40:08.105771093-05:00",
      "id": "LkiEUtJNndhbbz8qfFQW",
      "org_id": "eboThjQLWfAd79dvQ05O",
      "entity_id": "eboThjQLWfAd79dvQ05O",
      "entity": "org",
      "deleted_at": null,
      "type": "note",
      "geo_point": {
        "latitude": 41.8781,
        "longitude": -87.6298
      },
      "note": "text",
      "media": [
        {
          "url": "http://some_url/1.png",
          "type": "image/jpeg",
          "filename": "pic.png"
        }
      ],
      "form_id": "bx3GtRgE90FLi1CeZCif",
      "form_data": {
        "first_field": "some text"
      },
    }
  ]
}
```
</details>

## Assets

### POST new Asset

```sh
curl "$GOTRACE_API/v1/orgs/$ORG_ID/assets" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"name":"Octocat","org_id":"ORG_ID","is_container":true,"status":"active"}' \
```

<details>
    <summary>Example Response</summary>

```json
{
  "asset": {
    "updated_at": "2020-07-15T10:40:08.105771227-05:00",
    "created_at": "2020-07-15T10:40:08.105771093-05:00",
    "id": "LkiEUtJNndhbbz8qfFQW",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "deleted_at": null,
    "name": "Named Asset",
    "type": "",
    "is_container": false,
    "status": "active"
  }
}
```
</details>

### GET Assets

List assets for an organization, sorted by recently created.

```sh
curl "$GOTRACE_API/v1/orgs/$ORG_ID/assets" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
```

<details>
    <summary>Example Response</summary>

```json
{
  "assets": [
    {
      "updated_at": "2020-07-15T15:40:08.105771Z",
      "created_at": "2020-07-15T15:40:08.105771Z",
      "id": "LkiEUtJNndhbbz8qfFQW",
      "org_id": "eboThjQLWfAd79dvQ05O",
      "deleted_at": null,
      "name": "Named Asset",
      "type": "",
      "is_container": false,
      "status": "active"
    }
  ]
}
```
</details>

### GET Asset by ID

```sh
curl "$GOTRACE_API/v1/assets/$ASSET_ID" \
  -H 'Content-Type: application/json' \  
  -H "Authorization: Bearer $API_TOKEN" \
```

<details>
    <summary>Example Response</summary>

```json
{
  "asset": {
    "updated_at": "2020-07-15T15:40:08.105771Z",
    "created_at": "2020-07-15T15:40:08.105771Z",
    "id": "LkiEUtJNndhbbz8qfFQW",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "deleted_at": null,
    "name": "Named Asset",
    "type": "",
    "is_container": false,
    "status": "active"
  }
}
```
</details>

## Loads

### POST new Load

#### Request


```jsonc
{
  "name": "Named Load",
  "paired_id": "", // optional: a unique ID generated by your system
  "asset": "LkiEUtJNndhbbz8qfFQW",
  "org_id": "eboThjQLWfAd79dvQ05O",
  "source_load_ids": null,
  "start_point": {"latitude":LATITUDE,"longitude":LONGITUDE},
}
```

If the load must have an asset, the only field that must be filled in is asset (by ID of needed asset). Initial values of fields are always empty. To set them use **POST Load Event** with type 'update'.

Curl example:

```sh
curl "$GOTRACE_API/v1/orgs/$ORG_ID/loads" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"name":"LOAD_NAME","org_id":"ORG_ID","parent_id":PARENT_ID,"asset":"ASSET_ID","source_load_ids":SOURCE_LOAD_IDS,"start_point":{"latitude":LATITUDE,"longitude":LONGITUDE}}'
```


<details>
    <summary>Example Response</summary>

```json
{
  "load": {
    "updated_at": "2020-07-15T10:44:27.009721748-05:00",
    "created_at": "2020-07-15T10:44:24.503183829-05:00",
    "id": "bx3GtRgE90FLi1CeZCif",
    "deleted_at": null,
    "name": "Named Load",
    "asset": "LkiEUtJNndhbbz8qfFQW",
    "paired_id": "",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "latest_org_id": "eboThjQLWfAd79dvQ05O",
    "parent_id": "",
    "status": "Stopped",
    "source_load_ids": null,
    "start_location_id": "5peH6IvKncoDZMVLEleK",
    "start_point": null,
    "latest_location_id": "5peH6IvKncoDZMVLEleK",
    "latest_point": null,
    "latest_event_type": "created",
    "trace_is_public": false,
    "is_container": false,
    "last_committed_event_at": "2020-07-15T10:44:24.968239965-05:00"
  }
}
```
</details>

### POST new Loads (batch)

#### Request

```jsonc
{
  "load": {
    "name": "Named Load",    
  },
  "num_loads": 2,
  "paired_ids": ["sample1","sample2"] // a list of unique IDs generated by your system
}
```

Curl example:

```sh
curl "$GOTRACE_API/v2/orgs/$ORG_ID/loads" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"load":{"name":"Named Load"},"num_loads":2, "paired_ids": ["sample1","sample2"]}}'
```

#### Query Parameters
You could use either of the following parameters (or both):

- `num_loads amount of loads to create`
- `paired_ids array of external paired ids`

<details>
    <summary>Example Response</summary>

```json
{
  "errors": null,
  "loads": [
      {
      "updated_at": "2020-07-15T10:44:27.009721748-05:00",
      "created_at": "2020-07-15T10:44:24.503183829-05:00",
      "id": "bx3GtRgE90FLi1CeZCif",
      "deleted_at": null,
      "name": "Named Load",
      "asset": "LkiEUtJNndhbbz8qfFQW",
      "paired_id": "sample1",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "eboThjQLWfAd79dvQ05O",
      "latest_org_id": "eboThjQLWfAd79dvQ05O",
      "parent_id": "",
      "status": "Stopped",
      "source_load_ids": null,
      "start_location_id": "5peH6IvKncoDZMVLEleK",
      "start_point": null,
      "latest_location_id": "5peH6IvKncoDZMVLEleK",
      "latest_point": null,
      "latest_event_type": "created",
      "trace_is_public": false,
      "is_container": false,
      "last_committed_event_at": "2020-07-15T10:44:24.968239965-05:00"
    },
    {
      "updated_at": "2020-07-15T10:44:27.009721748-05:00",
      "created_at": "2020-07-15T10:44:24.503183829-05:00",
      "id": "bx3GtRgE90FLi1CeZCif",
      "deleted_at": null,
      "name": "Named Load",
      "asset": "LkiEUtJNndhbbz8qfFQW",
      "paired_id": "sample2",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "eboThjQLWfAd79dvQ05O",
      "latest_org_id": "eboThjQLWfAd79dvQ05O",
      "parent_id": "",
      "status": "Stopped",
      "source_load_ids": null,
      "start_location_id": "5peH6IvKncoDZMVLEleK",
      "start_point": null,
      "latest_location_id": "5peH6IvKncoDZMVLEleK",
      "latest_point": null,
      "latest_event_type": "created",
      "trace_is_public": false,
      "is_container": false,
      "last_committed_event_at": "2020-07-15T10:44:24.968239965-05:00"
    }
  ]
}
```

</details>

### Publish Load Traces

Make the traces public for a set of load IDs.

```sh
curl "$GOTRACE_API/v2/loads/publish_trace" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"trace_is_public":true,"load_ids":["ABCD","EFGH"]}'
```

<details>
    <summary>Example Response</summary>

```json
{
  "errors": {
    "ABCD": "error message"
  }
}
```

</details>

### GET Loads

List loads for an organization, sorted by recently created.

```sh
curl "$GOTRACE_API/v1/orgs/$ORG_ID/loads" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
```

#### Query Parameters

- `latest_org boolean`
- `include_hidden boolean`
- `created_after datetime`
- `created_before datetime`
- `trace_is_public boolean`
- `asset_id string`
- `location_id string`
- `user_id string`

* `asset_id`, `location_id`, and `user_id` are exclusive with one another, though each may be included
multiple times on their own (up to 10).

```sh
curl "$GOTRACE_API/v1/orgs/0pFkVHqMtoQK5tB6EiC8/loads?latest_org=true&include_hidden=true&created_after=2020-07-09T12%3A59%3A00.000Z&created_before=2020-07-17T12%3A59%3A00.000Z&asset_id=a4yYiOqJYZqsAOlCrOnc&public_traces=true" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
```

<details>
    <summary>Example Response</summary>

```json
{
  "loads": [
    {
      "updated_at": "2020-07-15T15:44:27.009721Z",
      "created_at": "2020-07-15T15:44:24.503183Z",
      "id": "bx3GtRgE90FLi1CeZCif",
      "deleted_at": null,
      "name": "Named Load",
      "asset": "LkiEUtJNndhbbz8qfFQW",
      "paired_id": "",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "eboThjQLWfAd79dvQ05O",
      "latest_org_id": "eboThjQLWfAd79dvQ05O",
      "parent_id": "",
      "status": "Stopped",
      "source_load_ids": null,
      "start_location_id": "5peH6IvKncoDZMVLEleK",
      "start_point": null,
      "latest_location_id": "5peH6IvKncoDZMVLEleK",
      "latest_point": null,
      "latest_event_type": "created",
      "trace_is_public": false,
      "is_container": false,
      "last_committed_event_at": "2020-07-15T15:44:24.968239Z"
    }
  ]
}
```
</details>

### GET Load by ID

```sh
curl "$GOTRACE_API/v1/loads/$LOAD_ID" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
```

<details>
    <summary>Example Response</summary>

```json
{
  "load": {
    "updated_at": "2020-07-15T15:44:27.009721Z",
    "created_at": "2020-07-15T15:44:24.503183Z",
    "id": "bx3GtRgE90FLi1CeZCif",
    "deleted_at": null,
    "name": "Named Load",
    "asset": "LkiEUtJNndhbbz8qfFQW",
    "paired_id": "",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "latest_org_id": "eboThjQLWfAd79dvQ05O",
    "parent_id": "",
    "status": "Stopped",
    "source_load_ids": null,
    "start_location_id": "5peH6IvKncoDZMVLEleK",
    "start_point": null,
    "latest_location_id": "5peH6IvKncoDZMVLEleK",
    "latest_point": null,
    "latest_event_type": "created",
    "trace_is_public": false,
    "is_container": false,
    "last_committed_event_at": "2020-07-15T15:44:24.968239Z"
  }
}
```
</details>

To show load custom fields (if `load.asset` contains any asset ID) follow next steps:
- get `asset` entity. To get it use **GET Asset by ID** endpoint.
- build custom fields based on `asset.fields` field. `asset.fields` field is array of field objects. 
- to fill fields values use `load.data` field. `load.data` field is object with key/value structure, where key - is name of field (same as in `asset.fields`) and value - is value of field. If key/value is missing (`asset.field` has field, that has no record in `load.data`) field is empty.

<details>
    <summary>Example how to display load custom fields</summary>


We have load with `asset` (asset ID is `bx3GtRgE90FLi1CeZCif`) and with `data` with only 1 record (for field with name `field_1`).

**Load**
```json
{
    "updated_at": "2020-07-15T15:44:27.009721Z",
    "created_at": "2020-07-15T15:44:24.503183Z",
    "id": "bx3GtRgE90FLi1CeZCif",
    "deleted_at": null,
    "name": "Named Load",
    "asset": "LkiEUtJNndhbbz8qfFQW",
    "paired_id": "",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "latest_org_id": "eboThjQLWfAd79dvQ05O",
    "parent_id": "",
    "status": "Stopped",
    "source_load_ids": null,
    "start_location_id": "5peH6IvKncoDZMVLEleK",
    "start_point": null,
    "latest_location_id": "5peH6IvKncoDZMVLEleK",
    "latest_point": null,
    "latest_event_type": "created",
    "trace_is_public": false,
    "is_container": false,
    "last_committed_event_at": "2020-07-15T15:44:24.968239Z",
    "data": { "field_1": "any text value" }
}
```

We should get asset of this load. To get it we use `$GOTRACE_API/v1/assets/bx3GtRgE90FLi1CeZCif` endpoint.

**Asset**
```json
{
    "updated_at": "2020-07-15T15:40:08.105771Z",
    "created_at": "2020-07-15T15:40:08.105771Z",
    "id": "bx3GtRgE90FLi1CeZCif",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "deleted_at": null,
    "name": "Named Asset",
    "type": "",
    "is_container": false,
    "status": "active",
    "fields": [
      {"name": "field_1", "type": "text", "choices": null},
      {"name": "field_2", "type": "number", "choices": null},
    ]
}
```

So, we should show 2 custom fields:
- text field `field_1` with "any text value" value
- number field `field_2` with empty value
</details>

### POST Load Event


```sh
curl "$GOTRACE_API/v1/loads/$LOAD_ID/events" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"load_id":"2gcjqFd5pEVNwNhhH9u9","created_by":"p8Ov0RnOO9U1fVRJoa5R8JHcOLm1","org_id":"UEk2tZFKAF1LmkfzgbyA","type":"gps-start","geo_point":{"latitude":41.8781,"longitude":-87.6298}}'
```

<details>
<summary>Example Response</summary>

```json
{
  "event": {
    "updated_at": "2020-07-20T07:53:17.847251591-05:00",
    "created_at": "2020-07-20T07:53:16.481100112-05:00",
    "id": "8pAbKFm28vv1LZ3cCqRO",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "org_id": "UEk2tZFKAF1LmkfzgbyA",
    "load_id": "2gcjqFd5pEVNwNhhH9u9",
    "parent_id": "",
    "source_load_ids": null,
    "type": "gps-start",
    "geo_point": {
      "latitude": 41.8781,
      "longitude": -87.6298
    },
    "location_id": "5peH6IvKncoDZMVLEleK",
    "verified_geo_point": {
      "latitude": 41.8781,
      "longitude": -87.6298
    },
    "multihash": "QmUiPPL7AMWzKEwKNbCJn5ez4AFViwxEP9jRhFsUcq1jSv",
    "tx_hash": "0x4974668742b70b99308dd02717696aa9f0cb22b58c76ee1f796022f3b425955f"
  }
}
```
</details>


#### Event Types

##### `created`

Automatically generated when a new load is posted. 
May include the optional fields `source_load_ids` and `fields`.

##### `gps-start`

Initiates a tracking session. User must 'own' the load.

##### `gps-track`

Logs a geo point. Must follow `gps-start` or `gps-track`.

##### `gps-stop`

Ends a tracking session. Must follow `gps-track` or `gps-start`.

##### `accepted`

Transfers a load to a new user.

##### `added`

Adds a load to a 'parent' container load. Requires `parent_id` field.

##### `removed`

Removes a load from the current 'parent' container load.

##### `updated`

Updates load fields. Requires `fields` field, which is treated as a set of fields to PATCH.
Mutable fields include `name` (string) and `data` (object). Rather than setting `data` as a whole (which will overwrite),
one or more individual fields can be updated using dot notation (`data.*`).  

##### `note`

Event type, that is used to add any additional load info (files, text etc) to load history (load feed). 

##### `form`

Event type, that is used to add custom fields form to load history (load feed). Requires **form_id** and `form_data` fields. **form_id** should contain id of form, which is attached. `form_data` contains fields values, based on form.


<details>
    <summary>Example how to change all custom fields values</summary>

**Load**
```json
{
    "updated_at": "2020-07-15T15:44:27.009721Z",
    "created_at": "2020-07-15T15:44:24.503183Z",
    "id": "bx3GtRgE90FLi1CeZCif",
    "deleted_at": null,
    "name": "Named Load",
    "asset": "LkiEUtJNndhbbz8qfFQW",
    "paired_id": "",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "latest_org_id": "eboThjQLWfAd79dvQ05O",
    "parent_id": "",
    "status": "Stopped",
    "source_load_ids": null,
    "start_location_id": "5peH6IvKncoDZMVLEleK",
    "start_point": null,
    "latest_location_id": "5peH6IvKncoDZMVLEleK",
    "latest_point": null,
    "latest_event_type": "created",
    "trace_is_public": false,
    "is_container": false,
    "last_committed_event_at": "2020-07-15T15:44:24.968239Z",
    "data": { "field_1": "any text value" }
}
```

**Asset**
```json
{
    "updated_at": "2020-07-15T15:40:08.105771Z",
    "created_at": "2020-07-15T15:40:08.105771Z",
    "id": "bx3GtRgE90FLi1CeZCif",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "deleted_at": null,
    "name": "Named Asset",
    "type": "",
    "is_container": false,
    "status": "active",
    "fields": [
      {"name": "field_1", "type": "text", "choices": null},
      {"name": "field_2", "type": "number", "choices": null},
    ]
}
```

To change all fields values need to create such event:

**Event**
```json
{
    "org_id": "eboThjQLWfAd79dvQ05O",
    "load_id": "bx3GtRgE90FLi1CeZCif",
    "type": "updated",
    "fields": {
      "data": {
        "field_1": "new field 1 text value",
        "field_2": 5
      }
    }
  }
```

**Curl example**
```sh
curl "$GOTRACE_API/v1/loads/bx3GtRgE90FLi1CeZCif/events" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"org_id": "eboThjQLWfAd79dvQ05O","load_id": "bx3GtRgE90FLi1CeZCif","type": "updated","fields": {"data": {"field_1": "new field 1 text value","field_2": 5}}}'
```
</details>

<details>
    <summary>Example how to change one custom field value</summary>

**Load**
```json
{
    "updated_at": "2020-07-15T15:44:27.009721Z",
    "created_at": "2020-07-15T15:44:24.503183Z",
    "id": "bx3GtRgE90FLi1CeZCif",
    "deleted_at": null,
    "name": "Named Load",
    "asset": "LkiEUtJNndhbbz8qfFQW",
    "paired_id": "",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "latest_org_id": "eboThjQLWfAd79dvQ05O",
    "parent_id": "",
    "status": "Stopped",
    "source_load_ids": null,
    "start_location_id": "5peH6IvKncoDZMVLEleK",
    "start_point": null,
    "latest_location_id": "5peH6IvKncoDZMVLEleK",
    "latest_point": null,
    "latest_event_type": "created",
    "trace_is_public": false,
    "is_container": false,
    "last_committed_event_at": "2020-07-15T15:44:24.968239Z",
    "data": { "field_1": "any text value" }
}
```

**Asset**
```json
{
    "updated_at": "2020-07-15T15:40:08.105771Z",
    "created_at": "2020-07-15T15:40:08.105771Z",
    "id": "bx3GtRgE90FLi1CeZCif",
    "org_id": "eboThjQLWfAd79dvQ05O",
    "deleted_at": null,
    "name": "Named Asset",
    "type": "",
    "is_container": false,
    "status": "active",
    "fields": [
      {"name": "field_1", "type": "text", "choices": null},
      {"name": "field_2", "type": "number", "choices": null},
    ]
}
```

To change one field (for example `field_2`) value need to create such event:

**Event**
```json
{
    "org_id": "eboThjQLWfAd79dvQ05O",
    "load_id": "bx3GtRgE90FLi1CeZCif",
    "type": "updated",
    "fields": { "data.field_2": 5 }
  }
```

**Curl example**
```sh
curl "$GOTRACE_API/v1/loads/bx3GtRgE90FLi1CeZCif/events" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"org_id": "eboThjQLWfAd79dvQ05O","load_id": "bx3GtRgE90FLi1CeZCif","type": "updated","fields": { "data.field_2": 5 }}'
```
</details>

<details>
<summary>Example form_data</summary>

```json
{
  "field_1": "value_1",
  "field_2": "value_2"
}
```
</details>

### POST Load Event (with files)

This uses the same endpoint as above, but data must be sent in multipart/form-data format. 

One part must have the name `event` and should contain the same JSON object as defined above. 

File parts use incremented numbers as the name, starting with 0. So the first file part must be named `0`, the second is `1` and so on.

Files are limited to **50 MB**.

```sh
curl "$GOTRACE_API/v1/loads/$LOAD_ID/events" \
  -H "Authorization: Bearer $API_TOKEN" \
  -F 0=@photo.png
  -F 1=@document.pdf
  -F event='{"load_id":"2gcjqFd5pEVNwNhhH9u9","created_by":"p8Ov0RnOO9U1fVRJoa5R8JHcOLm1","org_id":"UEk2tZFKAF1LmkfzgbyA","type":"gps-start","geo_point":{"latitude":41.8781,"longitude":-87.6298}}'
```

<details>
<summary>Example Response</summary>

```json
{
  "event": {
    "updated_at": "2020-07-20T07:53:17.847251591-05:00",
    "created_at": "2020-07-20T07:53:16.481100112-05:00",
    "id": "8pAbKFm28vv1LZ3cCqRO",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "org_id": "UEk2tZFKAF1LmkfzgbyA",
    "load_id": "2gcjqFd5pEVNwNhhH9u9",
    "parent_id": "",
    "source_load_ids": null,
    "type": "gps-start",
    "geo_point": {
      "latitude": 41.8781,
      "longitude": -87.6298
    },
    "location_id": "5peH6IvKncoDZMVLEleK",
    "verified_geo_point": {
      "latitude": 41.8781,
      "longitude": -87.6298
    },
    "media": [
      {
        "url": "https://go-supply-chain.appspot.com.storage.googleapis.com/123432956643768.png",
        "type": "image/*",
        "filename": "photo.png",
      },
      {
        "url": "https://go-supply-chain.appspot.com.storage.googleapis.com/123846128364091.pdf",
        "type": "*.pdf",
        "filename": "document.pdf",
      },
    ]
    "multihash": "QmUiPPL7AMWzKEwKNbCJn5ez4AFViwxEP9jRhFsUcq1jSv",
    "tx_hash": "0x4974668742b70b99308dd02717696aa9f0cb22b58c76ee1f796022f3b425955f"
  }
}
```
</details>

### POST Load Events (batch)

```sh
curl "$GOTRACE_API/v2/loads/events" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \
  --data-binary '{"load_event":{"org_id":"4OL3XCCtl0","type":"gps-start","geo_point":{"latitude":54.7087691,"longitude":25.292111799999997}},"load_ids": ["9LZ9Eaz4eG","143CdNCWEGpw"],"paired_ids": ["sample1","sample2"]}'
```

#### Query Parameters
You could use either of the following parameters (or both):

- `load_ids array of load ids`
- `paired_ids array of external paired ids`

<details>
<summary>Example Response</summary>

```json
{
  {
  "errors": null,
  "load_events": [
    {
      "updated_at": "2021-06-07T17:13:26.426929757+03:00",
      "created_at": "2021-06-07T17:13:23.864663193+03:00",
      "id": "il3SaCGKA",
      "created_by": "qnFOaVV5MZVuMjO",
      "org_id": "4OL3XCCtl0",
      "load_id": "9LZ9Eaz4eG",
      "parent_id": "",
      "source_load_ids": null,
      "type": "gps-start",
      "geo_point": {
        "latitude": 54.7087691,
        "longitude": 25.292111799999997
      },
      "location_id": "r7UCXVrT",
      "verified_geo_point": {
        "latitude": 54.7087528,
        "longitude": 25.2892653
      },
      "fields": null,
      "shipengine_event": null,
      "multihash": "QmQF6LFkqWq1KhUGb",
      "tx_hash": "0x53df5385b77b63191711"
    },
    {
      "updated_at": "2021-06-07T17:13:26.447869082+03:00",
      "created_at": "2021-06-07T17:13:23.864522891+03:00",
      "id": "SzTnla1d8x",
      "created_by": "qnFOaVV5MZVuMjO",
      "org_id": "4OL3XCCtl0",
      "load_id": "143CdNCWE",
      "parent_id": "",
      "source_load_ids": null,
      "type": "gps-start",
      "geo_point": {
        "latitude": 54.7087691,
        "longitude": 25.292111799999997
      },
      "location_id": "r7UCXVr",
      "verified_geo_point": {
        "latitude": 54.7087528,
        "longitude": 25.2892653
      },
      "fields": null,
      "shipengine_event": null,
      "multihash": "QmTajvE46XSqCRW6qNMz",
      "tx_hash": "0xed78ef786c3887196eed22970b"
    },
     {
      "updated_at": "2021-06-07T17:13:30.62297253+03:00",
      "created_at": "2021-06-07T17:13:29.189617525+03:00",
      "id": "ye54NNUmD",
      "created_by": "qnFOaVV5MZVuMjO",
      "org_id": "4OL3XCCtl0",
      "load_id": "9LZ9Eaz4e",
      "parent_id": "",
      "source_load_ids": null,
      "type": "gps-start",
      "geo_point": {
        "latitude": 54.7087691,
        "longitude": 25.292111799999997
      },
      "location_id": "r7UCXVrT",
      "verified_geo_point": {
        "latitude": 54.7087528,
        "longitude": 25.2892653
      },
      "fields": null,
      "shipengine_event": null,
      "multihash": "QmVWrtFXWkkf1Z5krz",
      "tx_hash": "0x89190df9dc6fa9e39150f8e84"
    },
    {
      "updated_at": "2021-06-07T17:13:31.253634042+03:00",
      "created_at": "2021-06-07T17:13:29.803544631+03:00",
      "id": "cqARuOnTJ",
      "created_by": "qnFOaV",
      "org_id": "4OL3XCCtl0",
      "load_id": "143CdNCW",
      "parent_id": "",
      "source_load_ids": null,
      "type": "gps-start",
      "geo_point": {
        "latitude": 54.7087691,
        "longitude": 25.292111799999997
      },
      "location_id": "r7UCXVrT",
      "verified_geo_point": {
        "latitude": 54.7087528,
        "longitude": 25.2892653
      },
      "fields": null,
      "shipengine_event": null,
      "multihash": "QmeXNJQqkYLGrKSRNGt",
      "tx_hash": "0x5c23471b8b6748acadbbf9367f"
    }
  ]
}
```
</details>


### GET Load Events by ID

List events for a load.

```sh
curl "$GOTRACE_API/v1/loads/$LOAD_ID/events" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN" \

```

#### Query Parameters

- `type string` Filter results by event type. Repeat for multiple: `.../events?type=gps-start&type=gps-stop`. See [Event Types](#event-types)

<details>
    <summary>Example Response</summary>

```json
{
  "events": [
    {
      "updated_at": "2020-07-15T15:44:27.009622Z",
      "created_at": "2020-07-15T15:44:24.968239Z",
      "id": "K90u9oKoEQLylHScsi7L",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "eboThjQLWfAd79dvQ05O",
      "load_id": "bx3GtRgE90FLi1CeZCif",
      "parent_id": "",
      "source_load_ids": null,
      "type": "created",
      "geo_point": {
        "latitude": 41.8781,
        "longitude": -87.6298
      },
      "location_id": "5peH6IvKncoDZMVLEleK",
      "verified_geo_point": {
        "latitude": 41.8781,
        "longitude": -87.6298
      },
      "multihash": "QmW5EKHx8zV5TB77f5jdADxVRentUU6WFym4PVyFXJsiWs",
      "tx_hash": "0x75d15b3a953f678e459d02b9188f3934cddbc8f3f214ae906161bc9f781e83a4"
    }
  ]
}
```
</details>

### GET Loads by Paired ID

```sh
curl "$GOTRACE_API/v1/loads?paired_id=$PAIRED_ID" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```

Example Response: see [GET Loads](#GET-Loads)

### Hide Load

```sh
curl "$GOTRACE_API/v1/loads/$LOAD_ID/hide" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
  --data-binary '{"hidden":true}'
```

Example Response: see [GET Load by ID](#GET-Load-by-ID) 

### Set/Update Load PairedID

```sh
curl "$GOTRACE_API/v1/loads/$LOAD_ID/paired" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
  --data-binary '{"paired_id":$PAIRED_ID}'
```

Example Response: see [GET Load by ID](#GET-Load-by-ID) 

### GET Supply Graph

```sh
curl "$GOTRACE_API/v1/loads/$LOAD_ID/supply_graph" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```  

<details>
    <summary>Example Response</summary>

```json
{
  "load_shipment": {
    "source_shipments": [
      "e9pUL27a7W9GBMEZX3Bq",
      "RKpN7GeRFFRhg7aLBCKP",
      "oaZqDzrJlFafIALbjaAT",
      "4Lu02xQHzZaQc000W478"
    ],
    "events": [
      {
        "id": "UsLVKXke9UJruwcPQRFh",
        "created_at": "2020-06-25T19:11:34.247451Z",
        "load_id": "e8qSBfKxeoNCQoTBI2Lw",
        "type": "gps-stop",
        "geo_point": {
          "latitude": 40.7128,
          "longitude": -74.006
        },
        "location_id": "FgylD5ArYo9XmzZGK59F",
        "org_id": "UEk2tZFKAF1LmkfzgbyA",
        "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
        "multihash": "QmQif5mgTttHMN2Cs6e3PWYMdg63MBKeg4E19ySMV65AtY",
        "tx_hash": "0xe7b601b4d643926c1c133617342707b645e936d7e735f1ac854544a596a817c4"
      },
      {
        "id": "DNrMmhM1eTamqJodjIAB",
        "created_at": "2020-06-25T19:11:29.938887Z",
        "load_id": "e8qSBfKxeoNCQoTBI2Lw",
        "type": "gps-start",
        "geo_point": {
          "latitude": 41.7128,
          "longitude": -78.006
        },
        "org_id": "UEk2tZFKAF1LmkfzgbyA",
        "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
        "multihash": "QmdCeqcNncvdv3Akxo7rs53ATpCcCHTu5iUdt522rbwp5p",
        "tx_hash": "0xcc919a8d67a22be519645dde36d749fc49bd4d8349832cbde2881137c5c2a567"
      },
      {
        "id": "Ps22xE2z0y2XpdWyQHCO",
        "created_at": "2020-06-25T17:04:32.198797Z",
        "load_id": "e8qSBfKxeoNCQoTBI2Lw",
        "source_load_ids": [
          "e9pUL27a7W9GBMEZX3Bq",
          "RKpN7GeRFFRhg7aLBCKP",
          "oaZqDzrJlFafIALbjaAT",
          "4Lu02xQHzZaQc000W478"
        ],
        "type": "created",
        "org_id": "UEk2tZFKAF1LmkfzgbyA",
        "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
        "multihash": "QmSPgZe8q3Q2M34D4YBdutn3CWSYV5cq3LBUNusu1rKQpe",
        "tx_hash": "0xd683074b038466dbfd68e348877567fec7b71fc7c3be32f243dda6caee4a0ac7"
      }
    ]
  },
  "loads": {
    "1Hp0IeCX7AY7xjv7pGFW": {
      "updated_at": "2020-07-07T13:36:39.782447Z",
      "created_at": "2020-06-05T12:46:23.807044Z",
      "id": "1Hp0IeCX7AY7xjv7pGFW",
      "deleted_at": null,
      "name": "Kitts Beans",
      "asset": "EHYp2zermtSsgasvRwoC",
      "barcode_pair": "",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "latest_org_id": "UEk2tZFKAF1LmkfzgbyA",
      "parent_id": "",
      "status": "Stopped",
      "source_load_ids": null,
      "start_location_id": "SYppZcqTWk6p0vv9AdrP",
      "start_point": null,
      "latest_location_id": "",
      "latest_point": {
        "latitude": 41.7128,
        "longitude": -74.006
      },
      "latest_event_type": "gps-stop",
      "trace_is_public": true,
      "is_container": null,
      "last_committed_event_at": "2020-07-07T13:36:38.350654Z",
      "fields": null
    },
    "4Lu02xQHzZaQc000W478": {
      "updated_at": "2020-06-18T12:07:42.80593Z",
      "created_at": "2020-06-17T13:24:34.486423Z",
      "id": "4Lu02xQHzZaQc000W478",
      "deleted_at": null,
      "name": "Verfied Test",
      "asset": "gr5EWhThyITUwFlv1e2P",
      "barcode_pair": "",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "latest_org_id": "UEk2tZFKAF1LmkfzgbyA",
      "parent_id": "",
      "status": "Stopped",
      "source_load_ids": [
        "Unc9jqRda39uHo5xhll3"
      ],
      "start_location_id": "5peH6IvKncoDZMVLEleK",
      "start_point": null,
      "latest_location_id": "FgylD5ArYo9XmzZGK59F",
      "latest_point": null,
      "latest_event_type": "",
      "trace_is_public": true,
      "is_container": null,
      "last_committed_event_at": "2020-06-18T12:07:41.213569Z",
      "fields": null
    },
    "RKpN7GeRFFRhg7aLBCKP": {
      "updated_at": "2020-06-19T22:20:42.139053Z",
      "created_at": "2020-06-19T22:20:20.397377Z",
      "id": "RKpN7GeRFFRhg7aLBCKP",
      "deleted_at": null,
      "name": "Cycle A",
      "asset": "",
      "barcode_pair": "",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "latest_org_id": "UEk2tZFKAF1LmkfzgbyA",
      "parent_id": "e9pUL27a7W9GBMEZX3Bq",
      "status": "Stopped",
      "source_load_ids": null,
      "start_location_id": "FgylD5ArYo9XmzZGK59F",
      "start_point": null,
      "latest_location_id": "FgylD5ArYo9XmzZGK59F",
      "latest_point": null,
      "latest_event_type": "",
      "trace_is_public": false,
      "is_container": null,
      "last_committed_event_at": "2020-06-19T22:20:40.19023Z",
      "fields": null
    },
    "Unc9jqRda39uHo5xhll3": {
      "updated_at": "2020-06-22T15:28:51.034506Z",
      "created_at": "2020-06-05T12:48:36.619514Z",
      "id": "Unc9jqRda39uHo5xhll3",
      "deleted_at": null,
      "name": "Kitts Grounds",
      "asset": "Bon4dE1RHUSO8NM65JtV",
      "barcode_pair": "",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "latest_org_id": "UEk2tZFKAF1LmkfzgbyA",
      "parent_id": "",
      "status": "Stopped",
      "source_load_ids": [
        "1Hp0IeCX7AY7xjv7pGFW"
      ],
      "start_location_id": "5peH6IvKncoDZMVLEleK",
      "start_point": null,
      "latest_location_id": "5peH6IvKncoDZMVLEleK",
      "latest_point": null,
      "latest_event_type": "",
      "trace_is_public": true,
      "is_container": null,
      "last_committed_event_at": "2020-06-22T15:28:49.10568Z",
      "fields": null
    },
    "e9pUL27a7W9GBMEZX3Bq": {
      "updated_at": "2020-07-07T14:49:50.717222Z",
      "created_at": "2020-06-19T22:20:34.891739Z",
      "id": "e9pUL27a7W9GBMEZX3Bq",
      "deleted_at": null,
      "name": "Cycle B",
      "asset": "",
      "barcode_pair": "",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "latest_org_id": "UEk2tZFKAF1LmkfzgbyA",
      "parent_id": "",
      "status": "Stopped",
      "source_load_ids": null,
      "start_location_id": "FgylD5ArYo9XmzZGK59F",
      "start_point": null,
      "latest_location_id": "FgylD5ArYo9XmzZGK59F",
      "latest_point": null,
      "latest_event_type": "gps-stop",
      "trace_is_public": true,
      "is_container": null,
      "last_committed_event_at": "2020-07-07T14:49:49.556363Z",
      "fields": null
    },
    "oaZqDzrJlFafIALbjaAT": {
      "updated_at": "2020-06-19T22:14:33.496844Z",
      "created_at": "2020-06-19T22:13:53.827475Z",
      "id": "oaZqDzrJlFafIALbjaAT",
      "deleted_at": null,
      "name": "Test Parent",
      "asset": "",
      "barcode_pair": "",
      "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "latest_user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "latest_org_id": "UEk2tZFKAF1LmkfzgbyA",
      "parent_id": "Unc9jqRda39uHo5xhll3",
      "status": "Stopped",
      "source_load_ids": null,
      "start_location_id": "FgylD5ArYo9XmzZGK59F",
      "start_point": null,
      "latest_location_id": "FgylD5ArYo9XmzZGK59F",
      "latest_point": null,
      "latest_event_type": "",
      "trace_is_public": false,
      "is_container": null,
      "last_committed_event_at": "2020-06-19T22:14:31.656746Z",
      "fields": null
    }
  },
  "locations": {
    "3ldaGcFnfXNGbJHhe6eC": {
      "updated_at": "2020-04-15T23:42:22.354617Z",
      "created_at": "2020-04-15T23:42:22.354617Z",
      "id": "3ldaGcFnfXNGbJHhe6eC",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "deleted_at": null,
      "name": "London",
      "status": "",
      "address": "123 Fake St.",
      "lat": 51.5074,
      "lon": -0.1278,
      "geo": "gcpvj0duq533",
      "radius": 1,
      "fuzzy": null
    },
    "5VwAGZ2qpFIFXLQRg09n": {
      "updated_at": "2020-05-28T18:17:46.277449Z",
      "created_at": "2020-05-28T18:17:46.277449Z",
      "id": "5VwAGZ2qpFIFXLQRg09n",
      "org_id": "0pFkVHqMtoQK5tB6EiC8",
      "deleted_at": null,
      "name": "Los Angeles",
      "status": "",
      "address": "",
      "lat": 34.0522,
      "lon": -118.2437,
      "geo": "9q5ctr186n4v",
      "radius": 1,
      "fuzzy": null
    },
    "5peH6IvKncoDZMVLEleK": {
      "updated_at": "2020-04-15T17:39:50.38664Z",
      "created_at": "2020-04-15T17:39:50.38664Z",
      "id": "5peH6IvKncoDZMVLEleK",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "deleted_at": null,
      "name": "Gotham",
      "status": "",
      "address": "13 Wayne Pl.",
      "lat": 41.8781,
      "lon": -87.6298,
      "geo": "dp3wjztvtetj",
      "radius": 10,
      "fuzzy": null
    },
    "BK9wsmRuKXUKTsAZ1LRc": {
      "updated_at": "2020-05-28T18:16:50.189721Z",
      "created_at": "2020-05-28T18:16:50.189721Z",
      "id": "BK9wsmRuKXUKTsAZ1LRc",
      "org_id": "0pFkVHqMtoQK5tB6EiC8",
      "deleted_at": null,
      "name": "Dallas",
      "status": "",
      "address": "",
      "lat": 32.7767,
      "lon": -96.797,
      "geo": "9vg4mqfd47tr",
      "radius": 1,
      "fuzzy": null
    },
    "FgylD5ArYo9XmzZGK59F": {
      "updated_at": "2020-05-28T18:17:09.949798Z",
      "created_at": "2020-05-28T18:17:09.949798Z",
      "id": "FgylD5ArYo9XmzZGK59F",
      "org_id": "0pFkVHqMtoQK5tB6EiC8",
      "deleted_at": null,
      "name": "New York",
      "status": "",
      "address": "",
      "lat": 40.7128,
      "lon": -74.006,
      "geo": "dr5regw3ppyz",
      "radius": 5,
      "fuzzy": null
    },
    "SYppZcqTWk6p0vv9AdrP": {
      "updated_at": "2020-04-16T13:51:52.164332Z",
      "created_at": "2020-04-16T13:51:52.164332Z",
      "id": "SYppZcqTWk6p0vv9AdrP",
      "org_id": "UEk2tZFKAF1LmkfzgbyA",
      "deleted_at": null,
      "name": "GoChain",
      "status": "",
      "address": "123 Fake St.",
      "lat": 17.3578,
      "lon": -62.783,
      "geo": "de56erfud06n",
      "radius": 30,
      "fuzzy": null
    },
    "r7UCXVrTinDWEB6Sr4U0": {
      "updated_at": "2020-05-28T17:18:14.992948Z",
      "created_at": "2020-05-28T17:18:14.992948Z",
      "id": "r7UCXVrTinDWEB6Sr4U0",
      "org_id": "0k2biJjpHB5heGpEXEzr",
      "deleted_at": null,
      "name": "Home office",
      "status": "",
      "address": "Ulonu 3",
      "lat": 54.7087528,
      "lon": 25.2892653,
      "geo": "u99zprrjdddy",
      "radius": 1,
      "fuzzy": null
    }
  },
  "orgs": {
    "OEV1RKWQtZWxaiR7tLwZ": {
      "updated_at": "2020-06-16T20:32:52.761081Z",
      "created_at": "2020-06-16T20:32:52.761081Z",
      "id": "OEV1RKWQtZWxaiR7tLwZ",
      "deleted_at": null,
      "name": "Another org from FF",
      "logo": "",
      "status": ""
    },
    "UEk2tZFKAF1LmkfzgbyA": {
      "updated_at": "2020-04-10T13:14:56.228951Z",
      "created_at": "2020-04-10T13:08:03.039567Z",
      "id": "UEk2tZFKAF1LmkfzgbyA",
      "deleted_at": null,
      "name": "Coffee Corp.",
      "logo": "https://cdn.pixabay.com/photo/2016/12/27/13/10/logo-1933884_960_720.png",
      "status": ""
    }
  },
  "output_shipments": {},
  "supply_graph": {
    "shipments": {
      "1Hp0IeCX7AY7xjv7pGFW": {
        "source_shipments": null,
        "events": [
          {
            "id": "bQoS4PfAArslU6wtSZYq",
            "created_at": "2020-07-07T13:36:38.350654Z",
            "load_id": "1Hp0IeCX7AY7xjv7pGFW",
            "type": "gps-stop",
            "geo_point": {
              "latitude": 41.7128,
              "longitude": -74.006
            },
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmUqyhSTk7ERMKmDEvV5F2iutYnainMchCxWhvonniVGf4",
            "tx_hash": "0xf6465ef06e25764451d2fb7326510c71fc645a2cd78ab2d3ef9f735e550a47a0"
          },
          {
            "id": "GOA9zSrXmCH53dTly2S7",
            "created_at": "2020-07-07T13:36:04.690423Z",
            "load_id": "1Hp0IeCX7AY7xjv7pGFW",
            "type": "gps-start",
            "geo_point": {
              "latitude": 41.7128,
              "longitude": -74.006
            },
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmQuxsWgC1tWu8morCZqqpsJ1SYcD4WuENtq5UTn3shnd4",
            "tx_hash": "0xbc8795a389bee814bdb6a18d5019fd1a14a6c4bdef1876131cca25cf2c22465c"
          },
          {
            "id": "",
            "created_at": "2020-06-05T12:46:28.005345Z",
            "load_id": "1Hp0IeCX7AY7xjv7pGFW",
            "type": "accepted",
            "location_id": "SYppZcqTWk6p0vv9AdrP",
            "org_id": "",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1"
          }
        ]
      },
      "4Lu02xQHzZaQc000W478": {
        "source_shipments": [
          "Unc9jqRda39uHo5xhll3"
        ],
        "events": [
          {
            "id": "FwNcKY1UDRTDnJbUBw9T",
            "created_at": "2020-06-18T12:07:41.213569Z",
            "load_id": "4Lu02xQHzZaQc000W478",
            "type": "gps-stop",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmNkAeMk7YSK6ez22ftJMeuJYHEGhfuFTJBt57VYZnkZ52",
            "tx_hash": "0xa81eaf6fa7200e5e18c47f233d157bae43e82e6a357449d2c97f4695416eb66b"
          },
          {
            "id": "09piPZE6IhLC5jPDrUfB",
            "created_at": "2020-06-18T12:07:01.75471Z",
            "load_id": "4Lu02xQHzZaQc000W478",
            "type": "gps-start",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmWmkhTG1k4LGksDkUsne8VEwprpo48zHAG6ex6p4gxHvw",
            "tx_hash": "0x11f38290d8bae96c849054f07e79eb8a964993812ca3990f198b9f1b8ecf2414"
          },
          {
            "id": "CHnx2EbshHrIIjmtE5V5",
            "created_at": "2020-06-17T13:24:42.132428Z",
            "load_id": "4Lu02xQHzZaQc000W478",
            "type": "accepted",
            "geo_point": {
              "latitude": 41.8781,
              "longitude": -87.6298
            },
            "location_id": "5peH6IvKncoDZMVLEleK",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmTVFyXKwC2nH5mfc8jyAeBmxQuhgy2RM51pNLkprwSTkt",
            "tx_hash": "0xffc001a482c3736393cc171adb1ffeb862688c9c02a1d950e4ffe9c2560de575"
          }
        ]
      },
      "RKpN7GeRFFRhg7aLBCKP": {
        "source_shipments": null,
        "events": [
          {
            "id": "1MR5zC2SxJvuaCrBq5bu",
            "created_at": "2020-07-07T14:49:49.556363Z",
            "load_id": "e9pUL27a7W9GBMEZX3Bq",
            "type": "gps-stop",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmRP2urUjvoiXZ68PCLC9h5iUnCRhTviYG1Cg2ZHyQXFTT",
            "tx_hash": "0xd24b5b650002e3701cc39aba43a2b410b4c01d21ad73bbccf333a4fa93e55475"
          },
          {
            "id": "NmxC5HIviFuO5UplAgpy",
            "created_at": "2020-07-07T14:48:54.080727Z",
            "load_id": "e9pUL27a7W9GBMEZX3Bq",
            "type": "gps-start",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmWduqroeyZKauikpgrYAFp2gty6929pdvxzr2H7Ksi56F",
            "tx_hash": "0x2493848375372cf4a0c46309b981306415fc09e942616669ba7fd8340bc68e8e"
          },
          {
            "id": "9xAaWKbwTLcCqyEqhuvI",
            "created_at": "2020-06-19T22:20:40.19023Z",
            "load_id": "RKpN7GeRFFRhg7aLBCKP",
            "parent_id": "e9pUL27a7W9GBMEZX3Bq",
            "type": "added",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmWVrynt2yYiAJiG1NW8ci5R4dBXvdk8otFBUpeCCiMHRW",
            "tx_hash": "0xbb1a3bf08d75322753987a4eacb354db64cb041fb9b28932b076ad1dbe264ef2"
          },
          {
            "id": "HOkl6jXgsIsIlL3nqeow",
            "created_at": "2020-06-19T22:20:21.158366Z",
            "load_id": "RKpN7GeRFFRhg7aLBCKP",
            "type": "created",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmdYrstMX6FYqNJGNewmA7AXVxKKe44PsKuFpsmDafT8ob",
            "tx_hash": "0x1ade90ce96bc5d92c2988159dd9f53eccb670e592939eed4e4bc422624f301b5"
          }
        ]
      },
      "Unc9jqRda39uHo5xhll3": {
        "source_shipments": [
          "1Hp0IeCX7AY7xjv7pGFW"
        ],
        "events": [
          {
            "id": "VyXeCv1TOIwY8AWWqeRh",
            "created_at": "2020-06-22T15:28:49.10568Z",
            "load_id": "Unc9jqRda39uHo5xhll3",
            "type": "gps-stop",
            "geo_point": {
              "latitude": 41.8781,
              "longitude": -87.6298
            },
            "location_id": "5peH6IvKncoDZMVLEleK",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmVK1HKP1RxVUgsxqcWfoR3seCUBBLZw1dHyuCKnpTR8wn",
            "tx_hash": "0xaab537ddfc9a2fc5b7e1fabf42a200d978a85393ab2dddfc983d9c2826d1b2b8"
          },
          {
            "id": "AJHn9igzvnJ1K6FpCkw7",
            "created_at": "2020-06-22T15:28:19.255516Z",
            "load_id": "Unc9jqRda39uHo5xhll3",
            "type": "gps-start",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmfZkaNAF65u9YYFZqS4RDirbckRXZpxnNCodtK4w5nXCV",
            "tx_hash": "0x7ad08c6f4caec188ea8d37d544e8b12cf2ad36bf7d04965fddb2a80538fae730"
          },
          {
            "id": "",
            "created_at": "2020-06-05T12:49:10.836242Z",
            "load_id": "Unc9jqRda39uHo5xhll3",
            "type": "gps-stop",
            "location_id": "5VwAGZ2qpFIFXLQRg09n",
            "org_id": "",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1"
          },
          {
            "id": "",
            "created_at": "2020-06-05T12:48:58.929884Z",
            "load_id": "Unc9jqRda39uHo5xhll3",
            "type": "gps-start",
            "location_id": "5peH6IvKncoDZMVLEleK",
            "org_id": "",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1"
          },
          {
            "id": "",
            "created_at": "2020-06-05T12:48:49.071672Z",
            "load_id": "Unc9jqRda39uHo5xhll3",
            "type": "accepted",
            "location_id": "5peH6IvKncoDZMVLEleK",
            "org_id": "",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1"
          }
        ]
      },
      "e9pUL27a7W9GBMEZX3Bq": {
        "source_shipments": null,
        "events": [
          {
            "id": "1MR5zC2SxJvuaCrBq5bu",
            "created_at": "2020-07-07T14:49:49.556363Z",
            "load_id": "e9pUL27a7W9GBMEZX3Bq",
            "type": "gps-stop",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmRP2urUjvoiXZ68PCLC9h5iUnCRhTviYG1Cg2ZHyQXFTT",
            "tx_hash": "0xd24b5b650002e3701cc39aba43a2b410b4c01d21ad73bbccf333a4fa93e55475"
          },
          {
            "id": "NmxC5HIviFuO5UplAgpy",
            "created_at": "2020-07-07T14:48:54.080727Z",
            "load_id": "e9pUL27a7W9GBMEZX3Bq",
            "type": "gps-start",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmWduqroeyZKauikpgrYAFp2gty6929pdvxzr2H7Ksi56F",
            "tx_hash": "0x2493848375372cf4a0c46309b981306415fc09e942616669ba7fd8340bc68e8e"
          },
          {
            "id": "RZtpyDQu0hHIy1wVJxih",
            "created_at": "2020-06-23T22:03:21.56064Z",
            "load_id": "e9pUL27a7W9GBMEZX3Bq",
            "type": "gps-stop",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmY3Gc9DNJhBLa4XpMzEJEHDp7rX7r8uQHQjNEBjLjLi8a",
            "tx_hash": "0xcce9d9a6cf81d2b0912536eb57986ab18cc96123b939d601190d340c0cf61b27"
          },
          {
            "id": "IzbNCZCZCDCEDHnqbB7g",
            "created_at": "2020-06-23T22:02:33.254392Z",
            "load_id": "e9pUL27a7W9GBMEZX3Bq",
            "type": "gps-start",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmSUZU7PsCzLY5T4GaWXmrMui7WdcBe6FJkG7Q6LkmsSLJ",
            "tx_hash": "0xa74a71d12cdcf25a46bdc0963dd449a0953d5f75130ed19bdd0ef19126cd76a0"
          },
          {
            "id": "BTLAgnYDzgQpS96rprJp",
            "created_at": "2020-06-19T22:20:35.004791Z",
            "load_id": "e9pUL27a7W9GBMEZX3Bq",
            "type": "created",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmbMEFf7uPp9ZSBWGuHgbuPWWeWpKoe4a45NLeBLfPd3BJ",
            "tx_hash": "0x56baa5147f65bcf801d445186c082e3540444d31ef6a2bd8857d90abbbb4f0e5"
          }
        ]
      },
      "oaZqDzrJlFafIALbjaAT": {
        "source_shipments": null,
        "events": [
          {
            "id": "VyXeCv1TOIwY8AWWqeRh",
            "created_at": "2020-06-22T15:28:49.10568Z",
            "load_id": "Unc9jqRda39uHo5xhll3",
            "type": "gps-stop",
            "geo_point": {
              "latitude": 41.8781,
              "longitude": -87.6298
            },
            "location_id": "5peH6IvKncoDZMVLEleK",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmVK1HKP1RxVUgsxqcWfoR3seCUBBLZw1dHyuCKnpTR8wn",
            "tx_hash": "0xaab537ddfc9a2fc5b7e1fabf42a200d978a85393ab2dddfc983d9c2826d1b2b8"
          },
          {
            "id": "AJHn9igzvnJ1K6FpCkw7",
            "created_at": "2020-06-22T15:28:19.255516Z",
            "load_id": "Unc9jqRda39uHo5xhll3",
            "type": "gps-start",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmfZkaNAF65u9YYFZqS4RDirbckRXZpxnNCodtK4w5nXCV",
            "tx_hash": "0x7ad08c6f4caec188ea8d37d544e8b12cf2ad36bf7d04965fddb2a80538fae730"
          },
          {
            "id": "BUre4Pnqnc10z5X24zY6",
            "created_at": "2020-06-19T22:14:31.656746Z",
            "load_id": "oaZqDzrJlFafIALbjaAT",
            "parent_id": "Unc9jqRda39uHo5xhll3",
            "type": "added",
            "geo_point": {
              "latitude": 40.7128,
              "longitude": -74.006
            },
            "location_id": "FgylD5ArYo9XmzZGK59F",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmbLUiDGF79CsFtp1aNpHbQF5JsV7JVa9qW7hUfxUzYJ9y",
            "tx_hash": "0xc5a13db1f6a8baf18cfea2a6b30dae5188b7d5fef8ef19c05d601b671d483c12"
          },
          {
            "id": "L2XLq1DNeKrprK9oBoGR",
            "created_at": "2020-06-19T22:13:53.936258Z",
            "load_id": "oaZqDzrJlFafIALbjaAT",
            "type": "created",
            "org_id": "UEk2tZFKAF1LmkfzgbyA",
            "user_id": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
            "multihash": "QmYo9NuU99zHSwsyfP63JyUtK38sjkPgAhBVF92w2fsVGy",
            "tx_hash": "0x65911ace3fae6862a89bf58f9cb581623c91853fee1e899901a4d0b5fab6e678"
          }
        ]
      }
    }
  }
}
```
</details>

## Locations

### GET Location

```sh
curl "$GOTRACE_API/v1/locations/$LOCATION_ID" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```  

<details>
    <summary>Example Response</summary>
    
```json
{
  "loc": {
    "updated_at": "2021-05-21T12:31:47.547573Z",
    "created_at": "2021-05-21T12:31:47.547573Z",
    "id": "WOWTxCg0Ml1XaoxIw1Gx",
    "org_id": "4OL3XCCtl0GfoEmN7iVz",
    "hidden_at": null,
    "name": "Test Tract",
    "status": "",
    "address": "test",
    "lat": 12,
    "lon": 4,
    "geo": "s44u7089nf0h",
    "radius": 1,
    "fuzzy": null,
    "type": {
      "id": "kSjbZZXKG9XOLOyift3g",
      "name": "Tract",
      "data_form": {
        "id": "e4lqNRi3WN8DlQXTbiah",
        "name": "Tract Demo"
      },
      "forms": [
        {
          "id": "pm4Hhey53nfw4hwFiShH",
          "name": "Field Inspection Demo"
        }
      ]
    },
    "data": {
      "comments": null,
      "county": "Nowhere",
      "gps-lat": "12.1",
      "gps-long": "10.2",
      "harvest_volume": "32",
      "kept_forested": true,
      "landowner-name": "Owner",
      "landowner-type": "family-forest",
      "longleaf_present": "yes",
      "longleaf_restoration": "no",
      "mill": "Greenwood",
      "name": "Named",
      "prev_tract_number": "",
      "region": "Southeast",
      "source_id": "asdf123",
      "stand_data": [
        {
          "acreage": "5",
          "age_class": "31-40",
          "forest_cover_type": "pine-with-hw",
          "harvest_type": "preharest",
          "history": null,
          "planted": "yes",
          "preharvest_stocking": "under",
          "regeneration": "planted-same",
          "tons_to_enviva": "4"
        }
      ],
      "stand_data_count": "1",
      "state": "NC"
    }
  }
}
```    
</details>

### GET Location Data

Fetch a location's `data` (custom fields).

```sh
curl "$GOTRACE_API/v1/locations/$LOCATION_ID/data" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```  

<details>
    <summary>Example Response</summary>
    
```json
{
  "data": {
    "comments": null,
    "county": "Nowhere",
    "gps-lat": "12.1",
    "gps-long": "10.2",
    "harvest_volume": "32",
    "kept_forested": true,
    "landowner-name": "Owner",
    "landowner-type": "family-forest",
    "longleaf_present": "yes",
    "longleaf_restoration": "no",
    "mill": "Greenwood",
    "name": "Named",
    "prev_tract_number": "",
    "region": "Southeast",
    "source_id": "asdf123",
    "stand_data": [
      {
        "acreage": "5",
        "age_class": "31-40",
        "forest_cover_type": "pine-with-hw",
        "harvest_type": "preharest",
        "history": null,
        "planted": "yes",
        "preharvest_stocking": "under",
        "regeneration": "planted-same",
        "tons_to_enviva": "4"
      }
    ],
    "stand_data_count": "1",
    "state": "NC"
  }
}
```    
</details>

### GET Locations

List locations for an organization, sorted by recently updated.

```sh
curl "$GOTRACE_API/v1/orgs/$ORG_ID/locations" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```  

#### Query Parameters

- `start_at datetime`
- `limit int`

<details><summary>Example Response</summary>

```json
{
    "locations": [
        {
            "updated_at": "2021-05-20T20:36:46.576967Z",
            "created_at": "2021-05-19T12:43:31.255573Z",
            "id": "eUHZAR5rMeCcnpRZXgN8",
            "org_id": "4OL3XCCtl0GfoEmN7iVz",
            "hidden_at": null,
            "name": "Test",
            "status": "",
            "address": "test",
            "lat": 7,
            "lon": 9,
            "geo": "s1nruf39gnw6",
            "radius": 1,
            "fuzzy": null,
            "type": {
                "id": "kSjbZZXKG9XOLOyift3g",
                "name": "Jordan's Tract and Stand Test",
                "data_form": {
                    "id": "psD3Xhd8G17YwbAmfdi2",
                    "name": "Tract Demo"
                },
                "forms": [
                    {
                        "id": "pm4Hhey53nfw4hwFiShH",
                        "name": "Field Inspection Demo"
                    }
                ]
            },
            "data": {
                "comments": null,
                "county": "C",
                "fiber": null,
                "gps-lat": null,
                "gps-long": null,
                "harvest_volume": null,
                "landowner-name": null,
                "landowner-type": null,
                "longleaf_present": "no",
                "longleaf_restoration": "yes",
                "mill": null,
                "name": "N",
                "prev_tract_number": null,
                "source_id": null,
                "stand_data": [
                    {
                        "acreage": null,
                        "age_class": "11-20",
                        "forest_cover_type": "pine-no-hw",
                        "harvest_type": "selection",
                        "history": null,
                        "kept_forested": true,
                        "planted": "yes",
                        "preharvest_stocking": "under",
                        "regeneration": "planted-same",
                        "tons_to_enviva": null
                    },
                    {
                        "acreage": null,
                        "age_class": "11-20",
                        "forest_cover_type": "pine-with-hw",
                        "harvest_type": "preharest",
                        "history": null,
                        "kept_forested": true,
                        "planted": null,
                        "preharvest_stocking": "under",
                        "regeneration": "na",
                        "tons_to_enviva": null
                    }
                ],
                "stand_data_count": "2",
                "state": "S"
            }
        }
    ]
}
```
</details>

### GET Locations (v2)

List locations for an organization, sorted by recently updated.

```sh
curl "$GOTRACE_API/v2/orgs/$ORG_ID/locations" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```  

#### Query Parameters

- `last_id string` last location id
- `limit int` locations count for pagination (if not passed - set to 50, max - 500) 

<details><summary>Example Response</summary>

```json
{
    "locations": [
        {
            "updated_at": "2021-05-20T20:36:46.576967Z",
            "created_at": "2021-05-19T12:43:31.255573Z",
            "id": "eUHZAR5rMeCcnpRZXgN8",
            "org_id": "4OL3XCCtl0GfoEmN7iVz",
            "hidden_at": null,
            "name": "Test",
            "status": "",
            "address": "test",
            "lat": 7,
            "lon": 9,
            "geo": "s1nruf39gnw6",
            "radius": 1,
            "fuzzy": null,
            "type": {
                "id": "kSjbZZXKG9XOLOyift3g",
                "name": "Jordan's Tract and Stand Test",
                "data_form": {
                    "id": "psD3Xhd8G17YwbAmfdi2",
                    "name": "Tract Demo"
                },
                "forms": [
                    {
                        "id": "pm4Hhey53nfw4hwFiShH",
                        "name": "Field Inspection Demo"
                    }
                ]
            },
            "data": {
                "comments": null,
                "county": "C",
                "fiber": null,
                "gps-lat": null,
                "gps-long": null,
                "harvest_volume": null,
                "landowner-name": null,
                "landowner-type": null,
                "longleaf_present": "no",
                "longleaf_restoration": "yes",
                "mill": null,
                "name": "N",
                "prev_tract_number": null,
                "source_id": null,
                "stand_data": [
                    {
                        "acreage": null,
                        "age_class": "11-20",
                        "forest_cover_type": "pine-no-hw",
                        "harvest_type": "selection",
                        "history": null,
                        "kept_forested": true,
                        "planted": "yes",
                        "preharvest_stocking": "under",
                        "regeneration": "planted-same",
                        "tons_to_enviva": null
                    },
                    {
                        "acreage": null,
                        "age_class": "11-20",
                        "forest_cover_type": "pine-with-hw",
                        "harvest_type": "preharest",
                        "history": null,
                        "kept_forested": true,
                        "planted": null,
                        "preharvest_stocking": "under",
                        "regeneration": "na",
                        "tons_to_enviva": null
                    }
                ],
                "stand_data_count": "2",
                "state": "S"
            }
        }
    ]
}
```
</details>

### POST Location Events (with files)


```sh
curl "$GOTRACE_API/v1/locations/$LOCATION_ID/events" \
  -H "Authorization: Bearer $API_TOKEN" \
  -F 0=@photo.png
  -F 1=@document.pdf
  -F event='{"load_id":"2gcjqFd5pEVNwNhhH9u9","created_by":"p8Ov0RnOO9U1fVRJoa5R8JHcOLm1","org_id":"UEk2tZFKAF1LmkfzgbyA","type":"gps-start","geo_point":{"latitude":41.8781,"longitude":-87.6298}}'
```

Data is sent in multipart/form-data format. Event entity is stored in 'event' key as json string. Files are stored in incremented numbers keys (strings that starts from "0"). For example, first file is sotred in 0 key, second - in 1 key, etc. Files are limited by **50 MB**.

<details>
<summary>Example Response</summary>

```json
{
  "event": {
    "updated_at": "2020-07-20T07:53:17.847251591-05:00",
    "created_at": "2020-07-20T07:53:16.481100112-05:00",
    "id": "8pAbKFm28vv1LZ3cCqRO",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "org_id": "UEk2tZFKAF1LmkfzgbyA",
    "entity": "location",
    "entity_id": "2gcjqFd5pEVNwNhhH9u9",
    "type": "note",
    "geo_point": {
      "latitude": 41.8781,
      "longitude": -87.6298
    },
    "media": [
      {
        "url": "https://go-supply-chain.appspot.com.storage.googleapis.com/123432956643768.png",
        "type": "image/*",
        "filename": "photo.png",
      },
      {
        "url": "https://go-supply-chain.appspot.com.storage.googleapis.com/123846128364091.pdf",
        "type": "*.pdf",
        "filename": "document.pdf",
      }
    ]
  }
}
```
</details>

#### Event Types

##### `note`

Event type, that is used to add any additional location info (files, text etc) to location history (location feed). 

##### `form`

Event type, that is used to add custom fields form to location history (location feed). Requires **form_id** and `form_data` fields. **form_id** should contain id of form, which is attached. `form_data` contains fields values, based on form.

<details>
<summary>Example form_data</summary>

```json
{
  "field_1": "value_1",
  "field_2": "value_2"
}
```
</details>

## Forms

Custom form definitions.

### GET Form

```sh
curl "$GOTRACE_API/v1/forms/$FORM_ID" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```  

<details>
    <summary>Example Response</summary>

```json
{
  "form": {
    "updated_at": "2021-05-21T12:21:21.731111Z",
    "created_at": "2021-05-21T12:21:21.731111Z",
    "id": "e4lqNRi3WN8DlQXTbiah",
    "org_id": "4OL3XCCtl0GfoEmN7iVz",
    "created_by": "p8Ov0RnOO9U1fVRJoa5R8JHcOLm1",
    "name": "Tract Demo",
    "fields": [
      {
        "name": "kept_forested",
        "label": "This land will be kept in forested use",
        "type": "checkbox",
        "required": true
      },
      {
        "name": "region",
        "label": "Fiber Supply Region",
        "type": "dropdown",
        "required": true,
        "options": [
          {
            "value": "Mid-Atlantic"
          },
          {
            "value": "Wilmington"
          },
          {
            "value": "Southeast"
          },
          {
            "value": "Pascagoula"
          }
        ]
      },
      {
        "name": "mill",
        "label": "Mill",
        "type": "dropdown",
        "required": true,
        "options": [
          {
            "value": "Northampton"
          },
          {
            "value": "Southampton"
          },
          {
            "value": "Ahoskie"
          },
          {
            "value": "Sampson"
          },
          {
            "value": "Greenwood"
          },
          {
            "value": "Hamlet"
          },
          {
            "value": "Cottondale"
          },
          {
            "value": "Amory"
          },
          {
            "value": "Waycross"
          },
          {
            "value": "Lucedale"
          }
        ]
      },
      {
        "name": "source_id",
        "label": "Source ID",
        "required": true,
        "max_lines": 1
      },
      {
        "name": "prev_tract_number",
        "label": "Previous Tract Number",
        "description": "Has the tract been entered before?",
        "max_lines": 1
      },
      {
        "name": "name",
        "label": "Tract Name",
        "required": true,
        "max_lines": 1
      },
      {
        "name": "county",
        "label": "Tract County",
        "required": true,
        "max_lines": 1
      },
      {
        "name": "state",
        "label": "Tract State",
        "type": "choicechip",
        "required": true,
        "options": [
          {
            "value": "AL"
          },
          {
            "value": "FL"
          },
          {
            "value": "GA"
          },
          {
            "value": "NC"
          },
          {
            "value": "MS"
          },
          {
            "value": "PA"
          },
          {
            "value": "SC"
          },
          {
            "value": "VA"
          }
        ]
      },
      {
        "name": "gps-lat",
        "label": "GPS Lat",
        "type": "text",
        "value_type": "number",
        "required": true,
        "decimals": 4,
        "range_min": -90,
        "range_max": 90
      },
      {
        "name": "gps-long",
        "label": "GPS Long",
        "type": "text",
        "value_type": "number",
        "required": true,
        "decimals": 4,
        "range_min": -180,
        "range_max": 180
      },
      {
        "name": "landowner-name",
        "label": "Landowner Name",
        "required": true,
        "max_lines": 1
      },
      {
        "name": "landowner-type",
        "label": "Landowner Type",
        "type": "radio",
        "required": true,
        "options": [
          {
            "value": "family-forest",
            "label": "Family Forest (NIPL)"
          },
          {
            "value": "industrial",
            "label": "Industrial (TIMO/REIT)"
          },
          {
            "value": "state-federal",
            "label": "State/Federal"
          }
        ]
      },
      {
        "name": "harvest_volume",
        "label": "Estimated % harvest volume to Enviva",
        "type": "text",
        "value_type": "number",
        "required": true,
        "decimals": 0,
        "range_min": 1,
        "range_max": 100
      },
      {
        "name": "longleaf_present",
        "label": "Longleaf Present",
        "type": "choicechip",
        "description": "Is there longleaf pine present on the tract?",
        "required": true,
        "options": [
          {
            "value": "yes",
            "label": "Yes"
          },
          {
            "value": "no",
            "label": "No"
          }
        ]
      },
      {
        "name": "longleaf_restoration",
        "label": "Longleaf Restoration",
        "type": "choicechip",
        "description": "Is this a longleaf pine restoration harvest?",
        "required": true,
        "options": [
          {
            "value": "yes",
            "label": "Yes"
          },
          {
            "value": "no",
            "label": "No"
          }
        ]
      },
      {
        "name": "stand_data",
        "label": "How many stands are there total within the tract?",
        "type": "formlist",
        "required": true,
        "default_value": "1",
        "options": [
          {
            "value": "1"
          },
          {
            "value": "2"
          },
          {
            "value": "3"
          },
          {
            "value": "4"
          },
          {
            "value": "5"
          }
        ],
        "fields": [
          {
            "name": "planted",
            "label": "Planted",
            "type": "choicechip",
            "description": "Was the stand established by planting?",
            "options": [
              {
                "value": "yes",
                "label": "Yes"
              },
              {
                "value": "no",
                "label": "No"
              },
              {
                "value": "unknown",
                "label": "Unknown"
              }
            ]
          },
          {
            "name": "regeneration",
            "label": "Regeneration",
            "type": "radio",
            "description": "How will stand be regenerated after harvest?",
            "required": true,
            "options": [
              {
                "value": "planted-same",
                "label": "Planted- same species as prior to harvest"
              },
              {
                "value": "planted-different",
                "label": "Planted- different species as prior to harvest"
              },
              {
                "value": "planted-unknown",
                "label": "Planted- unknown species"
              },
              {
                "value": "natural-regen",
                "label": "Naturally Regenerated"
              },
              {
                "value": "na",
                "label": "NA- Arboriculture/Salvage, Preharvest, Selection, or Thinning"
              },
              {
                "value": "unknown",
                "label": "Unknown"
              }
            ]
          },
          {
            "name": "preharvest_stocking",
            "label": "Preharvest Stocking",
            "type": "radio",
            "description": "Describe the stand's stocking preharvest",
            "required": true,
            "options": [
              {
                "value": "under",
                "label": "Under Stocked"
              },
              {
                "value": "full",
                "label": "Fully Stocked"
              },
              {
                "value": "over",
                "label": "Overstocked"
              }
            ]
          },
          {
            "name": "tons_to_enviva",
            "label": "Estimated tons to Enviva",
            "type": "text",
            "value_type": "number",
            "required": true,
            "decimals": 0,
            "range_min": 1
          },
          {
            "name": "forest_cover_type",
            "label": "Forest cover type",
            "type": "radio",
            "required": true,
            "options": [
              {
                "value": "pine-with-hw",
                "label": "Pine with hardwood understory"
              },
              {
                "value": "pine-no-hw",
                "label": "Pine with no hardwood understory"
              },
              {
                "value": "mixed-pine-hw",
                "label": "Mixed pine-hardwood"
              },
              {
                "value": "hw-bottomland",
                "label": "Bottomland hardwood"
              },
              {
                "value": "hw-other",
                "label": "Other hardwood"
              }
            ]
          },
          {
            "name": "harvest_type",
            "label": "Harvest type",
            "type": "radio",
            "required": true,
            "options": [
              {
                "value": "arbo_salv",
                "label": "Arbo/Salv"
              },
              {
                "value": "clearcut",
                "label": "Clearcut"
              },
              {
                "value": "preharest",
                "label": "Preharvest"
              },
              {
                "value": "seed-tree",
                "label": "Seed Tree"
              },
              {
                "value": "selection",
                "label": "Selection"
              },
              {
                "value": "thinning",
                "label": "Thinning"
              }
            ]
          },
          {
            "name": "age_class",
            "label": "Age class",
            "type": "choicechip",
            "required": true,
            "options": [
              {
                "value": "0-10"
              },
              {
                "value": "11-20"
              },
              {
                "value": "21-30"
              },
              {
                "value": "31-40"
              },
              {
                "value": "41-50"
              },
              {
                "value": "51-60"
              },
              {
                "value": "61-70"
              },
              {
                "value": "71-80"
              },
              {
                "value": "81-90"
              },
              {
                "value": "91+"
              }
            ]
          },
          {
            "name": "acreage",
            "label": "Acreage",
            "type": "text",
            "value_type": "number",
            "required": true,
            "decimals": 0,
            "range_min": 1
          },
          {
            "name": "history",
            "label": "Stand History",
            "description": "Please provide additional information that will help us understand\nthe stand. Examples of useful information include: Year the stand\nwas previously harvested / when current forest was established. Is\nthis a âsecond entryâ? Water features present?"
          }
        ]
      },
      {
        "name": "comments",
        "label": "Additional Comments"
      }
    ]
  }
}
```

</details>

### POST Form response

```sh
curl "$GOTRACE_API/v1/forms/$FORM_ID/responses/$RESPONSE_ID" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
  --data-raw '{"audit_type":"post","img_inspection":"no","water_features":["ditches"],"smz_compliant":"yes","stream_crossings":"no","hcv_priority_present":["low-pocosin"],"land_use_change":"yes","hydrology_altered":"2","flow_maintained":"1","minimized_disturbance":"2","residual_impact":"1","appropriate_timing":"2","stream_debris_clear":"2","stream_crossing_stable":"2","soil_erosion_prevention":"2","smz_width":"2","trash_cleanup":"2","oil_spill_presence":"1","haul_road_condition":"2","waterbars_installed":"1","origin_wood_documentation":"no","from_known":"yes","native_species":"no","econ_haul_radius":"no","timber_theft_trespass":"yes","rights_violation":"yes","genetic_modified":"no","weland":"yes","peatland":"no","comments":null,"overall_tract_compliance":"in"}'
```  

### GET Form response

```sh
curl "$GOTRACE_API/v1/forms/$FORM_ID/responses/$RESPONSE_ID" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```

<details>
    <summary>Example Response</summary>

```json
{
   "response":{
      "updated_at":"2021-05-25T15:42:01.377693Z",
      "created_at":"2021-05-25T15:37:27.918248Z",
      "id":"loCbfjz3oM",
      "status":"done",
      "form_id":"K3NF9VO0A",
      "location_id":"glG6bUS",
      "data":{
         "appropriate_timing":"2",
         "audit_type":"post",
         "comments":null,
         "econ_haul_radius":"no",
         "flow_maintained":"1",
         "from_known":"yes",
         "genetic_modified":"no",
         "haul_road_condition":"2",
         "hcv_priority_present":[
            "low-pocosin"
         ],
         "hydrology_altered":"2",
         "img_inspection":"no",
         "land_use_change":"yes",
         "minimized_disturbance":"2",
         "native_species":"no",
         "oil_spill_presence":"1",
         "origin_wood_documentation":"no",
         "overall_tract_compliance":"in",
         "peatland":"no",
         "residual_impact":"1",
         "rights_violation":"yes",
         "smz_compliant":"yes",
         "smz_width":"2",
         "soil_erosion_prevention":"2",
         "stream_crossing_stable":"2",
         "stream_crossings":"no",
         "stream_debris_clear":"2",
         "timber_theft_trespass":"yes",
         "trash_cleanup":"2",
         "water_features":[
            "ditches"
         ],
         "waterbars_installed":"1",
         "weland":"yes"
      }
   }
}
```

</details>

### GET Form responses

```sh
curl "$GOTRACE_API/v1/forms/$FORM_ID/responses" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```

#### Query Parameters

- `created_after datetime`
- `created_before datetime`

<details>
    <summary>Example Response</summary>

```json
{
   {
   "responses":[
      {
         "updated_at":"2021-05-25T15:42:01.377693Z",
         "created_at":"2021-05-25T15:37:27.918248Z",
         "id":"loCbfjz3o",
         "status":"done",
         "form_id":"K3NF9VO0",
         "location_id":"glG6bUS",
         "created_by":"",
         "data":{
            "appropriate_timing":"2",
            "audit_type":"post",
            "comments":null,
            "econ_haul_radius":"no",
            "flow_maintained":"1",
            "from_known":"yes",
            "genetic_modified":"no",
            "haul_road_condition":"2",
            "hcv_priority_present":[
               "low-pocosin"
            ],
            "hydrology_altered":"2",
            "img_inspection":"no",
            "land_use_change":"yes",
            "minimized_disturbance":"2",
            "native_species":"no",
            "oil_spill_presence":"1",
            "origin_wood_documentation":"no",
            "overall_tract_compliance":"in",
            "peatland":"no",
            "residual_impact":"1",
            "rights_violation":"yes",
            "smz_compliant":"yes",
            "smz_width":"2",
            "soil_erosion_prevention":"2",
            "stream_crossing_stable":"2",
            "stream_crossings":"no",
            "stream_debris_clear":"2",
            "timber_theft_trespass":"yes",
            "trash_cleanup":"2",
            "water_features":[
               "ditches"
            ],
            "waterbars_installed":"1",
            "weland":"yes"
         }
      },
      {
         "updated_at":"2021-05-25T15:36:50.402077Z",
         "created_at":"2021-05-25T15:36:50.402077Z",
         "id":"TITUqoqirf",
         "status":"invited",
         "form_id":"K3NF9VO0A",
         "location_id":"glG6bUSe",
         "created_by":""
      },
      {
         "updated_at":"2021-05-25T01:41:31.972429Z",
         "created_at":"2021-05-25T01:40:32.121555Z",
         "id":"zhiBZeD68",
         "status":"done",
         "form_id":"K3NF9VO0A",
         "location_id":"glG6bUSe",
         "created_by":"",
         "data":{
            "appropriate_timing":"2",
            "audit_type":"post",
            "comments":null,
            "econ_haul_radius":"yes",
            "flow_maintained":"2",
            "from_known":"yes",
            "genetic_modified":"no",
            "haul_road_condition":"1",
            "hcv_priority_present":[
               "low-pocosin"
            ],
            "hydrology_altered":"3",
            "img_inspection":"yes",
            "land_use_change":"no",
            "minimized_disturbance":"3",
            "native_species":"no",
            "oil_spill_presence":"2",
            "origin_wood_documentation":"no",
            "overall_tract_compliance":"in",
            "peatland":"no",
            "residual_impact":"2",
            "rights_violation":"yes",
            "smz_compliant":"yes",
            "smz_width":"2",
            "soil_erosion_prevention":"na",
            "stream_crossing_stable":"2",
            "stream_crossings":"yes",
            "stream_debris_clear":"3",
            "timber_theft_trespass":"no",
            "trash_cleanup":"3",
            "water_features":[
               "defined-stream"
            ],
            "waterbars_installed":"2",
            "weland":"yes"
         }
      },
      {
         "updated_at":"2021-05-25T01:38:52.435895Z",
         "created_at":"2021-05-25T01:37:40.74711Z",
         "id":"txdE65KOoH",
         "status":"done",
         "form_id":"K3NF9VO0A",
         "location_id":"glG6bUS",
         "created_by":"",
         "data":{
            "appropriate_timing":"2",
            "audit_type":"ongoing",
            "comments":null,
            "econ_haul_radius":"no",
            "flow_maintained":"2",
            "from_known":"no",
            "genetic_modified":"yes",
            "haul_road_condition":"0",
            "hcv_priority_present":[
               "atlantic-white-cedar"
            ],
            "hydrology_altered":"2",
            "img_inspection":"yes",
            "land_use_change":"yes",
            "minimized_disturbance":"1",
            "native_species":"yes",
            "oil_spill_presence":"2",
            "origin_wood_documentation":"yes",
            "overall_tract_compliance":"in",
            "peatland":"yes",
            "residual_impact":"0",
            "rights_violation":"no",
            "smz_compliant":"yes",
            "smz_width":"0",
            "soil_erosion_prevention":"2",
            "stream_crossing_stable":"1",
            "stream_crossings":"yes",
            "stream_debris_clear":"3",
            "timber_theft_trespass":"yes",
            "trash_cleanup":"2",
            "water_features":[
               "defined-stream"
            ],
            "waterbars_installed":"2",
            "weland":"no"
         }
      },
      {
         "updated_at":"2021-05-24T23:47:03.278445Z",
         "created_at":"2021-05-24T23:47:03.278445Z",
         "id":"kEkTIyZyAF",
         "status":"invited",
         "form_id":"K3NF9VO0Ap",
         "location_id":"glG6bUSeC",
         "created_by":""
      },
      {
         "updated_at":"2021-05-24T23:25:01.156167Z",
         "created_at":"2021-05-24T23:25:01.156167Z",
         "id":"fm9R1yB7k92XhKEFFGBt",
         "status":"invited",
         "form_id":"K3NF9VO0ApLycw6gmwMR",
         "location_id":"glG6bUSeCr66tSAUDzHo",
         "created_by":""
      },
      {
         "updated_at":"2021-05-24T23:24:15.36479Z",
         "created_at":"2021-05-24T23:11:43.380386Z",
         "id":"kttFBzAH3",
         "status":"done",
         "form_id":"K3NF9VO0Ap",
         "location_id":"glG6bUS",
         "created_by":"",
         "data":{
            "appropriate_timing":"3",
            "audit_type":"ongoing",
            "comments":null,
            "econ_haul_radius":"yes",
            "flow_maintained":"3",
            "from_known":"yes",
            "genetic_modified":"yes",
            "haul_road_condition":"3",
            "hcv_priority_present":[
               "carolina-bay"
            ],
            "hydrology_altered":"3",
            "img_inspection":"yes",
            "land_use_change":"no",
            "minimized_disturbance":"2",
            "native_species":"yes",
            "oil_spill_presence":"3",
            "origin_wood_documentation":"yes",
            "overall_tract_compliance":"in",
            "peatland":"yes",
            "residual_impact":"2",
            "rights_violation":"no",
            "smz_compliant":"yes",
            "smz_width":null,
            "soil_erosion_prevention":null,
            "stream_crossing_stable":"3",
            "stream_crossings":"yes",
            "stream_debris_clear":"3",
            "timber_theft_trespass":"no",
            "trash_cleanup":"3",
            "water_features":null,
            "waterbars_installed":"3",
            "weland":"no"
         }
      }
   ]
}
}
```

</details>

### GET Form responses by location

```sh
curl "$GOTRACE_API/v1/locations/$LOCATION_ID/forms/$FORM_ID/responses" \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer $API_TOKEN"
```

#### Query Parameters

- `created_after datetime`
- `created_before datetime`

<details>
    <summary>Example Response</summary>

```json
{
   {
   "responses":[
      {
         "updated_at":"2021-05-25T15:42:01.377693Z",
         "created_at":"2021-05-25T15:37:27.918248Z",
         "id":"loCbfjz3o",
         "status":"done",
         "form_id":"K3NF9VO0A",
         "location_id":"glG6bUSe",
         "created_by":"",
         "data":{
            "appropriate_timing":"2",
            "audit_type":"post",
            "comments":null,
            "econ_haul_radius":"no",
            "flow_maintained":"1",
            "from_known":"yes",
            "genetic_modified":"no",
            "haul_road_condition":"2",
            "hcv_priority_present":[
               "low-pocosin"
            ],
            "hydrology_altered":"2",
            "img_inspection":"no",
            "land_use_change":"yes",
            "minimized_disturbance":"2",
            "native_species":"no",
            "oil_spill_presence":"1",
            "origin_wood_documentation":"no",
            "overall_tract_compliance":"in",
            "peatland":"no",
            "residual_impact":"1",
            "rights_violation":"yes",
            "smz_compliant":"yes",
            "smz_width":"2",
            "soil_erosion_prevention":"2",
            "stream_crossing_stable":"2",
            "stream_crossings":"no",
            "stream_debris_clear":"2",
            "timber_theft_trespass":"yes",
            "trash_cleanup":"2",
            "water_features":[
               "ditches"
            ],
            "waterbars_installed":"1",
            "weland":"yes"
         }
      },
      {
         "updated_at":"2021-05-25T15:36:50.402077Z",
         "created_at":"2021-05-25T15:36:50.402077Z",
         "id":"TITUqoqir",
         "status":"invited",
         "form_id":"K3NF9VO0",
         "location_id":"glG6bUSe",
         "created_by":""
      },
      {
         "updated_at":"2021-05-25T01:41:31.972429Z",
         "created_at":"2021-05-25T01:40:32.121555Z",
         "id":"zhiBZeD68",
         "status":"done",
         "form_id":"K3NF9VO0Ap",
         "location_id":"glG6bUSe",
         "created_by":"",
         "data":{
            "appropriate_timing":"2",
            "audit_type":"post",
            "comments":null,
            "econ_haul_radius":"yes",
            "flow_maintained":"2",
            "from_known":"yes",
            "genetic_modified":"no",
            "haul_road_condition":"1",
            "hcv_priority_present":[
               "low-pocosin"
            ],
            "hydrology_altered":"3",
            "img_inspection":"yes",
            "land_use_change":"no",
            "minimized_disturbance":"3",
            "native_species":"no",
            "oil_spill_presence":"2",
            "origin_wood_documentation":"no",
            "overall_tract_compliance":"in",
            "peatland":"no",
            "residual_impact":"2",
            "rights_violation":"yes",
            "smz_compliant":"yes",
            "smz_width":"2",
            "soil_erosion_prevention":"na",
            "stream_crossing_stable":"2",
            "stream_crossings":"yes",
            "stream_debris_clear":"3",
            "timber_theft_trespass":"no",
            "trash_cleanup":"3",
            "water_features":[
               "defined-stream"
            ],
            "waterbars_installed":"2",
            "weland":"yes"
         }
      },
      {
         "updated_at":"2021-05-25T01:38:52.435895Z",
         "created_at":"2021-05-25T01:37:40.74711Z",
         "id":"txdE65KOo",
         "status":"done",
         "form_id":"K3NF9VO0A",
         "location_id":"glG6bUS",
         "created_by":"",
         "data":{
            "appropriate_timing":"2",
            "audit_type":"ongoing",
            "comments":null,
            "econ_haul_radius":"no",
            "flow_maintained":"2",
            "from_known":"no",
            "genetic_modified":"yes",
            "haul_road_condition":"0",
            "hcv_priority_present":[
               "atlantic-white-cedar"
            ],
            "hydrology_altered":"2",
            "img_inspection":"yes",
            "land_use_change":"yes",
            "minimized_disturbance":"1",
            "native_species":"yes",
            "oil_spill_presence":"2",
            "origin_wood_documentation":"yes",
            "overall_tract_compliance":"in",
            "peatland":"yes",
            "residual_impact":"0",
            "rights_violation":"no",
            "smz_compliant":"yes",
            "smz_width":"0",
            "soil_erosion_prevention":"2",
            "stream_crossing_stable":"1",
            "stream_crossings":"yes",
            "stream_debris_clear":"3",
            "timber_theft_trespass":"yes",
            "trash_cleanup":"2",
            "water_features":[
               "defined-stream"
            ],
            "waterbars_installed":"2",
            "weland":"no"
         }
      },
      {
         "updated_at":"2021-05-24T23:47:03.278445Z",
         "created_at":"2021-05-24T23:47:03.278445Z",
         "id":"kEkTIyZyA",
         "status":"invited",
         "form_id":"K3NF9VO0A",
         "location_id":"glG6bUSe",
         "created_by":""
      },
      {
         "updated_at":"2021-05-24T23:25:01.156167Z",
         "created_at":"2021-05-24T23:25:01.156167Z",
         "id":"fm9R1yB7k9",
         "status":"invited",
         "form_id":"K3NF9VO0Ap",
         "location_id":"glG6bUS",
         "created_by":""
      },
      {
         "updated_at":"2021-05-24T23:24:15.36479Z",
         "created_at":"2021-05-24T23:11:43.380386Z",
         "id":"kttFBzAH",
         "status":"done",
         "form_id":"K3NF9VO0",
         "location_id":"glG6bUSe",
         "created_by":"",
         "data":{
            "appropriate_timing":"3",
            "audit_type":"ongoing",
            "comments":null,
            "econ_haul_radius":"yes",
            "flow_maintained":"3",
            "from_known":"yes",
            "genetic_modified":"yes",
            "haul_road_condition":"3",
            "hcv_priority_present":[
               "carolina-bay"
            ],
            "hydrology_altered":"3",
            "img_inspection":"yes",
            "land_use_change":"no",
            "minimized_disturbance":"2",
            "native_species":"yes",
            "oil_spill_presence":"3",
            "origin_wood_documentation":"yes",
            "overall_tract_compliance":"in",
            "peatland":"yes",
            "residual_impact":"2",
            "rights_violation":"no",
            "smz_compliant":"yes",
            "smz_width":null,
            "soil_erosion_prevention":null,
            "stream_crossing_stable":"3",
            "stream_crossings":"yes",
            "stream_debris_clear":"3",
            "timber_theft_trespass":"no",
            "trash_cleanup":"3",
            "water_features":null,
            "waterbars_installed":"3",
            "weland":"no"
         }
      }
   ]
}
}
```

</details>
