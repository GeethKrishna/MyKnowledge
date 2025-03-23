#### Current
{
    name: ...
    displayName: ... (not there)
    type: ...
}

#### Proposed
{
    name: ...
    displayName: ...
    dbColumnName: ...
    type: string, integer, numeric(x, y), enum (single-select, multi-select), json?, array?, record?
    subtype: ... (JanusId, Iso8601DateTime)
    constraints: fk, unique, not-null
}

```
{

  "projectId": "f094dabe-19d5-4919-914d-b65b735197ed",

  "displayName": "Dummy Customer data",

  "slug": "customers",

  "schema": {

    "type": "object",

    "required": ["d5d58a12-0afe-4c74-8f02-16b560f5841e", "78acdd29-a315-42dc-918d-f16e30600f5a", "24c48ef1-e776-4778-adff-f81223fdecb3"],

    "properties": {

        "d5d58a12-0afe-4c74-8f02-16b560f5841e": {

            "type": "string",

            "internalName": "customerName",

            "displayName": "Customer Name",

            "columnName": "customer_name",

            "defaultValue": "geeth"

        },

        "78acdd29-a315-42dc-918d-f16e30600f5a": {

            "type": "string",

            "displayName":"Customer Phone no.",

            "internalName": "phoneNumber",

            "columnName": "phone_number",

            "unique": true,

            "required": true

        },

        "24c48ef1-e776-4778-adff-f81223fdecb3": {

            "type": "uuid",

            "internalName": "leadId",

            "displayName": "Lead Id",

            "columnName": "lead_id",

            "foreignKey": {

                "targetCollectionId": "ea6c88c9-1997-4b1d-9d34-ecd16b4b2dda",

                "targetColumnName": "id",

                "targetColumnId": "b95fa828-1afa-447f-b7ab-5e1ef6efd8e1",

                "onDelete": "CASCADE",

                "onUpdate": "CASCADE"

            }

        }

    },

    "additionalProperties": false

  }

}
```


```
{

    "projectId": "f094dabe-19d5-4919-914d-b65b735197ed",

    "displayName": "User Post Data",

    "slug": "posts",

    "schema": {

        "type": "object",

        "required": [

            "430dadcb-d2d6-4540-b394-53f1dc90ef40",

            "3a13e2ea-832e-40b2-b730-9d36e22807b8",

            "1ec4b588-7ec9-4e60-8d83-8a4b5d12b1e4"

        ],

        "properties": {

            "430dadcb-d2d6-4540-b394-53f1dc90ef40": {

                "type": "string",

                "internalName": "postContent",

                "displayName": "Post Content",

                "columnName": "post_contetn",

                "defaultValue": "new content"

            },

            "3a13e2ea-832e-40b2-b730-9d36e22807b8": {

                "type": "number",

                "displayName": "Likes Count",

                "internalName": "likesCount",

                "columnName": "likes_count",

                "required": true

            },

            "1ec4b588-7ec9-4e60-8d83-8a4b5d12b1e4": {

                "type": "uuid",

                "internalName": "userId",

                "displayName": "User Id",

                "columnName": "user_id",

                "foreignKey":{

                    "targetCollectionId": "f73a0c71-c1b3-4f44-a5e5-d541e4eb71ae",

                    "targetColumnName": "id",

                    "targetColumnId": "e34ff736-83c9-44b4-9647-d814e039f388",

                    "onDelete": "CASCADE"

                }

            }

        },

        "additionalProperties": false

    }

}
```