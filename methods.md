# Methods

There are methods for storing the commission into a reservation and for calculating the commission.

{% method %}
## /store

This method calculates the commission for a given reservation and stores it into it.

{% sample lang="json" %}
Request:

```json
{
  "Locator": "<locator>",
  "PCC" : "<pcc>"
}
```

{% common %}
You'll also need to send your authentication token in HTTP headers with your request:

```bash
  Authorization: <token>
```
**Successful response**:

```json
{
    "Commission": {
        "Type": "<Commission type>",
        "Value": "<Commission value>"
    }
}
```

* _Commission type_ - one of: Flat, PercentBase, None
* _Commission value_ - integer of float as a string or "No commission available." string

**Error response**:

```json
{
  "Error": {
      "Code": "<Error type>",
      "Detail": "<Error text>"
    }
}
```

* _Error type_ - one of: Application, Retrieve, Store
* _Error text_ - string

{% endmethod %}

{% method %}
## /read

This method calculates the commission for a given reservation and returns it.

Request and Response are the same as with /store .

{% endmethod %}


