---
title: "Namespace Protection"
category: Other
version: 1.9.0
subject: Namespace
policyType: "validate"
description: >
    Cases where RBAC may be applied at a higher level and where Namespace-level protections may be necessary can be accomplished with a separate policy. For example, one may want to protect creates, updates, and deletes on only a single Namespace. This policy will block creates, updates, and deletes to any Namespace labeled with `freeze=true`. Caution should be exercised when using rules which match on all kinds (`"*"`) as this will involve, for larger clusters, a substantial amount of processing on Kyverno's part. Additional resource requests and/or limits may be required.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/m-q/namespace-protection/namespace-protection.yaml" target="-blank">/other/m-q/namespace-protection/namespace-protection.yaml</a>

```yaml
apiVersion: kyverno.io/v2beta1
kind: ClusterPolicy
metadata:
  name: namespace-protection
  annotations:
    policies.kyverno.io/title: Namespace Protection
    policies.kyverno.io/category: Other
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Namespace
    kyverno.io/kyverno-version: 1.9.0
    policies.kyverno.io/minversion: 1.9.0
    kyverno.io/kubernetes-version: "1.24"
    policies.kyverno.io/description: >-
      Cases where RBAC may be applied at a higher level and where Namespace-level
      protections may be necessary can be accomplished with a separate policy. For example,
      one may want to protect creates, updates, and deletes on only a single Namespace. This
      policy will block creates, updates, and deletes to any Namespace labeled with `freeze=true`.
      Caution should be exercised when using rules which match on all kinds (`"*"`) as this will
      involve, for larger clusters, a substantial amount of processing on Kyverno's part. Additional
      resource requests and/or limits may be required.
spec:
  validationFailureAction: Enforce
  background: false
  rules:
    - name: check-freeze
      match:
        any:
        - resources:
            kinds:
            - "*"
            namespaceSelector:
              matchExpressions:
                - key: freeze
                  operator: In
                  values:
                  - "true"
      validate:
        message: "This Namespace is frozen and no modifications may be performed."
        deny: {}
```
