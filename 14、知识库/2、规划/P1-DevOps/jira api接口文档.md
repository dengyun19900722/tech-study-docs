# 更新issue

#### [Edit issue](https://docs.atlassian.com/software/jira/docs/api/REST/7.2.8/#api/2/issue-editIssue)`PUT /rest/api/2/issue/{issueIdOrKey}`

Edits an issue from a JSON representation.

The issue can either be updated by setting explicit the field value(s) or by using an operation to change the field value.

The fields that can be updated, in either the fields parameter or the update parameter, can be determined using the **/rest/api/2/issue/{issueIdOrKey}/editmeta** resource.
If a field is not configured to appear on the edit screen, then it will not be in the editmeta, and a field validation error will occur if it is submitted.

Specifying a "field_id": field_value in the "fields" is a shorthand for a "set" operation in the "update" section.
Field should appear either in "fields" or "update", not in both.

##### Request

###### QUERY PARAMETERS

| parameter     | type                     | description                                                  |
| :------------ | :----------------------- | :----------------------------------------------------------- |
| `notifyUsers` | *boolean*Default: `true` | send the email with notification that the issue was updated to users that watch it. Admin or project admin permissions are required to disable the notification. |

**EXAMPLE:**

```json

{
  "update": {
    "summary": [
      {
        "set": "Bug in business logic"
      }
    ],
    "components": [
      {
        "set": ""
      }
    ],
    "timetracking": [
      {
        "edit": {
          "originalEstimate": "1w 1d",
          "remainingEstimate": "4d"
        }
      }
    ],
    "labels": [
      {
        "add": "triaged"
      },
      {
        "remove": "blocker"
      }
    ]
  },
  "fields": {
    "summary": "This is a shorthand for a set operation on the summary field",
    "customfield_10010": 1,
    "customfield_10000": "This is a shorthand for a set operation on a text custom field"
  },
  "historyMetadata": {
    "type": "myplugin:type",
    "description": "text description",
    "descriptionKey": "plugin.changereason.i18.key",
    "activityDescription": "text description",
    "activityDescriptionKey": "plugin.activity.i18.key",
    "actor": {
      "id": "tony",
      "displayName": "Tony",
      "type": "mysystem-user",
      "avatarUrl": "http://mysystem/avatar/tony.jpg",
      "url": "http://mysystem/users/tony"
    },
    "generator": {
      "id": "mysystem-1",
      "type": "mysystem-application"
    },
    "cause": {
      "id": "myevent",
      "type": "mysystem-event"
    },
    "extraData": {
      "keyvalue": "extra data",
      "goes": "here"
    }
  },
  "properties": [
    {
      "key": "key1",
      "value": 'properties': 'can be set at issue create or update time'
    },
    {
      "key": "key2",
      "value": 'and': 'there can be multiple properties'
    }
  ]
}
```

**SCHEMA**

```json
{
    "id": "https://docs.atlassian.com/jira/REST/schema/issue-update#",
    "title": "Issue Update",
    "type": "object",
    "properties": {
        "transition": {
            "title": "Transition",
            "type": "object",
            "properties": {
                "id": {
                    "type": "string"
                },
                "name": {
                    "type": "string"
                },
                "to": {
                    "title": "Status",
                    "type": "object",
                    "properties": {
                        "statusColor": {
                            "type": "string"
                        },
                        "description": {
                            "type": "string"
                        },
                        "iconUrl": {
                            "type": "string"
                        },
                        "name": {
                            "type": "string"
                        },
                        "id": {
                            "type": "string"
                        },
                        "statusCategory": {
                            "title": "Status Category",
                            "type": "object",
                            "properties": {
                                "id": {
                                    "type": "integer"
                                },
                                "key": {
                                    "type": "string"
                                },
                                "colorName": {
                                    "type": "string"
                                },
                                "name": {
                                    "type": "string"
                                }
                            },
                            "additionalProperties": false
                        }
                    },
                    "additionalProperties": false
                },
                "fields": {
                    "type": "object",
                    "patternProperties": {
                        ".+": {
                            "title": "Field Meta",
                            "type": "object",
                            "properties": {
                                "required": {
                                    "type": "boolean"
                                },
                                "schema": {
                                    "title": "Json Type",
                                    "type": "object",
                                    "properties": {
                                        "type": {
                                            "type": "string"
                                        },
                                        "items": {
                                            "type": "string"
                                        },
                                        "system": {
                                            "type": "string"
                                        },
                                        "custom": {
                                            "type": "string"
                                        },
                                        "customId": {
                                            "type": "integer"
                                        }
                                    },
                                    "additionalProperties": false
                                },
                                "name": {
                                    "type": "string"
                                },
                                "autoCompleteUrl": {
                                    "type": "string"
                                },
                                "hasDefaultValue": {
                                    "type": "boolean"
                                },
                                "operations": {
                                    "type": "array",
                                    "items": {
                                        "type": "string"
                                    }
                                },
                                "allowedValues": {
                                    "type": "array",
                                    "items": {}
                                }
                            },
                            "additionalProperties": false,
                            "required": [
                                "required"
                            ]
                        }
                    },
                    "additionalProperties": false
                }
            },
            "additionalProperties": false
        },
        "fields": {
            "type": "object",
            "patternProperties": {
                ".+": {}
            },
            "additionalProperties": false
        },
        "update": {
            "type": "object",
            "patternProperties": {
                ".+": {
                    "type": "array",
                    "items": {
                        "title": "Field Operation",
                        "type": "object"
                    }
                }
            },
            "additionalProperties": false
        },
        "historyMetadata": {
            "title": "History Metadata",
            "type": "object",
            "properties": {
                "type": {
                    "type": "string"
                },
                "description": {
                    "type": "string"
                },
                "descriptionKey": {
                    "type": "string"
                },
                "activityDescription": {
                    "type": "string"
                },
                "activityDescriptionKey": {
                    "type": "string"
                },
                "emailDescription": {
                    "type": "string"
                },
                "emailDescriptionKey": {
                    "type": "string"
                },
                "actor": {
                    "$ref": "#/definitions/history-metadata-participant"
                },
                "generator": {
                    "$ref": "#/definitions/history-metadata-participant"
                },
                "cause": {
                    "$ref": "#/definitions/history-metadata-participant"
                },
                "extraData": {
                    "type": "object",
                    "patternProperties": {
                        ".+": {
                            "type": "string"
                        }
                    },
                    "additionalProperties": false
                }
            },
            "additionalProperties": false
        },
        "properties": {
            "type": "array",
            "items": {
                "title": "Entity Property",
                "type": "object",
                "properties": {
                    "key": {
                        "type": "string"
                    },
                    "value": {}
                },
                "additionalProperties": false
            }
        }
    },
    "definitions": {
        "history-metadata-participant": {
            "title": "History Metadata Participant",
            "type": "object",
            "properties": {
                "id": {
                    "type": "string"
                },
                "displayName": {
                    "type": "string"
                },
                "displayNameKey": {
                    "type": "string"
                },
                "type": {
                    "type": "string"
                },
                "avatarUrl": {
                    "type": "string"
                },
                "url": {
                    "type": "string"
                }
            },
            "additionalProperties": false
        }
    },
    "additionalProperties": false
}
```

