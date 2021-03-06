# Group API

The Group API returns only the metadata for each group.

<div class="wrap">
  <p class="flash info">
    The Group API does not return the records associated with each group. To get the associated records use the get the <code>filter_id</code> from this endpoint and use this value in the <a href="#group-page-api">Group Pages API</a>.
  </p>
</div>

<div class="wrap">
  <p class="flash warn">
    The Group API funcionality differs depending on if the group is a smart or a static group. For an explanation of the different types of groups see the <a href="http://support.salesseek.net/general/groups-and-reporting/introduction-to-groups">help center article</a>.
  </p>
</div>

## Group Attributes

> A sample smart group response would look like this.

```json
{
	"id": "9c254f26-2217-4d21-b7b5-49591fde0a2f",
	"name": "🇬🇧 UK Users",
	"group_type": "smart",
	"element_type": "individuals",
	"locked": false,
	"comments": "All current users of the SalesSeek system. ",
	"mailing_list": false,
	"created": "2017-06-13T16:01:07.192856",
	"modified": "2017-06-13T17:46:27.194513",
	"owner_id": "69c5bc2a-c8e5-4d11-a102-6ec0fb410d1b",
	"creator_id": "69c5bc2a-c8e5-4d11-a102-6ec0fb410d1b",
	"last_modified_by_id": "69c5bc2a-c8e5-4d11-a102-6ec0fb410d1b",
	"owner": {
		...
	},
	"creator": {
		...
	},
	"filter_id": "9d60827c-89da-4e31-8b62-0258f3adbd59",
	"filter": {
		...
	},
	"columns": [
    "photo_url",
    "first_name",
    "last_name",
    "organization.name",
    "email",
    "phone",
    "sortable_name"
  ],
	"order_by": null,
	"cost_basis": "total",
	"cost": 0,
	"target": null,
}
```

Parameter |  Description
--------- | ------- 
`id`      | The unique identifier for this group **String**
`name`		| User provided name for the group **String**
`group_type` | The type of group (`static`, `smart`) **Enum (String)**
`element_type` | The type of record this group can contain (`individuals`, `organizations`, `opportunities`, `tasks`)**Enum (String)**
`locked`  | Set to `true` if editing this group has been locked (usually the result of the group being used in an automation) **boolean**
`comments`| User provided comments associated with the group **String**
`mailing_list` | Set to `true` if this group is a mailing list (only applicable on static groups) **boolean**
`created` | Group creation timestamp **Timestamp (ISO 8601)**
`modified`| Group last modified timestamp **Timestamp (ISO 8601)**
`owner_id` | ID for the [User](#user-api) that has ownership for this group **String**
`creator_id` | ID for the [User](#user-api) that created this group **String**
`last_modified_by_id` | ID for the [User](#user-api) that last modified the group **String**
`creator` | Object containing detailed information about the [User](#user-api) that created this group **Object**
`owner` | Object containing detailed information about the [User](#user-api) that owns this group **Object**
`filter_id` | The ID for the group [filter](#filter-api) **String**
`filter` | Object containing detailed information about the group filter **Object**
`columns` | Array of column names to be shown in this group **Object**
`creator` | Object containing detailed information about the [User](#user-api) that created this group **Object**
`display_options` | Object containing display options for this group **Object**
`display_options.sort.direction` | Set to `true` if sort order is ascending, set to `false` for descending **Boolean**
`display_options.sort.field` | The name of the column to apply the sort ordering to **String**
`display_options.show_as` | Set to either `table` or `list` depending on the chosen default view for this group **String**

## Get Group

Returns the group matching the `group_id`

### Request URL

<span class='verb get'>GET</span> `https://{CLIENT_ID}.salesseek.net/api/groups/{group_id}`

### Request Query Parameters

Parameter |  Description
--------- | ------- 
`group_id` | The ID for the group you'd like to retrieve



## Create Group

> An example request body to create a smart group called '🇺🇸 US Customers' would be:

```json
{
	"name": "🇺🇸 US Customers",
	"group_type": "smart",
	"element_type": "individuals",
}
```
> When the group is created it will contain all individual records. You need to apply a [filter](#filter-api) to this group to filter to customers from the US.

> By default group owner will be assigned to the user creating the group. You can pass a `owner` object to set the group owner to a different user.


Creates a new group and then returns the newly created group.

### Request URL

<span class='verb post'>POST</span> `https://{CLIENT_ID}.salesseek.net/api/{record_type}/filters/`

### Request Body Parameters

Parameter |  Description
--------- | ------- 
`name` | Provided name for the group **String**
`group_type` | The type of group (`static`, `smart`) **Enum (String)**
`element_type` | The type of record this group can contain (`individuals`, `organizations`, `opportunities`, `tasks`) **Enum (String)**


## Update Group

> An example request body to change the group owner would be:

```json
{
	"owner_id": "40575ca1-381c-4542-a556-e3c2d81ef67e"
}
```

> An example request body to set a filter to smart group would be:

```json
{
	"filter_id": "0c8b0614-db0d-4b90-ace2-b2eabea66788"
}
```

Updates values of group by `group_id`. The resulting group is returned. 

### Request URL

<span class='verb patch'>PATCH</span> `https://{CLIENT_ID}.salesseek.net/api/groups/{group_id}`

Parameter |  Description
--------- | ------- 
`group_id`      | The ID for the group you'd like to update **String**

### Request Body

The only fields updated are the one passed on the request body JSON. You can pass any [attribute](#group-attributes) to update. 


## Delete Group

Deletes the group matching the `group_id`

### Request URL

<span class='verb delete'>DELETE</span> `https://{CLIENT_ID}.salesseek.net/api/groups/{group_id}`

### Request Query Parameters

Parameter |  Description
--------- | ------- 
`group_id` | The unique ID for the group to be deleted.


## List Groups

> An example request to get all individual groups would be: 

```http
GET https://example.salesseek.net/api/groups?start=0&rows=-1&element_type=individuals
```

Returns a list of groups from your SalesSeek account.

### Request URL

<span class='verb get'>GET</span> `https://{CLIENT_ID}.salesseek.net/api/groups`

### Request Query Parameters

Parameter |  Description
--------- | ------- 
`element_type` | Filter by groups containing certain record types (`organizations`, `opportunities`, `individuals`).
`rows` | The maximum number of organizations to be returned
`start` | The row number to start to retrieve data. (0 for start)
`order_by` | Results are ordered by the provided field name followed by `&desc` or `&asc`

<div class="wrap">
  <p class="flash info">
    If <code>element_type</code> is not provided individual and opportunity groups are returned.
  </p>
</div>

## Add Record to Static Group

> An example request to add a record to a group would be. 

```http
PUT https://example.salesseek.net/api/groups/13663429-0f49-4e83-836c-7ecd6d58adf5/items/4bb55a82-ee66-4752-bc47-ce7edd05ddb1
```

Adds a record to a static group.

<span class='verb put'>PUT</span>  `https://{CLIENT_ID}.salesseek.net/api/groups/{group_id}/items/{record_id}` 

### Request Query Parameters

Parameter |  Description
--------- | ------- 
`group_id` | The unique ID for the group to be deleted
`record_id` | The ID for the record to be added to this group

<div class="wrap">
  <p class="flash warn">
    Records can only be added to static groups. Filters should be applied to smart groups to add records to smart groups.
  </p>
</div>

