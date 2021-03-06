# MQTT v3 recommendation

Before [[[MQTT5]]] there were no extensibility metadata defined for the messages. Thus
this section is only a set of recommendations on how trace context can be
implemented, not the specification.

## JSON payload

It is common that MQTT message payloads are represented as a JSON string. In this case
it is recommended to set `traceparent` and `tracestate` fields as a fields on a
outermost JSON object.

For example, if original payload looks like following:

``` json
{
    "_type": "cmd",
    "action": "setConfiguration",
    "configuration": {
        "_type":"configuration",
        "settingA": "value A",
        "settingB": 42
        }
}
```

Payload with trace context configured will look like following:

``` json
{
    "_type": "cmd",
    "action": "setConfiguration",
    "configuration": {
        "_type":"configuration",
        "settingA": "value A",
        "settingB": 42
        },
    "traceparent": "00-0af7651916cd43dd8448eb211c80319c-b7ad6b7169203331-01",
    "tracestate": "congo=BleGNlZWRzIHRohbCBwbGVhc3VyZS4"
}
```

## Handling end-to-end encryption

TBD: Should there be a note that for e2e encryption one may decide to send trace
context un-encrypted while the rest of the message can be encrypted?

## Binary payload

For binary payload - [Trace Context: binary protocol](https://w3c.github.io/trace-context-binary/) may be used to
propagate trace context fields.
