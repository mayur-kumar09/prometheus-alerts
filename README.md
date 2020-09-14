# prometheus-alerts

Successive Technologies uses Prometheus as the primary alert tool. This repository aims to make defining and deploying new alerts as frictionless as possible.

## Adding a New Alert

All alerts are stored within the repository. The general workflow follows:

1. Raise PR for new alert 

2. Validate on CI: Check alert syntax with `promtool`, validate the alert schema with `yamale`. Schema can be found at `schema/alerts/schema.yaml`. 

3. Merge PR

4. Alert is Live

The Merge->Live step is completed by running `git-sync` as a sidecar in the `prometheus-server` pod. The sidecar will automatically pull new alerts to a volume mounted on the Prometheus pod. When an update occurs the config reloader triggers a reload of the server.

### General Alerts

These alerts are deployed to **all** Kubernetes clusters. Simply add your alert to `alerts/core_alerts.yml`.



## Built With

* [promtool](https://github.com/prometheus/prometheus/tree/master/cmd/promtool) - Prometheus alert syntax checker.
* [Yamale](https://github.com/23andMe/Yamale) - YAML schema validator.
* [yamale-github-action](https://github.com/nrkno/yaml-schema-validator-github-action) - GitHub action leveraging `Yamale`.
* [git-sync](https://github.com/kubernetes/git-sync) - Sidecar to clone git repo.