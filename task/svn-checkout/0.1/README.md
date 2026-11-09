# svn-checkout

Checks out (or exports) a path from a Subversion repository into a workspace.
`HEAD` is resolved to a fixed revision first, so the checkout and the returned result always match.

## Parameters

| Name | Description | Default |
|------|-------------|---------|
| `url` | Subversion URL (required) | |
| `revision` | Revision number or `HEAD` | `HEAD` |
| `subdirectory` | Target folder inside the `output` workspace | `""` |
| `export` | `"true"` for `svn export` (no `.svn` folders) | `"false"` |
| `image` | Image that provides `svn` | `ghcr.io/enma1111/svn-tools:latest` |

## Workspaces

| Name | Description |
|------|-------------|
| `output` | Receives the checked-out files |
| `basic-auth` | Optional. Secret with the keys `username` and `password` |

## Results

| Name | Description |
|------|-------------|
| `revision` | The revision that was fetched |

## Example

See [samples/taskrun.yaml](samples/taskrun.yaml).

```bash
kubectl create secret generic svn-credentials \
  --from-literal=username=USER --from-literal=password=PASSWORD
kubectl apply -f svn-checkout.yaml
kubectl create -f samples/taskrun.yaml
```

## Notes

- The password is passed through standard input, never as a command line argument.
- Certificate errors are not ignored. Use a trusted certificate or provide your CA in a custom image.
- In production, set `image` to a date tag or digest instead of `latest`.