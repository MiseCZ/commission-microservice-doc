# Methods

There are several resources for commission and commission rules handling. You can use them:

* for storing the commission into a reservation
* for calculating the commission
* for management of commission rules

## Authorization

Accessing any resource is possible only with http header:

```
Authorization: <AuthorizationToken>
```

{% method %}
## Storing Commission

```
POST /reservation/v1/<Pcc>/<Locator>
```

This method calculates the commission for a given reservation and stores it into it. Body of 

| Param  | Type   | Description 
|--      |--
|Pcc     | String | Agency's PCC
|Locator | String | Locator of a reservation.

**Successful response**:

```json
{
    "Commission": {
        "Type": "<CommissionType>",
        "Value": "<CommissionValue>"
    }
}
```

|Param|Type|Description 
|-- |--
|CommissionType  | Enum | One of: Flat, PercentBase, None
|CommissionValue | String | Integer or float as a string or "No commission available." constant.

**Error response**:

```json
{
  "Error": {
      "Code": "<ErrorCode>",
      "Detail": "<ErrorText>"
    }
}
```

|Param|Type|Description 
|-- |--
|ErrorCode  | Enum | Code. It starts with category (Application, Retrieve, Store or [GolApi](http://doc.golibe.com/golapi/wiki/gol_api_xml_errors "GolApi").) 
|ErrorValue | String | Text.

{% endmethod %}


{% method %}
## Calculation

```
GET /calculation/v1/<Pcc>/<Locator>
```

This method calculates the commission for a given reservation and returns it.

Request params response and error format are the same as with `POST /reservation/v1/<Pcc>/<Locator>`.

{% endmethod %}


{% method %}
## Commission rule creation

```
POST /rules/v1/<Pcc>
```

This method creates a new rule for the commission calculation.

**Request body**:

```json
{
    "ValidatingAirline": "<ValidatingAirlineCode>",
    "Priority": "<Priority>",
    "MarketingAirlineCombination": {
        "Type": "<MarketingAirlineCombinationType>"
    },
    "OperatingAirlineCombination": {
        "Type": "<OperatingAirlineCombinationType>",
        "OperatingAirline": [
          {
            "Code": "<OperatingAirlineCode>"
          }
        ]
    },
    "SelectedOrigin": {
        "Type": "Iata",
        "Value": "NYC"
    },
    "SelectedDestination": {
        "Type": "Country",
        "Value": "AL"
    },
    "BookingClasses": {
        "BookingClass": [
            {
                "Code": "Y"
            },
            {
                "Code": "S"
            },
            {
                "Code": "R"
            }
        ]
    },
    "Commission": {
        "Type": "Flat",
        "Value": "17"
    }
}
```

|Param|Type|Description 
|-- |--
|ValidatingAirlineCode           | String  | Code of validating airline 
|Priority                        | Integer | Priority of a rule. 
|MarketingAirlineCombinationType | Enum    | One of: AnyCombination, VariousAirlines, ValidatingAirlineOnly, ValidatingAirlineMissing
|OperatingAirlineCombinationType | Enum    | One of: AnyCombination, OperatedByListed, NotOperatedByListed
|OperatingAirlineCode            | String  | Code of operating airline



**Successful response**:

```json
{
    "CommissionRuleId": "<CommissionRuleId>",
    "CommissionRule": {
        // Same as in request
    }
}
```

|Param|Type|Description 
|-- |--
|CommissionType  | String | One of: Flat, PercentBase, None |
|CommissionValue | String | Integer or float as a string or "No commission available." constant. |

**Error response**:

Error format is the same as with `POST /reservation/v1/<Pcc>/<Locator>`.

{% endmethod %}


{% method %}
## Commission rule modification

```
POST /rules/v1/<Pcc>/<CommissionRuleId>
```

This method modifies the specified commission rule.

Response and error format are the same as with `POST /rules/v1/<Pcc>`.

{% endmethod %}

{% method %}
## Commission rule details

```
GET /rules/v1/<Pcc>/<CommissionRuleId>
```

This method returns details the specified commission rule.

Response and error format are the same as with `POST /rules/v1/<Pcc>`.

{% endmethod %}


{% method %}
## Commission rule canceling

```
DELETE /rules/v1/<Pcc>/<CommissionRuleId>
```

This method cancels the specified commission rule.

**Successful response**:

```json
{
    "CanceledCommissionRule": {
        "CommissionRuleId": "2028"
    }
}
```

Error format is the same as with `POST /rules/v1/<Pcc>`.

{% endmethod %}


{% method %}
## Commission rule listing

```
GET /rules/v1/<Pcc>
```

This method returns defined commission rules.

**Successful response**:

```json
[
    {
        "CommissionRuleId": "2028",
        "CommissionRule": {
          // CommissionRule object
        }
    },
    {
        "CommissionRuleId": "2059",
        "CommissionRule": {
          // CommissionRule object
    }
]
```

Error format is the same as with `POST /rules/v1/<Pcc>`.

{% endmethod %}




## Testing

Testing with curl:

```
# List existing commission rules
curl -X GET https://cm.cee-systems.com/prod/rules/v1/<Pcc> -H "Authorization: <AuthorizationToken>"

# View one existing commission rule
curl -X GET https://cm.cee-systems.com/prod/rules/v1/<Pcc>/<CommissionRuleId> -H "Authorization: <AuthorizationToken>"

```




