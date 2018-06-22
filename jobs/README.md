# [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)

### Create a [job](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) that runs every 3 minutes and prints out the current time.

```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/3 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: displayDate
            image: busybox
            args:
            - /bin/sh
            - -c
            - date
          restartPolicy: OnFailure
```

### Create a job that runs 20 times, 5 containers at a time, and prints "Hello parallel world"

```
apiVersion: batch/v1
kind: Job
metadata:
  name: helloP
spec:
  completions: 20
  parallelism: 5
  template:
    spec:
      containers:
      - name: helloP
        image: perl
        command: ["echo",  "Hello parallel world"]
      restartPolicy: Never
```
