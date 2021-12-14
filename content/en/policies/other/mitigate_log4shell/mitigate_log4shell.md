---
title: "Log4Shell Mitigation"
category: Sample
version: 
subject: Pod
policyType: "mutate"
description: >
    In response to CVE-2021-44228 referred to as Log4Shell, a RCE vulnerability in the Log4j library, a workaround for versions 2.10 to 2.14.1 of the library is to set the environment variable LOG4J_FORMAT_MSG_NO_LOOKUPS to "true" which will disable the vulnerable feature. This policy will mutate all initContainers and containers in an incoming Pod to add this environment variable automatically.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/mitigate_log4shell/mitigate_log4shell.yaml" target="-blank">/other/mitigate_log4shell/mitigate_log4shell.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: log4shell-mitigation
  annotations:
    policies.kyverno.io/title: Log4Shell Mitigation
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/category: Sample
    policies.kyverno.io/description: >-
      In response to CVE-2021-44228 referred to as Log4Shell, a RCE vulnerability in the Log4j library, a
      workaround for versions 2.10 to 2.14.1 of the library is to set the environment
      variable LOG4J_FORMAT_MSG_NO_LOOKUPS to "true" which will disable the vulnerable
      feature. This policy will mutate all initContainers and containers in an
      incoming Pod to add this environment variable automatically.
spec:
  rules:
  - name: add-log4shell-mitigation-initcontainers
    match:
      resources:
        kinds:
        - Pod
    mutate:
      patchStrategicMerge:
        spec:
          initContainers:
            - (name): "*"
              env:
              - name: LOG4J_FORMAT_MSG_NO_LOOKUPS
                value: "true"
  - name: add-log4shell-mitigation-containers
    match:
      resources:
        kinds:
        - Pod
    mutate:
      patchStrategicMerge:
        spec:
          containers:
            - (name): "*"
              env:
              - name: LOG4J_FORMAT_MSG_NO_LOOKUPS
                value: "true"

```