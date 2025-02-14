# Job -
- Job is like a task that you want to run and complete.
- It creates a pod to do the job.
- The job runs until it's finished (successfully).
- If the job fails, Kubernetes will try again until it succeeds or the maximum number of retries is reached.

            apiVersion: batch/v1
            kind: Job
            metadata:
              name: hello-job
            spec:
              template:
                spec:
                  containers:
                  - name: hello
                    image: busybox
                    command: ["echo", "Hello, Kubernetes!"]
                  restartPolicy: OnFailure  # Restart only if it fails


### List the jobs -

       kubectl get jobs

## Running Jobs in Parallel -
- You can run multiple pods in parallel if you need to process a large number of tasks concurrently.
        
        apiVersion: batch/v1
        kind: Job
        metadata:
          name: hello-job
        spec:
          completions: 5   # The total number of successful pods the job needs to complete
          parallelism: 3   # The number of pods to run in parallel
          template:
            spec:
              containers:
              - name: hello
                image: busybox
                command: ["echo", "Hello, Kubernetes!"]
              restartPolicy: OnFailure
          backoffLimit: 4

- completions: 5: The job will be considered complete when 5 successful completions have occurred.
- parallelism: 3: The job will run 3 pods in parallel at a time.



# Cron Job -
- CronJob in Kubernetes is a way to run tasks automatically on a schedule that just like a cron job in Linux.
- CronJob allows you to run a task (like a script or job) at a specific time or on a recurring schedule (e.g., every day, every hour).
- You set it up using a cron-like syntax to define when the task should run


      apiVersion: batch/v1
      kind: CronJob
      metadata:
        name: hello-cronjob
      spec:
        schedule: "* * * * *"  # This cron schedule means "run every minute"
        jobTemplate:
          spec:
            template:
              spec:
                containers:
                - name: hello
                  image: busybox
                  command: ["echo", "Hello, Kubernetes!"]
                restartPolicy: OnFailure  # Restart only if the pod fails
      

## To check the CronJob status:

      kubectl get cronjobs

## To see the jobs triggered by the CronJob:

      kubectl get jobs




















