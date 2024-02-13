### OpenTelemetry for Envoy Access Logs
#### Enable Envoy Access Logs
Using the `openTelemetry-collector-logs.yaml` manifest will send Envoy Access Logs to Dynatrace OTLP endpoint.
#### Query (DQL) Envoy Access Logs
Envoy Access Logs can be queried using DQL.  Adjust your `dt.auth.origin` value to match your environment.
```
fetch logs
| filter isNull(log.source) AND dt.auth.origin == "dt0c01.OZF7HWB2UJO5KMFPMWFT5HDM"
| parse content, """LD SPACE DQS:REQUEST SPACE INTEGER:RESPONSE_CODE SPACE LD:RESPONSE_FLAG SPACE LD:RESPONSE_DETAILS SPACE LD:CONNECTION_TERMINATION_DETAILS SPACE DQS:UPSTREAM_TRANSPORT_FAILURE_REASON SPACE INTEGER:BYTES_RECEIVED SPACE INTEGER:BYTES_SENT SPACE INTEGER:DURATION SPACE LD:RESPONSE_UPSTREAM_TIME SPACE DQS:X_FORWARDED_FOR SPACE DQS:USER_AGENT SPACE DQS:X_REQUEST_ID SPACE DQS:AUTHORITY SPACE DQS:UPSTREAM_HOST SPACE LD:UPSTREAM_CLUSTER SPACE LD:UPSTREAM_LOCAL_ADDR SPACE LD:DOWNSTREAM_LOCAL_ADDR SPACE LD:DOWNSTREAM_REMOTE_ADDR SPACE LD:REQUESTED_SERVER_NAME SPACE LD:ROUTE_NAME EOL"""
```
#### Disable Envoy Access Logs
Using the `openTelemetry-collector-no-logs.yaml` manifest will prevent Envoy Access Logs from being sent to Dynatrace OTLP endpoint.  Making this change will not automatically recycle pods.  Delete existing pods to generate new pods with the new configuration.
```
kubectl delete pods -n default --field-selector="status.phase=Running"
```