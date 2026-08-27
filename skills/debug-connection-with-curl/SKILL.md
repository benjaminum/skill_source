---
name: debug-connection-with-curl
description: Diagnose HTTPS/network connection failures caused by proxy configuration and missing or untrusted TLS certificates using curl. Use when requests fail with proxy errors, TLS handshake failures, or certificate trust errors.
---

# Debug Connection With Curl

## Purpose

Use this skill to diagnose network connection failures that are commonly caused
by:

- Incorrect proxy configuration
- Missing or untrusted TLS certificates

## Tooling

- `curl`

## Agent workflow

1. Reproduce the issue with verbose output:
   - `curl -v https://<target-host>`
2. Check for environment proxy settings:
   - `env | grep -i proxy`
3. Test with and without proxy if applicable:
   - `curl -v --proxy "$HTTPS_PROXY" https://<target-host>`
   - `curl -v --noproxy "*" https://<target-host>`
4. Inspect TLS certificate behavior:
   - `curl -vI https://<target-host>`
5. If a custom CA is required, test with CA bundle:
   - `curl -v --cacert /path/to/ca.pem https://<target-host>`
6. Report root cause and recommend the minimal fix (proxy correction, CA
   installation, or trust configuration update).

## Expected output

- A short diagnosis of whether the failure is proxy-related or certificate-related.
- The exact `curl` command(s) that reproduced the failure.
- A minimal remediation plan.
