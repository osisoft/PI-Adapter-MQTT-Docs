---
uid: TroubleshootPIAdapterForMQTT
---

# Troubleshoot PI Adapter for MQTT

PI Adapter for MQTT provides troubleshooting features that enable you to verify adapter configuration, confirm connectivity, and view message logs. If you are unable to resolve issues with the adapter or need additional guidance, contact OSIsoft Technical Support through the [OSIsoft Customer Portal](https://my.osisoft.com/).

## Check configurations

Incorrect configurations can interrupt data flow and cause errors in values and ranges. Perform the following steps to confirm correct configuration for your adapter.

1. Navigate to [data source configuration](xref:PIAdapterForMQTTDataSourceConfiguration) and verify the following for each configured data source item below:

    * **HostNameOrIpAddress** - The correct host name or IP address of the MQTT server is referenced. A non-existent or incorrect **HostNameOrIpAddress** causes the adapter to not find the MQTT server.
    * **Port** - The correct port number is referenced. With an incorrect port number specified, the adapter cannot communicate to the MQTT server.
    * **Protocol** - The correct protocol is referenced. With an incorrect **Protocol** specified, the adapter cannot communicate to the MQTT broker.
    * **TLS** - The supported **TLS** (Transport Layer Security) is used.
    * **MQTTVersion** - The correct **MQTTVersion** is referenced.
    * **ValidateServerCertificate** - If enabled (and TLS is enabled), the adapter validates the server certificate. When the server certificate is not trusted, you can either add it to the OS certificate store or set **ValidateServerCertificate** to false.

1. Navigate to [data selection configuration](xref:PIAdapterForMQTTDataSelectionConfiguration) and verify that the following data selection items are correct:

    For the MQTT Sparkplug B component:
    * **Topic** - The topic string is valid. An incorrect topic string causes the adapter to not receive any values for this data selection item.
    * **MetricName** - The metric name string is valid. With an incorrect metric name, the adapter cannot extract a data value from the MQTT server payload.

    For the generic MQTT component:
    * **ValueField** - The JsonPath expression is valid. With an invalid JsonPath expression, the adapter cannot extract a data value from the MQTT server payload.
    * **TimeField** - The JsonPath expression is valid. With an invalid JsonPath expression, the adapter cannot extract a timestamp from the MQTT server payload.
    * **DataType** - The correct data type is referenced. An incorrect data type causes data conversion to fail.
    * **TimeFormat** - The correct time format is referenced. A time format that does not match the value from **TimeField** means that the adapter cannot convert timestamp from the MQTT server payload.

3. Navigate to [egress endpoints configuration](xref:EgressEndpointsConfiguration). For each configured endpoint, verify that the **Endpoint** and authentication properties are correct.

    * For a PI server or EDS endpoint, verify **UserName** and **Password**.
    * For an OCS endpoint, verify **ClientId** and **ClientSecret**.

## Check connectivity

Perform the following steps to verify active connections to the data source and egress endpoints.

1. Start PI Web API and verify that the PI point values are updating or start OCS and verify that the stream values are updating.
2. If configured, use a health endpoint to determine the status of the adapter.<br>For more information, see [Health and diagnostics](xref:HealthAndDiagnostics).

## Check logs

Perform the following steps to view the adapter and endpoint logs to isolate issues for resolution.

1. Navigate to the logs directory:<br>
    Windows: `%ProgramData%\OSIsoft\Adapters\MQTT\Logs`<br>
    Linux: `/usr/share/OSIsoft/Adapters/MQTT/Logs`.
2. Optional: Change the log level of the adapter to receive more information and context.<br>For more information, see [Logging configuration](xref:LoggingConfiguration).
