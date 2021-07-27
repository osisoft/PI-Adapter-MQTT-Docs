---
uid: PIAdapterForMQTTPrinciplesOfOperation
---

# Principles of operation

This adapter's operations focus on data collection and stream creation.

## Adapter configuration

To start data collection for the MQTT adapter, configure the following:

- Data source: Provide the data source from which the adapter should collect data.<br> For more details, see the data source topics [PI Adapter for MQTT data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration) and [PI Adapter for MQTT Sparkplug B data source configuration](xref:PIAdapterForMQTTSparkplugBDataSourceConfiguration).
- Data selection: Select MQTT items to which the adapter should subscribe for data. <br> For more details, see the data selection topics [PI Adapter for MQTT data selection configuration](xref:PIAdapterForMQTTDataSelectionConfiguration) and [PI Adapter for MQTT Sparkplug B data selection configuration](xref:PIAdapterForMQTTSparkplugBDataSelectionConfiguration).
- Logging: Set up the logging attributes to manage the adapter logging behavior.

## Connection

The adapter uses either the Transmission Control Protocol (TCP) or WebSocket protocol (WS) to speak to an MQTT server. For more information on how to configure which protocol should be used, see [PI Adapter for MQTT data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration) and [PI Adapter for MQTT Sparkplug B data source configuration](xref:PIAdapterForMQTTSparkplugBDataSourceConfiguration).

## Data collection

The adapter collects time-series data from the MQTT server using Topics. The generic MQTT adapter only supports data from devices producing JSON payload, the MQTT Sparkplug B adapter also supports data from devices adhering to the Sparkplug B specification. For more information see [PI Adapter for MQTT data selection configuration](xref:PIAdapterForMQTTDataSelectionConfiguration) and [PI Adapter for MQTT Sparkplug B data selection configuration](xref:PIAdapterForMQTTSparkplugBDataSelectionConfiguration).

### Data types

#### MQTT data types

The following table lists MQTT variable types that the adapter collects data from and the types of streams that will be created.

| MQTT data type | Stream data type |
|------------------|------------------|
| Byte             | Int16            |
| SByte            | Int16            |
| Int16            | Int16            |
| Int32            | Int32            |
| Int64            | Int64            |
| UInt16           | UInt16           |
| UInt32           | UInt32           |
| UInt64           | UInt64           |
| Float            | Float32          |
| Double           | Float64          |
| Boolean          | Boolean          |
| String           | String           |
| DateTime         | DateTime         |

#### MQTT Sparkplug B data types

The following table lists MQTT Sparkplug B variable types that the adapter collects data from and the types of streams that will be created.

| MQTT Sparkplug B data type | Stream data type |
|----------------------------|------------------|
| Int8                       | Int8             |
| Int16                      | Int16            |
| Int32                      | Int32            |
| Int64                      | Int64            |
| UInt8                      | UInt8            |
| UInt16                     | UInt16           |
| UInt32                     | UInt32           |
| UInt64                     | UInt64           |
| Float                      | Float32          |
| Double                     | Float64          |
| Boolean                    | Boolean          |
| String                     | String           |
| DateTime                   | UInt64           |
| Text                       | String           |

## Stream creation

The MQTT adapter creates a stream with two properties for each selected MQTT item. The properties are described in the following table.

| Property name | Data type | Description |
|---------------|-----------|-------------|
| `Timestamp`   | String    | The response time of the stream data from the MQTT device |
| `Value`       | Specified by the data selection | The value of the stream data from the MQTT device |

Certain metadata are sent with each stream created. The following metadata are common for every adapter type:

- **ComponentId**: Specifies the data source, for example, _MQTT1_
- **ComponentType**: Specifies the type of adapter, for example, _MQTT_

### MQTT specific metadata

Metadata specific to the MQTT generic component are:

- **Topic**: Specifies the MQTT topic string
- **ValueField**: Specifies the JsonPath expression used to extract the data value from a property within the payload supplied by the MQTT server

**Note:** A configured metadata level allows you to set the amount of metadata for the adapter. Specify the metadata level in [General configuration](xref:GeneralConfiguration). For the MQTT adapter, the following metadata are sent for the individual level:

- `None`: No metadata
- `Low`: AdapterType (ComponentType) and DataSource (ComponentId)
- `Medium`: AdapterType (ComponentType), DataSource (ComponentId), Topic, and ValueField

Each stream created for the selected measurement has a unique identifier (stream Id). If you specify a custom stream Id for the measurement in the data selection configuration, the adapter uses that stream Id to create the stream. Otherwise, the adapter constructs the stream Id using the following format:

```code
<AdapterComponentId>.<Topic>.<ValueField>
```

**Note:** `StreamIdPrefix` and `DefaultStreamIdPattern` settings in the data source configuration affect the naming convention. For more information, see [PI Adapter for MQTT data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration).

### MQTT Sparkplug B specific metadata

Metadata specific to the MQTT Sparkplug B component are:

- **Namespace**: Specifies the topic namespace
- **Group_Id**: Specifies the element of the topic namespace that logically groups MQTT EoN nodes into the MQTT server and the consuming MQTT clients
- **Edge_Node_Id**: Specifies the element of the topic namespace that uniquely identifies the MQTT EoN node within the infrastructure
- **Device_Id**: Specifies the element of the topic namespace that identifies a device attached to the MQTT EoN node

**Note:** A configured metadata level allows you to set the amount of metadata for the adapter. Specify the metadata level in [General configuration](xref:GeneralConfiguration). For the MQTT adapter, the following metadata are sent for the individual level:

- `None`: No metadata
- `Low`: AdapterType (ComponentType) and DataSource (ComponentId)
- `Medium`: AdapterType (ComponentType), DataSource (ComponentId), Topic, and MetricName

Each stream created for the selected measurement has a unique identifier (stream Id). If you specify a custom stream Id for the measurement in the data selection configuration, the adapter uses that stream Id to create the stream. Otherwise, the adapter constructs the stream Id using the following format:

```code
<AdapterComponentId>.<Topic>.<MetricName>
```

**Note:** `StreamIdPrefix` and `DefaultStreamIdPattern` settings in the data source configuration affect the naming convention. For more information, see [PI Adapter for MQTT Sparkplug B data source configuration](xref:PIAdapterForMQTTSparkplugBDataSourceConfiguration).
