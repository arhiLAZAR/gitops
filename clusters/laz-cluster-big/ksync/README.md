# Ksync
Synchronizes folders between your laptop and a pod in the kubernetes cluster.

## How to install on Linux/MacOSX
1) Use an install script:
```
curl https://ksync.github.io/gimme-that/gimme.sh | bash
```
or just download from [release page](https://github.com/ksync/ksync/releases)

2) Create the initial local configs:
```
ksync init --remote=false
```
Please, do not forget to add `--remote=false`

## How to use
1) Run watch in background / different console / tmux / screen / as a service / etc:
```
ksync watch
```
2) Show ksync which folders to use:
```
ksync create -n <namespace> --pod <pod_name> /local/directory /directory/on/server
```
For example:
```
ksync create -n services --pod alert-publisher /home/user/src/my-app /opt/my-app
```
Please notice a couple of things:

a) You must explicitly specify a namespace, even if you have it in your kubeconfig

b) When you use the `--pod <pod_name>` flag, actually you sync with _all_ of the pods in this deployment

c) Instead of `--pod <pod_name>` flag you can use the `--selector <label>` one. For example:

```
ksync create -n services --selector app=alert-publisher /home/user/src/my-app /opt/my-app
```

## Debug hints
### Get the list of all syncing pods
```
ksync get
```
Status must be `watching`

### Delete all syncronizations
```
ksync delete --all
```

### Connection refused to 127.0.0.1:8384
If you see the following error from the `ksync watch` output:
```
Get "http://localhost:8384/rest/events?since=18": dial tcp 127.0.0.1:8384: connect: connection refused
```
most likely, you're trying to sync with a deployment containing 4 or more pods.

Also, you can try to remove all synchronizations `ksync delete --all` and then kill and restart the `ksync watch` process.

### Official Readme
See the full version of Readme on the [official site](https://github.com/ksync/ksync/blob/master/README.md)
