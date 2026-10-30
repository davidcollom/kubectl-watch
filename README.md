# `kubectl-watch`

`kubectl watch` is a [kubectl](https://kubernetes.io/docs/reference/kubectl/) plugin that pipes `kubectl`
commands through [watch](https://man7.org/linux/man-pages/man1/watch.1.html), so the output of a command
is re-run and refreshed on an interval.

Instead of re-running `kubectl get pods` by hand every few seconds, `kubectl watch` keeps the output
live in your terminal.

## Requirements

- `kubectl`
- `watch` from [procps-ng](https://gitlab.com/procps-ng/procps) installed and on your `PATH`
- Access to a Kubernetes cluster with a working kubeconfig

## Installation

### krew

This plugin is published in a personal [krew index](https://github.com/davidcollom/krew-index):

```bash
kubectl krew index add davidcollom https://github.com/davidcollom/krew-index
kubectl krew install davidcollom/watch
```

The index registers the plugin as `watch`, which installs the `kubectl-watch` binary and gives you the
`kubectl watch` subcommand. The index prefix is required — a bare `kubectl krew install watch` would
be resolved against the default index instead.

### Manual

Download the latest release archive and unzip it:

```bash
curl -LO https://github.com/davidcollom/kubectl-watch/releases/download/v0.0.1/release.zip
unzip release.zip
chmod +x kubectl-watch
mv kubectl-watch /usr/local/bin/
```

`kubectl` discovers plugins by looking for an executable named `kubectl-<plugin>` on your `PATH`, so the
script must keep its `kubectl-watch` name and be executable.

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

| Environment variable   | Default | Description                                                           |
| ---------------------- | ------- | --------------------------------------------------------------------- |
| `KUBECTL_WATCH_PARAMS` | `-d`    | Flags passed to `watch`. Use `-d` to show the diff between refreshes. |

## How it works

The plugin maps `kubectl watch ...` onto `watch`:

```bash
watch ${KUBECTL_WATCH_PARAMS:-'-d'} -- kubectl $@
```

## Releases

Releases are automated with GitHub Actions. Pushing a `v*.*.*` tag builds a `release.zip` archive and
publishes it to the [GitHub releases page](https://github.com/davidcollom/kubectl-watch/releases).

## License

No licence file is included in this repository.
