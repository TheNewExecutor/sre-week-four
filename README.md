# SRE Fundamentals with Google Week 4

This week's assignment explored canary release using Helm. The real work in the assignment was to understand the details of what it takes to deploy a canary release. Some observations include:
- canary configuration files are very similar to the upcommerce files, but with name changes
- compute resources must be allocated between the canary and control releases
- the `values.yml` file must be updated to include a separate deployment
- `helm update` and `helm rollback` commands are very convenient for managing deployments


## CodeSpace Session: Helm Rollback
```bash
TheNewExecutor ➜ /workspaces/sre-week-four (main) $ kubectl get deployment -n sre -o wide
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS   IMAGES                       SELECTOR
upcommerce-app          1/1     1            1           9m28s   upcommerce   uonyeka/upcommerce:v3        app=upcommerce-app
upcommerce-canary-app   0/1     1            0           23s     canary       uonyeka/canary:linux-amd64   app=upcommerce-canary-app
@TheNewExecutor ➜ /workspaces/sre-week-four (main) $ kubectl get deployment -n sre -o wide
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES                       SELECTOR
upcommerce-app          1/1     1            1           10m   upcommerce   uonyeka/upcommerce:v3        app=upcommerce-app
upcommerce-canary-app   0/1     1            0           67s   canary       uonyeka/canary:linux-amd64   app=upcommerce-canary-app
@TheNewExecutor ➜ /workspaces/sre-week-four (main) $ helm list -a -n sre
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
upcommerce      sre             2               2024-04-24 06:36:27.471634444 +0000 UTC deployed        upcommerce-0.1.0                   
@TheNewExecutor ➜ /workspaces/sre-week-four (main) $ helm rollback upcommerce 1 -n sre
Rollback was a success! Happy Helming!
@TheNewExecutor ➜ /workspaces/sre-week-four (main) $ helm list -a -n sre
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
upcommerce      sre             3               2024-04-24 06:38:50.820590015 +0000 UTC deployed        upcommerce-0.1.0                   
@TheNewExecutor ➜ /workspaces/sre-week-four (main) $ ```
