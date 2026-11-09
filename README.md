# tekton-svn-tasks

Reusable [Tekton](https://tekton.dev) tasks for working with Subversion in Kubernetes pipelines.
They use the [svn-tools](https://github.com/Enma1111/svn-tools) container image.

> **Unofficial.** Not affiliated with or endorsed by the Apache Software Foundation or the Tekton project.
> Apache and Subversion are trademarks of the Apache Software Foundation.

## Tasks

| Task | Description | Version |
|------|-------------|---------|
| [svn-checkout](task/svn-checkout/0.1/README.md) | Check out or export a path, returns the revision | 0.1 |

## Usage

Reference a task directly from this repository with the Tekton git resolver:

```yaml
taskRef:
  resolver: git
  params:
    - name: url
      value: https://github.com/Enma1111/tekton-svn-tasks
    - name: revision
      value: main
    - name: pathInRepo
      value: task/svn-checkout/0.1/svn-checkout.yaml
```

For production, pin `revision` to a Git tag or commit instead of `main`.

## Contributing

Issues and pull requests are welcome. Each task lives in `task/<name>/<version>/`.
New versions get a new folder, old versions stay for people who pin them.

## License

Apache-2.0