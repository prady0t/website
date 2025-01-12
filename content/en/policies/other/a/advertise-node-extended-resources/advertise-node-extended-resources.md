---
title: "Advertise Node Extended Resources"
category: Other
version: 1.9.0
subject: Node
policyType: "mutate"
description: >
    Kubernetes Nodes, in addition to standard compute resources like CPU and memory, may offer extended resources such as FPGAs and GPUs, both of which can be defined per custom design. These extended resources are advertised in the `status` object of a Node. This policy, functional only starting in Kyverno 1.9, adds the extended resource `example.com/dongle` with a value/capacity of `2` to Kubernetes Nodes.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/a/advertise-node-extended-resources/advertise-node-extended-resources.yaml" target="-blank">/other/a/advertise-node-extended-resources/advertise-node-extended-resources.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: advertise-node-extended-resources
  annotations:
    policies.kyverno.io/title: Advertise Node Extended Resources
    policies.kyverno.io/category: Other
    policies.kyverno.io/severity: medium
    kyverno.io/kyverno-version: 1.9.0
    policies.kyverno.io/minversion: 1.9.0
    kyverno.io/kubernetes-version: "1.24"
    policies.kyverno.io/subject: Node
    policies.kyverno.io/description: >-
      Kubernetes Nodes, in addition to standard compute resources like
      CPU and memory, may offer extended resources such as FPGAs and GPUs, both
      of which can be defined per custom design. These extended resources are
      advertised in the `status` object of a Node. This policy, functional only
      starting in Kyverno 1.9, adds the extended resource `example.com/dongle` with
      a value/capacity of `2` to Kubernetes Nodes.
spec:
  background: false
  rules:
    - name: advertise-dongle
      match:
        any:
        - resources:
            kinds:
            - Node/status
      mutate:
        patchStrategicMerge:
          status:
            capacity:
              example.com/dongle: 2
```
