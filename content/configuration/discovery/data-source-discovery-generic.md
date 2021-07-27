---
uid: GenericDataSourceDiscovery
---

# Data source discovery (generic)

A discovery against the data source of a generic MQTT adapter allows you to specify the optional **query** parameter. The query discovers the contents of the data source and narrows down the scope of the discovery. You can add the discovered items to the data selection.

**Note:** Only one discovery at a time is supported.

## Generic query string

The string of the **query** parameter must contain string items in the following form: <br>`Topics=<TopicName>;WaitTime=<WaitTime`<br><br>

| String item      | Required | Description |
|------------------|----------|-------------|
| **Topics**       | Optional |  The topics that the adapter will subscribe to when the discovery is posted.<br>**Note:** If you want to specify multiple topics in the query, you must separate the topic names with a comma. If you do not specify a topic, the adapter will subscribe to all topics. |
| **WaitTime**     | Optional |  The time that the adapter will wait to receive data for the specified topics. Once the wait time has passed, the discovery results will be available.<br><br>Minimum value: `0.00:00:30` (30 seconds)<br>Maximum value: `7.00:00:00` (7 days)<br>**Note:** If you do not specify a wait time, the default value `0.00:01:00` (1 minute) will be applied.          |

### Query rules

The following rules apply for specifying the query string:

- Only one query at a time is supported. That query can contain as many topics with the same wait time as needed. If you need different wait times for different topics, you need to create a new query once the query from the previous discovery has completed.
- Partial queries are terminated by a multi-level wildcard (`#`).
- A query cannot be terminated by a trailing slash (`/`).
- A query cannot start with a leading slash (`/`) or `$`.
- Topics are case sensitive.
- White spaces are supported.

**Note:** The data source might contain tens of thousands of metrics. Use the `#` judiciously and narrow down the query string to something specific or break down the query into different discoveries.

#### Wildcards

Wildcards are allowed in the query with the following specifications:

- A single-level wildcard replaces one topic level and is indicated by `+`.
- A multi-level wildcard covers many topic levels and is indicated by `#`.
- Wildcards can be combined.
- `#` must not be used more than once and can only be used at the end of the topic.
- No query, an empty string, or `null` as the query parameter is equivalent to `#`.

## Discovery query example

The query parameter of the generic MQTT component must be specified in the following form:
`Topics=<TopicName>;WaitTime=<WaitTime`.

### Generic data source discovery initiation

```json
{
	"id" : "SampleA",
	"query" : "Topics=Sample1,Sample2;WaitTime=00:00:30"
}
```

### Generic data source discovery results

```json
[
    {
	    "id" : "SampleA",
	    "query" : "Topics=Sample1,Sample2;WaitTime=00:00:00:30",
	    "startTime": "2020-12-14T14:19:01.4383791-08:00",
	    "endTime": "2020-12-14T14:19:31.8549164-08:00",
	    "progress": 30,
	    "itemsFound": 700,
	    "newItems": 200,
	    "resultUri": "http://127.0.0.1:5590/api/v1/Configuration/mqttComponentId/Discoveries/40/result",
	    "autoSelect": false,
	    "status": "Complete",
	    "errors": null
	}
]
```

### MQTT discovered selection items

```json
[
    {
        "Topic": "Sample1",
        "ValueField": "$.Value4",
        "TimeField": null,
        "DataType": "Single",
        "TimeFormat": "Adapter",
        "Selected": false,
        "Name": null,
        "StreamId": "Sample1.$.Value4",
        "DataFilterId": null
    },
    {
        "Topic": "Sample2",
        "ValueField": "$.Value1",
        "TimeField": null,
        "DataType": "Single",
        "TimeFormat": "Adapter",
        "Selected": false,
        "Name": null,
        "StreamId": "Sample1.$.Value1",
        "DataFilterId": null
    }
]
```