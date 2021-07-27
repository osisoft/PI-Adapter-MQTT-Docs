---
uid: PIAdapterForMQTTDataSelectionConfiguration
---

# Data selection (generic)

In addition to the data source configuration, you need to provide a data selection configuration to specify the data you want the adapter to collect from the data sources.

## Configure MQTT data selection

Complete the following steps to configure an MQTT data selection. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/DataSelection` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an MQTT data selection into the file.

    For sample JSON, see [MQTT data selection examples](#mqtt-data-selection-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [MQTT data selection parameters](#mqtt-data-selection-parameters).

4. Save the file. For example, as `ConfigureDataSelection.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataSelection.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data selection configuration.

    ```bash
    curl -d "@ConfigureDataSelection.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/MQTT1/DataSelection"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `MQTT1`, update the endpoint with your chosen component ID.
    * For a list of other REST operations you can perform, like updating or deleting a data selection configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

## MQTT data selection schema

The full schema definition for the MQTT data selection configuration is in the `MQTT_DataSelection_schema.json` file located in one of the following folders:

* Windows: `%ProgramFiles%\OSIsoft\Adapters\MQTT\Schemas`
* Linux: `/opt/OSIsoft/Adapters/MQTT/Schemas`

## MQTT data selection parameters

| Parameter        | Required | Type      | Description |
|------------------|----------|-----------|-------------|
| **Selected**     | Optional | `boolean` |  Selects or clears a measurement. To select an item, set the field to `true`. To remove an item, leave the field empty or set the value to `false`.  <br><br>Allowed value: `true` or `false`<br>Default value: `true`           |
| **Name**         | Optional | `string`  | The optional friendly name of the data item collected from the data source <br><br>Allowed value: Any string<br>Default value: `null`            |
| **StreamId**     | Optional | `string`  |  The custom stream Id that is used to create the streams. If you do not specify the StreamId, the adapter generates a default stream Id based on the DataSource parameter. For more information, see [PI Adapter for MQTT data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration#mqtt-data-source-parameters). A properly configured custom stream Id follows these rules:<br><br>Is not case-sensitive<br>Can contain spaces<br>Can contain front slashes (`/`)<br>Can contain a maximum of 100 characters<br>Cannot start with two underscores (`__`)<br>Cannot use the following characters:<br> `:` `?` `#` `[` `]` `@` `!` `$` `&` `'` `(` `)` `\` `*` `+` `,` `;` `=` `%` `<` `>` or the vertical bar<br>Cannot start or end with a period<br>Cannot contain consecutive periods<br>Cannot consist of only periods<br><br>The default Id automatically updates when there are changes to the measurement and follows the format of `<Topic>.<MetricName>`.           |
| **Topic**        | Required | `string`  |  The MQTT topic string<br><br>Allowed value: Cannot be `null`, empty, or whitespace.        |
| **ValueField**   | Required | `string`  |  The JsonPath expression used to extract the data value from a property within the payload supplied by the MQTT server. A valid JsonPath expression starts with `$`.<br><br>Allowed value: Cannot be `null`, empty, or whitespace.          |
| **TimeField**    | Optional | `string`  | The JsonPath expression to take value to use as a timestamp from a property. A valid JsonPath expression starts with `$`. <br><br>**Note:** The adapter generates a timestamp when `null` is specified.<sup>1</sup><br><br>Allowed value: Any valid JsonPath expression      |
| **DataType**     | Required | `string`  |  The expected data type of the values for the specified field <br><br>Allowed value: See [PI Adapter for MQTT principles of operation](xref:PIAdapterForMQTTPrinciplesOfOperation#data-types)|
| **TimeFormat**   | Optional | `string`  | The time format of the timestamp value specified in the TimeField property<br><br>Allowed value: Any string that can be used as a DateTime format in the .NET `DateTime.TryParseExact()`method, for example `01/30/2021`.<br> For more information, see [DateTime.TryParseExact Method](https://docs.microsoft.com/en-us/dotnet/api/system.datetime.tryparseexact?view=net-5.0)<sup>1</sup><br><br>**Note:** If the string cannot be parsed, specify a custom DateTime string or one of the following keywords: `Adapter`, `UnixTimeSeconds`, `UnixTimeMilliseconds`<br>Default value: `null`            |
| **DataFilterId** | Optional  | `string`  | The Id of the data filter <br><br>Allowed value: Any string <br>Default value: `null`<br>**Note:** If the specified **DataFilterId** does not exist, unfiltered data is sent until that **DataFilterId** is created.            |

<sup>1</sup> If you do not specify **TimeField** and **TimeFormat**, the adapter automatically sets the latter to `Adapter`, which uses an adapter-supplied timestamp for the data. The timestamp is taken after the data is published to the adapter while the adapter processes it. If you specify **TimeFormat** only with a value other than `Adapter`, the validation fails and the adapter throws an error.

## Runtime changes

When you create a new data selection item with a new **Topic** property, the adapter automatically subscribes the topic to the data source and starts data collection for the stream associated with the data selection item.  When you delete a data selection item, the adapter automatically stops collecting data for that item. Additionally, when you update a data selection item, the adapter updates the stream and continues data collection.

**Note:** Runtime changes also handle validation of configuration, which means that an invalid data selection configuration is rejected.

## MQTT data selection examples

The following are examples of valid MQTT data selection configurations<sup>1</sup>:

### Minimal data selection configuration

```json
[
  {
    "Topic" : "RandomTopic",
    "ValueField" : "$.TestNode[:1].Value",
    "DataType" : "uint64",
  }
]
```

### Complete data selection configuration

```json
[
  {
    "Selected" : true,
    "Name" : null,
    "StreamId" : "RandomStreamId",
    "DataFilterId" : null,
    "Topic" : "RandomTopic",
    "ValueField" : "$.TestNode[:1].Value",
    "TimeField" : "$.TestNode[:1].Time",
    "DataType" : "uint64",
    "TimeFormat" : null

  }
]
```

<sup>1</sup> **Note:** Both **ValueField** and **TimeField** require the correct structure of the JSON payload to be specified, in other words, what the data source returns. The previous examples use the following Json payload structure:

```json
{
  "TestNode": [
    {
      "Time": "02/17/2021 12:01:36 AM PST",
      "Value": "4578"
    }
  ]
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/\<ComponentId\>/DataSelection  | `GET` | Retrieves the MQTT data selection configuration, including all data selection items. |
| api/v1/configuration/\<ComponentId\>/DataSelection  | `PUT` | Configures or updates the MQTT data selection configuration. The adapter starts collecting data for each data selection item when the following conditions are met:<br/><br/>&bull; The MQTT data selection configuration `PUT` request is received.<br/>&bull; A MQTT data source configuration is active. |
| api/v1/configuration/\<ComponentId\>/DataSelection | `DELETE` | Deletes the active MQTT data selection configuration. The adapter stops collecting data. |
| api/v1/configuration/\<ComponentId\>/DataSelection | `PATCH` | Allows partial updates of configured data selection items.<br/><br/>**Note:** The request must be an array containing one or more data selection items. Each item in the array must include its **StreamId**. |
| api/v1/configuration/\<ComponentId\>/DataSelection/\<StreamId\> | `PUT` | Updates or creates a new data selection item by **StreamId**. For new items, the adapter starts collecting data after the request is received. |
| api/v1/configuration/\<ComponentId\>/DataSelection/\<StreamId\> | `DELETE` | Deletes a data selection item from the configuration by **StreamId**. The adapter stops collecting data for the deleted item. |

**Note:** Replace _ComponentId_ with the Id of your MQTT component. For example, _Mqtt1_.
