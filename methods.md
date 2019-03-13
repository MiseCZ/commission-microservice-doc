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

## Storing Commission

```
POST /reservation/v1/<Pcc>/<Locator>
```

This method calculates the commission for a given reservation and stores it into it. Body of

| Param | Type | Description |
| :--- | :--- | :--- |
| Pcc | String | Agency's PCC |
| Locator | String | Locator of a reservation. |

**Successful response**:

```json
{
    "Reservation": {
        "AirPricingInfo": [{
            "Group": "<group>",
            "Origin": "<origin>",
            "Destination": "<destination>",
            "PlatingCarrier": "<platingCarrier>",
            "BasePrice": "<currency><amount>",
            "Passenger": [{
                "Name": "<fullName>",
                "Type": "<type>"
            }, ...],
            "Result": {
                "Commission": {
                    "Type": "<CommissionType>",
                    "Value": "<CommissionValue>"
                }
            }
        }, ... ]
    },
    // if commission was stored but remark into PNR wasn't written
    "Warning": {
        "Code": "Remark.<typ>",
        "Detail": "<Warning text>"
    }
}
```

| Param | Type | Description |
| :--- | :--- | :--- |
| CommissionType | Enum | One of: Flat, PercentBase, None |
| CommissionValue | String | Integer or float as a string or "No commission available." constant. |

**Error structure**:

```json
{
  "Error": {
      "Code": "<ErrorCode>",
      "Detail": "<ErrorText>"
    }
}
```

| Param | Type | Description |
| :--- | :--- | :--- |
| ErrorCode | Enum | Code. It starts with category \(Application, Retrieve, Store or [GolApi](http://doc.golibe.com/golapi/wiki/gol_api_xml_errors "GolApi").\) |
| ErrorValue | String | Text. |

**Examples of error responses**

Error processing a reservation:

```json
{
  "Error":
  {
    "Code": "Retrieve.NoPrice",
    "Detail" : "No price in reservation is viable for commission."
  }
}
```

Pricings from a reservation are processed separately, so error on pricing level is more structured:

```json
{
  "Reservation":
  {
    "AirPricingInfo":
    [
      {
        "Group": "1",
        "Origin": "PRG",
        "Destination": "LGW",
        "PlatingCarrier": "FI",
        "BasePrice": "CZK800",
        "Passenger":
        [
          {
            "Name": "VITMR SLANINKA",
            "Type": "ADT"
          }
        ],
        "Result":
        {
          "Error":
          {
            "Code": "Store.Failed",
            "Detail": "Store commission failed."
          }
        }
      }
    ]
  }
}
```

**Example of warning response**

```json
{
  "Reservation":
  {
    "AirPricingInfo":
    [
      {
        "Group": "1",
        "Origin": "PRG",
        "Destination": "LGW",
        "PlatingCarrier": "FI",
        "BasePrice": "CZK1000",
        "Passenger":
        [
          {
            "Name": "VITMR SLANINKA",
            "Type": "ADT"
          }
        ],
        "Result":
        {
          "Commission":
          {
            "Type": "PercentBase",
            "Value": "5"
          }
        }
      }
    ]
  },
  "Warning":
  {
    "Code": "Remark.Failed",
    "Detail": "Failed to add remark to reservation."
  }
}
```

## Calculation

```
GET /calculation/v1/<Pcc>/<Locator>
```

This method calculates the commission for a given reservation and returns it.

Request params response and error format are the same as with `POST /reservation/v1/<Pcc>/<Locator>`.

## Commission rule creation

```
POST /rules/v1/<Pcc>
```

This method creates a new rule for the commission calculation.

**Request body**:

```json
{
    "ApplicableForJourneys": {
        "OneWay": "false",
        "Return": "true"
    },
    "ValidatingAirline": "<ValidatingAirlineCode>",
    "Priority": "<Priority>",
    "UseAsCommission": "Y",
    "UseAsDiscount": "N",
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
    "CanBeOriginDestinationReversed": {
        "Value": "true"
    },
    "BookingClasses": {
        "Mode": "<BookingClassMode>",
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
    "CommissionRulePassengers": {
        "Mode": "<BookingClassMode>",
        "CommissionRulePassenger": [
            {
                "PassengerType": "ADT"
            },
            {
                "PassengerType": "CNN"
            }
        ]
    },
    "Commission": {
        "Type": "Flat",
        "Value": "17"
    },
    "SalesDateRange": {
        "DateFrom": "2018-05-01",
        "DateTo": "2018-05-31"
    },
    "OriginTravelRange": {
        "DateFrom": "2018-07-01",
        "DateTo": "2018-08-31"
    },
    "DestinationTravelRange": {
        "DateFrom": "",
        "DateFrom": "2018-12-31"
    }
}
```

| Param | Type | Description |
| :--- | :--- | :--- |
| ValidatingAirlineCode | String | Code of validating airline |
| Priority | Integer | Priority of a rule. |
| MarketingAirlineCombinationType | Enum | One of: AnyCombination, VariousAirlines, ValidatingAirlineOnly, ValidatingAirlineMissing |
| OperatingAirlineCombinationType | Enum | One of: AnyCombination, OperatedOnlyBySomeOfListed, OperatedByAllListed, NotOperatedByListed |
| OperatingAirlineCode | String | Code of operating airline |

**Successful response**:

```json
{
    "CommissionRuleId": "<CommissionRuleId>",
    "CommissionRule": {
        // Same as in request
    }
}
```

| Param | Type | Description |
| :--- | :--- | :--- |
| CommissionType | String | One of: Flat, PercentBase, None |
| CommissionValue | String | Integer or float as a string or "No commission available." constant. |

**Error response**:

Error format is the same as with `POST /reservation/v1/<Pcc>/<Locator>`.

## Commission rule modification

```
POST /rules/v1/<Pcc>/<CommissionRuleId>
```

This method modifies the specified commission rule.

Response and error format are the same as with `POST /rules/v1/<Pcc>`.

## Commission rule details

```
GET /rules/v1/<Pcc>/<CommissionRuleId>
```

This method returns details the specified commission rule.

Response and error format are the same as with `POST /rules/v1/<Pcc>`.

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

## Testing

Testing with curl:

```
# List existing commission rules
curl -X GET https://cm.cee-systems.com/prod/rules/v1/<Pcc> -H "Authorization: <AuthorizationToken>"

# View one existing commission rule
curl -X GET https://cm.cee-systems.com/prod/rules/v1/<Pcc>/<CommissionRuleId> -H "Authorization: <AuthorizationToken>"
```



