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
Response:

```json
{
    "Commission": {
        "Type": "Flat",
        "Value": "50"
    }
}
```

{% endmethod %}

{% method %}
## /read

This method calculates the commission for a given reservation and returns it.

Request and Response are the same as with /store .

{% endmethod %}


