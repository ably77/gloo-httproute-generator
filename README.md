# gloo-httproute-generator

A demo around using IDP concepts, Platform Engineering, and GitOps to enhance the developer experience with Gloo Gateway

## High Level Architecture Diagram
![High Level Architecture](.images/gh-action-idp-demo.png)

### Useful curl commands:

Configure an Upstream
```bash
curl -X POST \
  -H "Accept: application/vnd.github.v3+json" \
  -H "Authorization: token ${GITHUB_PAT}" \
  https://api.github.com/repos/ably77/gloo-httproute-generator/actions/workflows/generate-upstream-action.yaml/dispatches \
  -d '{
    "ref": "main",
    "inputs": {
      "upstream_name": "yahoo-finance",
      "upstream_uri": "query1.finance.yahoo.com"
    }
  }'
```

Configure a Route:
```bash
curl -X POST \
  -H "Accept: application/vnd.github.v3+json" \
  -H "Authorization: token ${GITHUB_PAT}" \
  https://api.github.com/repos/ably77/gloo-httproute-generator/actions/workflows/generate-route-action.yaml/dispatches \
  -d '{
    "ref": "main",
    "inputs": {
      "service_name": "yahoo-finance-route",
      "hostname": "yahoo-finance.glootest.com",
      "backend_ref_names": "yahoo-finance",
      "path_prefixes": "/",
      "jwt_disable": "true",
      "host_rewrite": "query1.finance.yahoo.com",
      "extauth_name": "foo-extauth",
      "extauth_namespace": "gloo-system"
    }
  }'
```

Delete a Route or Upstream
```bash
curl -X POST \
  -H "Accept: application/vnd.github.v3+json" \
  -H "Authorization: token ${GITHUB_PAT}" \
  https://api.github.com/repos/ably77/gloo-httproute-generator/actions/workflows/delete-route-action.yaml/dispatches \
  -d '{
    "ref": "main",
    "inputs": {
      "service_name": "yahoo-finance-route",
      "upstream_name": "yahoo-finance"
    }
  }'
```