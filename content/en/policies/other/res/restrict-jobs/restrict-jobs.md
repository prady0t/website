---
title: "Restrict Jobs"
category: Other
version: 
subject: Job
policyType: "validate"
description: >
    Jobs can be created directly and indirectly via a CronJob controller. In some cases, users may want to only allow Jobs if they are created via a CronJob. This policy restricts Jobs so they may only be created by a CronJob.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/res/restrict-jobs/restrict-jobs.yaml" target="-blank">/other/res/restrict-jobs/restrict-jobs.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-jobs
  annotations:
    policies.kyverno.io/title: Restrict Jobs
    policies.kyverno.io/category: Other
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Job
    kyverno.io/kyverno-version: 1.10.0
    kyverno.io/kubernetes-version: "1.26"
    policies.kyverno.io/description: >-
      Jobs can be created directly and indirectly via a CronJob controller.
      In some cases, users may want to only allow Jobs if they are created via a CronJob.
      This policy restricts Jobs so they may only be created by a CronJob.
spec:
  validationFailureAction: Enforce
  rules:
    - name: restrict-job-from-cronjob
      match:
        any:
        - resources:
            kinds:
              - Job
      preconditions:
        any:
          - key: "{{ request.object.metadata.ownerReferences[0].kind || '' }}"
            operator: NotEquals
            value: CronJob
      validate:
        message: Jobs are only allowed if spawned from CronJobs.
        deny: {}

```
