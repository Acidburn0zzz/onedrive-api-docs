﻿# OneDrive API optional query parameters

The OneDrive API provides several optional query parameters that can be used
to control the specify the data returned in a response.

## Selecting properties
You can use the _select_ query string parameter to provide a comma-separated
list of properties to return on [Items][item-resource].

### Example

This example selects only the **name** and **size** properties to be returned, when retrieving the children of an item.

```http
GET /drive/root/children?select=name,size
```

By submitting the request with the `select=name,size` query string, the objects
in the response will only have those property values included. However, by default, the **id** value
will always be returned even if its not specified.

<!-- { "blockType": "example", "@odata.type": "oneDrive.item", "isCollection": true, "truncated": true } -->
```json
{
  "value": [
    {
      "id": "13140a9sd9aba",
      "name": "Documents",
      "size": 1024
    },
    {
      "id": "123901909124a",
      "name": "Pictures",
      "size": 1012010210
    }
  ]
}
```

## Expanding collections

In OneDrive API requests, children collections of referenced items are not automatically
expanded. This is by design because it reduces network traffic and the time it takes to generate a response
from the service. However, in some cases you might want to include those results
in a response.

You can use the _expand_ query string parameter to instruct the OneDrive API to expand
a children collection and include those results.

For example, to retrieve the root drive information and the top level items in
a drive you use _expand_ parameter as in the example below. This example also uses a _select_
statement to only return the **id** and **name** properties of the children items.

<!-- { "blockType": "request", "name": "drive-plus-children" } -->
```http
GET /drive/root?expand=children(select=id,name)
```

The request returns the collection items, with the children collection expanded.

<!-- { "blockType": "response", "@odata.type": "oneDrive.item", "truncated": true } -->
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "createdBy": { },
  "createdDateTime": "2008-01-10T20:16:28.017Z",
  "cTag": "aZjpGMDRBQTk2MT...",
  "eTag": "aRjA0QUE5NjE3ND...",
  "id": "root",
  "lastModifiedBy": { },
  "lastModifiedDateTime": "2013-06-20T02:54:44.547Z",
  "name": "root",
  "size": 218753122201,
  "webUrl": "https://onedrive.live.com/?cid=0f040...",
  "folder": {
    "childCount": 4
  },
  "children": [
    {
      "id": "F04AA961744A809!48443",
      "name": "Applications",
    },
    {
      "id": "F04AA961744A809!92647",
      "name": "Attachments",
    },
    {
      "id": "F04AA961744A809!93269",
      "name": "Balsmiq Sketches",
    },
    {
      "id": "F04AA961744A809!65191",
      "name": "Camera imports",
    }
  ]
}
```

## Sorting objects

You can use the _orderby_ query string to control the sort order of the items
returned from the OneDrive API. For a collection of items, use the following fields in the _orderby_ parameter.

* name
* size
* lastModifiedDateTime

To sort the results in ascending or descending order, append
either `asc` or `desc` to the field name, separated by a space, for example,
`?orderby=name%20desc`.

For example, to return the contents of the root of a drive in OneDrive, ordered largest
to smallest, use this syntax: `/drive/items/root/children?orderby=size%20desc`.


## Optional OData query parameters
Here is a table of optional OData query parameters you can use in your OneDrive API requests.

| Name        | Value  | Status        | Description                                                                                                                                                          |
|:------------|:-------|:--------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _expand_    | string | available     | Comma-separated list of relationships to expand and include in the response. For example, to retrieve the children of a folder use `expand=children`.                |
| _select_    | string | available     | Comma-separated list of properties to include in the response.                                                                                                       |
| _skipToken_ | string | available     | Paging token that is used to get the next set of results.                                                                                                            |
| _top_       | int    | available     | The number of items to return in a result set. The OneDrive API may have a hard limit that prevents you from asking for more items per response.                     |
| _orderby_   | string | available     | Comma-separated list of properties that are used to sort the order of items in the response collection. Works for `name`, `size`, and `lastModifiedDateTime` fields. |
| _filter_    | string | not available | Filter string that lets you filter the response based on a set of criteria.                                                                                          |


 [item-resource]: ../resources/item.md
