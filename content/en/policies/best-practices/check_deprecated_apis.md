---
title: "Check deprecated APIs"
category: Best Practices
version: 
subject: Kubernetes APIs
policyType: "validate"
description: >
    Kubernetes APIs are sometimes deprecated and removed after a few releases.  As a best practice older API versions should be replaced with newer versions.  This policy validates for APIs that are deprecated or scheduled for removal.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/check_deprecated_apis.yaml" target="-blank">/best-practices/check_deprecated_apis.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: check-deprecated-apis
  annotations:
    policies.kyverno.io/title: Check deprecated APIs
    policies.kyverno.io/category: Best Practices
    policies.kyverno.io/subject: Kubernetes APIs
    policies.kyverno.io/description: >-
      Kubernetes APIs are sometimes deprecated and removed after a few releases. 
      As a best practice older API versions should be replaced with newer versions. 
      This policy validates for APIs that are deprecated or scheduled for removal.
spec:
  validationFailureAction: audit
  rules:
  - name: validate-v1-22-removals
    match:
      resources:
        kinds:
        - admissionregistration.k8s.io/v1beta1/ValidatingWebhookConfiguration
        - admissionregistration.k8s.io/v1beta1/MutatingWebhookConfiguration
        - apiextensions.k8s.io/v1beta1/CustomResourceDefinition
        - apiregistration.k8s.io/v1beta1/APIService
        - authentication.k8s.io/v1beta1/TokenReview
        - authorization.k8s.io/v1beta1/SubjectAccessReview
        - authorization.k8s.io/v1beta1/LocalSubjectAccessReview
        - authorization.k8s.io/v1beta1/SelfSubjectAccessReview 
        - certificates.k8s.io/v1beta1/CertificateSigningRequest
        - coordination.k8s.io/v1beta1/Lease
        - extensions/v1beta1/Ingress
        - networking.k8s.io/v1beta1/Ingress
    validate:
      message: >-
        {{ request.object.apiVersion }}/{{ request.object.kind }} is deprecated and will be removed in v1.22. 
        See: https://kubernetes.io/blog/2021/07/14/upcoming-changes-in-kubernetes-1-22/
      deny: {}

```
