# `kubectl-watch`

`kubectl watch` is a [kubectl](https://kubernetes.io/docs/reference/kubectl/) plugin that pipes `kubectl`
commands through [watch](https://github.com/sqweek/watch), so the output of a command is re-run and
refreshed on an interval.

Instead of re-running `kubectl get pods` by hand every few seconds, `kubectl watch` keeps the output
live in your terminal.

## Requirements

- `kubectl`
- [watch](https://github.com/sqweek/watch) installed and on your `PATH`
- Access to a Kubernetes cluster with a working kubeconfig

## Installation

### krew

This plugin is published in a personal [krew index](https://github.com/davidcollom/krew-index):

```bash
kubectl krew index add davidcollom https://github.com/davidcollom/krew-index
kubectl krew install kubectl-watch
```

### Manual

Download the latest release archive, unzip it, and place the `kubectl-watch` script on your `PATH`:

```bash
kubectl watch get pods
```

`kubectl` discovers plugins by looking for an executable named `kubectl-<plugin>` on your `PATH`, so the
script must keep its `kubectl-watch` name and be executable (`chmod +x kubectl-watch`).

## Usage

```bash
# Watch pod status
kubectl watch get pods

# Watch deployments in a specific namespace
kubectl watch -n my-namespace get deployments

# Watch any kubectl subcommand
kubectl watch get nodes
```

Everything after `watch` is handed to `kubectl`, and the resulting command is executed through `watch`.

## Configuration

| Environment variable     | Default | Description                                                                |
| ------------------------ | ------- | -------------------------------------------------------------------------- |
| `KUBECTL_WATCH_PARAMS`   | `-d`    | Flags passed to `watch`. Use `-d` to show the diff between refreshes.      |

## How it works

The plugin maps `kubectl watch ...` onto `watch`:

```bash
watch ${KUBECTL_WATCH_PARAMS:-'-d'} -- kubectl $@
```

## Releases

Releases are automated with GitHub Actions. Pushing a `v*.*.*` tag builds a `release.zip` archive and
publishes it to the [GitHub releases page](https://github.com/davidcollom/kubectl-watch/releases).

## License

See the repository for licensing details.
