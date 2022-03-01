# kubernetes_operator_example
Working example of https://www.magalix.com/blog/creating-custom-kubernetes-operators

## Steps 

Follow the steps mentioned in https://www.magalix.com/blog/creating-custom-kubernetes-operators
* kind load docker-image <flask-app/controller-image> --name kind-cluster-name
* Service account, role and rolebinding created for controller to resolve
  ```
  2022-03-01 14:22:07,539 Starting the service
  2022-03-01 14:22:07,543 {'kind': 'Status', 'apiVersion': 'v1', 'metadata': {}, 'status': 'Failure', 'message': 'configmaps is forbidden: User       "system:serviceaccount:default:default" cannot watch resource "configmaps" in API group "" in the namespace "default"', 'reason': 'Forbidden', 'details': {'kind': 'configmaps'}, 'code': 403}
  Traceback (most recent call last):
  File "/main.py", line 87, in <module>
    event_loop()
  File "/main.py", line 74, in event_loop
    event_type = obj['type']
  KeyError: 'type'
  ```
  
  Logs from controller container
  ```
  2022-03-01 16:44:13,691 Starting the service
  2022-03-01 16:44:13,706 {'type': 'ADDED', 'object': {'kind': 'ConfigMap', 'apiVersion': 'v1', 'metadata': {'name': 'flaskapp-config', 'namespace': 'default', 'uid': '88839aec-14ba-4f5f-a459-42f2d030c0c0', 'resourceVersion': '511973', 'creationTimestamp': '2022-03-01T13:04:23Z', 'annotations': {'kubectl.kubernetes.io/last-applied-configuration': '{"apiVersion":"v1","data":{"config.cfg":"MSG=\\"Welcome to Kubernetes!\\""},"kind":"ConfigMap","metadata":{"annotations":{},"name":"flaskapp-config","namespace":"default"}}\n'}, 'managedFields': [{'manager': 'kubectl', 'operation': 'Update', 'apiVersion': 'v1', 'time': '2022-03-01T13:04:23Z', 'fieldsType': 'FieldsV1', 'fieldsV1': {'f:data': {'.': {}, 'f:config.cfg': {}}, 'f:metadata': {'f:annotations': {'.': {}, 'f:kubectl.kubernetes.io/last-applied-configuration': {}}}}}]}, 'data': {'config.cfg': 'MSG="Welcome to Operator!"'}}}
  2022-03-01 16:44:13,710 {'type': 'ADDED', 'object': {'kind': 'ConfigMap', 'apiVersion': 'v1', 'metadata': {'name': 'istio-ca-root-cert', 'namespace': 'default', 'uid': '7e65dcd4-ed31-4924-89d1-3359091ebd7b', 'resourceVersion': '1577', 'creationTimestamp': '2022-01-31T16:41:15Z', 'labels': {'istio.io/config': 'true'}, 'managedFields': [{'manager': 'pilot-discovery', 'operation': 'Update', 'apiVersion': 'v1', 'time': '2022-01-31T16:41:15Z', 'fieldsType': 'FieldsV1', 'fieldsV1': {'f:data': {'.': {}, 'f:root-cert.pem': {}}, 'f:metadata': {'f:labels': {'.': {}, 'f:istio.io/config': {}}}}}]}, 'data': {'root-cert.pem': '-----BEGIN CERTIFICATE-----\nMIIC/DCCAeSgAwIBAgIQFAMRadF/qA/gmCM63Dbd+TANBgkqhkiG9w0BAQsFADAY\nMRYwFAYDVQQKEw1jbHVzdGVyLmxvY2FsMB4XDTIyMDEzMTE2NDExNFoXDTMyMDEy\nOTE2NDExNFowGDEWMBQGA1UEChMNY2x1c3Rlci5sb2NhbDCCASIwDQYJKoZIhvcN\nAQEBBQADggEPADCCAQoCggEBAKokjpMYOSjA2jsS8u4s++IdyH7deC7ttQQ5DPcE\n/sPyet6Dis585J8tGK3Cq3AGePVQIFkngW6eCg0InN7b/J45+VCTjMok/nNjnRJ8\no4IvRnPwdUETt/iWI6xNPmfja+8OXrxIecTmNFUcjyj6qkzxzk6Ys4k09kR9Yb7F\nRIJgJUBR+Zt12Zuat/EsHh6BI5FkrjJI9v8Dc2m2EqLFDbARBa1QsWIxoxQy56Rc\n7Ns+2Lj6nIbtgjBEKyRNp88n5SdBJvAQu7WWpCURo+bz6cnoJ/+9auWuotmwmsor\nLNoxR3UycX8qP+zazC0UyltPAG1few2GZjpvEmC/Jt98zzUCAwEAAaNCMEAwDgYD\nVR0PAQH/BAQDAgIEMA8GA1UdEwEB/wQFMAMBAf8wHQYDVR0OBBYEFCV03I+j+jt3\nBvPmxZU8ylJqwG6tMA0GCSqGSIb3DQEBCwUAA4IBAQCEJkv7mWsetlMHgwd+Sghg\nQppAH0ZjwtfhCl7qaznZbt4dU9doeMoJ7axwQJdmzTiPJjnJuOfkEtWcj1jEfpX+\n/gVjhV0z6J+gKVLjwb7UBNYozBjjYf6ihDfdIQwGKja85csVOY3wThtndcUm7meH\n4yB2MDwhFAVTeOPFi1r1YZBLrsFS0euR2Qog/pIFKKzQuGvthDIxkYIlgBFN52qZ\nO6ZTnd88SjUI9epuuaVG5TqzDNfpYKF12oHnecHhgDwOWOWDEIfQ9uWJ1bvd1kzx\nUhlTMDBiF4LuazBsgFBAvA0vydmgb1VAXZuUZ7UKeHz690QqtOGaFPP0qTvkk3+R\n-----END CERTIFICATE-----\n'}}}
  2022-03-01 16:44:13,711 {'type': 'ADDED', 'object': {'kind': 'ConfigMap', 'apiVersion': 'v1', 'metadata': {'name': 'kube-root-ca.crt', 'namespace': 'default', 'uid': 'c6ba1aea-011b-40fc-9b45-585d620e803c', 'resourceVersion': '458', 'creationTimestamp': '2022-01-31T16:35:46Z', 'managedFields': [{'manager': 'kube-controller-manager', 'operation': 'Update', 'apiVersion': 'v1', 'time': '2022-01-31T16:35:46Z', 'fieldsType': 'FieldsV1', 'fieldsV1': {'f:data': {'.': {}, 'f:ca.crt': {}}}}]}, 'data': {'ca.crt': '-----BEGIN CERTIFICATE-----\nMIIC5zCCAc+gAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJl\ncm5ldGVzMB4XDTIyMDEzMTE2MzUwNVoXDTMyMDEyOTE2MzUwNVowFTETMBEGA1UE\nAxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAL1V\nu4wPid3F6Nxa81RyaWB5AbPJAe1lvEDobO3I2QtaZ0xBVLTqpTcO7oO0a4AZGvRT\nRKRzXMWqsbeLnZyC2Z0t5r6mtE/YToz0ntYe3FreN4EIIZooQDs+6p1sTNKg18RZ\nVqaIFpIGZAWvXZ/6AU1IWFu29IHOxgDg6NI2+x7ig6TB5zAwBfaA7IDoWDAMixVC\nU+9awGeJd6NOFu51QoAmL5b243D/yxxsJXrWEfALHKIpyAYoAivZ8/X7KzY8lnoy\np1My7HWgmfKEHG/imvejpECKSIXuEybSoni/shbihwzb+mDsJeiPLkLXedHG3Plb\nXUIS3wV5060zH54KroECAwEAAaNCMEAwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB\n/wQFMAMBAf8wHQYDVR0OBBYEFCy3AdPOZ4g5gZ3BSXZ7RVC+jhtkMA0GCSqGSIb3\nDQEBCwUAA4IBAQA3pekRNdua/7IM8FDkb/kKoBmnDiEFuChZ99Cqf/RV7GgzxpXA\npT6ZM3qNcLRrhXlMX21cppXJZOgU03CRF70/fqGxh4UPeHlHNVXNi4s/ghGQ7Vb8\nvm6Xdfik7hWZKfXHFJGPajp2odqibGxjV/o6ImpN6LIq4tc2wjHGkC7/kHVtoglc\nGcjIbMvuMQ4e1CqN8jv2cxaj5PiB4CqSEC5ek0PViZrH6RryLxnNia8FxOjYDDnD\nqs7Z7DO6rqo3LaCw8dBkaeCQ3XXPhOgoYrTz7jUwoQHBPUpYnZ1qZl3Z3seL1x4h\n9SdsO9AZTQPLrRDbwiB7G4xvGpUCPUOlxMIz\n-----END CERTIFICATE-----\n'}}}
  2022-03-01 17:02:26,189 {'type': 'MODIFIED', 'object': {'kind': 'ConfigMap', 'apiVersion': 'v1', 'metadata': {'name': 'flaskapp-config', 'namespace': 'default', 'uid': '88839aec-14ba-4f5f-a459-42f2d030c0c0', 'resourceVersion': '517997', 'creationTimestamp': '2022-03-01T13:04:23Z', 'annotations': {'kubectl.kubernetes.io/last-applied-configuration': '{"apiVersion":"v1","data":{"config.cfg":"MSG=\\"Welcome to Kubernetes!\\""},"kind":"ConfigMap","metadata":{"annotations":{},"name":"flaskapp-config","namespace":"default"}}\n'}, 'managedFields': [{'manager': 'kubectl', 'operation': 'Update', 'apiVersion': 'v1', 'time': '2022-03-01T13:04:23Z', 'fieldsType': 'FieldsV1', 'fieldsV1': {'f:data': {'.': {}, 'f:config.cfg': {}}, 'f:metadata': {'f:annotations': {'.': {}, 'f:kubectl.kubernetes.io/last-applied-configuration': {}}}}}]}, 'data': {'config.cfg': 'MSG="    Welcome to Operator!"'}}}
  2022-03-01 17:02:26,189 Modification detected
  2022-03-01 17:02:26,281 frontend-6544d7998f-rcxsf was deleted successfully
  ```


## To Do:
* Convert this to a helm chart
