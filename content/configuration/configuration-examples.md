---
uid: PIAdapterForMQTTConfigurationExamples
---

# Configuration examples

The following tables provide examples for all configurations available for PI Adapter for MQTT.

**Note:** The examples in this topic are using the default port number `5590`. If you selected a different port number, replace it with that value.TEST

## System components configuration with two MQTT adapter instances

```json
[
    {
        "ComponentId": "Mqtt1",
        "ComponentType": "MQTT"
    },
    {
        "ComponentId": "Mqtt2",
        "ComponentType": "MQTT"
    },
    {
        "ComponentId": "OmfEgress",
        "ComponentType": "OmfEgress"
     }
]
```

## MQTT adapter configuration

```json
{
    "Mqtt1": {
        "Logging": {
            "logLevel": "Information",
            "logFileSizeLimitBytes": 34636833,
            "logFileCountLimit": 31
        },
        "DataSource": {
            "StreamIdPrefix" : null,
            "DefaultStreamIdPattern" : "{Topic}.{ValueField}",
            "HostNameOrIpAddress" : "125.0.0.0",
            "Port" : 8765,
            "Protocol" : "Tcp",
            "TLS" : "Tls12",
            "UserName": null,
            "Password": null,
            "ClientId" : "Test-Cliendt-Id",
            "MQTTVersion" : "3.1.1",
            "ValidateServerCertificate" : true
        },
        "DataSelection": [
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
    },
    "System": {
        "Logging": {
            "logLevel": "Information",
            "logFileSizeLimitBytes": 34636833,
            "logFileCountLimit": 31
        },
        "HealthEndpoints": [
        ],
    "Diagnostics": {
            "enableDiagnostics": true
    },
        "Components": [
            {
                "componentId": "Egress",
                "componentType": "OmfEgress"
            },
            {
                "componentId": "Mqtt1",
                "componentType": "MQTT"
            }
        ],
    "Buffering": {
            "BufferLocation": "C:/ProgramData/OSIsoft/Adapters/MQTT/Buffers",
            "MaxBufferSizeMB": -1,
            "EnableBuffering": true
        }
     },
    "OmfEgress": {
        "Logging": {
            "logLevel": "Information",
            "logFileSizeLimitBytes": 34636833,
            "logFileCountLimit": 31
        },
        "DataEndpoints": [
            {
                "id": "WebAPI EndPoint",
                "endpoint": "https://PIWEBAPIServer/piwebapi/omf",
                "userName": "USERNAME",
                "password": "PASSWORD"
            },
            {
                "id": "OCS Endpoint",
                "endpoint": "https://OCSEndpoint/omf",
                "clientId": "CLIENTID",
                "clientSecret": "CLIENTSECRET"
            },
            {
                "Id": "EDS",
                "Endpoint": "http://localhost:/api/v1/tenants/default/namespaces/default/omf",
                "UserName": "eds",
                "Password": "eds"
            }
        ]
    }
}
```

## Data source configuration

The following are representations of minimal and complete data source configurations of MQTT adapter.

### MQTT (generic) - Minimal data source configuration

```json
{
    "HostNameOrIpAddress" : "125.0.0.0" ,
    "Port" : 8765
}
```

### MQTT (generic) - Complete data source configuration

```json
{
    "StreamIdPrefix" : null,
    "DefaultStreamIdPattern" : "{Topic}.{ValueField}",
    "HostNameOrIpAddress" : "125.0.0.0",
    "Port" : 8765,
    "Protocol" : "Tcp",
    "TLS" : "Tls12",
    "UserName": null,
    "Password": null,
    "ClientId" : "Test-Cliendt-Id",
    "MQTTVersion" : "3.1.1",
    "ValidateServerCertificate" : true
}
```

### MQTT Sparkplug B - Minimal data source configuration

```json
{
    "HostNameOrIpAddress" : "Host1",
    "Port" : 4321,
}
```

### MQTT Sparkplug B - Complete  data source configuration

```json
{
    "HostNameOrIpAddress" : "Host1",
    "Port" : 4321,
    "PrimaryHostId" : "Optimus",
    "Protocol" : "Tcp",
    "TLS" : "Tls12",
    "UserName" : "RandomUser",
    "PassWord" : "RandomPassword",
    "ClientId" : "Random-Client-Id",
    "MQTTVersion" : "3.1.1",
    "ValidateServerCertificate" : true,
    "StreamIdPrefix" : null,
    "DefaultStreamIdPattern" : "{PointId}"
}
```

## Client settings configuration

```json
{
    "KeepAlivePeriod" : true,
    "QosLevel" : 2,
    "MaxInternalQueueSize" : 350000
}
```

## Data selection configuration

The following are representations of minimal and complete data selection configurations of MQTT adapter.

### MQTT (generic) - Minimal data selection configuration

```json
[
 {
    "Topic" : "RandomTopic",
    "ValueField" : "$.TestNode[:1].Value",
    "DataType" : "uint64"
  }
]
```

### MQTT (generic) - Complete data selection configuration

```json
[
 {  "Selected" : true,
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

### MQTT Sparkplug B - Minimal data selection configuration

```json
[
  {
    "Topic" : "Home/Sensors",
    "PropertyName" : "Temperature"
  }
]
```

### MQTT Sparkplug B - Complete data selection configuration

```json
[
  {
    "Selected" : true,
    "Name" : "Temperature",
    "Topic" : "Home/Sensors",
    "PropertyName" : "temperature",
    "StreamId" : "RandomStreamId",
    "DataFilterId" : null
  }
]
```
