# svn-changed

Read-only check that reports whether watched paths changed between two revisions.
It does not check out any files, so it finishes in seconds. Use it at the start of a
pipeline and skip build tasks with `when` expressions.

## Parameters

| Name | Description | Default |
|------|-------------|---------|
| `url` | Subversion URL, for example the trunk (required) | |
| `from-revision` | Last built revision. Empty or `0` means first build | `""` |
| `to-revision` | Revision to compare against | `HEAD` |
| `watch-paths` | Comma separated paths relative to `url`. Files and folders. Empty or `.` watches everything | `""` |
| `image` | Image that provides `svn` | `ghcr.io/enma1111/svn-tools:latest` |

## Workspaces

| Name | Description |
|------|-------------|
| `basic-auth` | Optional. Secret with the keys `username` and `password` |

## Results

| Name | Description |
|------|-------------|
| `changed` | `true` or `false` |
| `head-revision` | `to-revision` resolved to a number. Pass it to `svn-checkout` |
| `last-changed-revision` | Newest revision that touched a watched path, `0` if none |
| `changed-count` | Number of distinct changed paths inside the watched paths |

## Use in a pipeline

```yaml
- name: changes-contacts
  taskRef: { name: svn-changed }
  params:
    - { name: url, value: $(params.svn-url) }
    - { name: from-revision, value: $(params.last-built-revision) }
    - { name: watch-paths, value: "Lexware.Contacts, Core, Directory.Build.props" }
- name: build-contacts
  runAfter: [changes-contacts]
  when:
    - input: $(tasks.changes-contacts.results.changed)
      operator: in
      values: ["true"]
  taskRef: { name: dotnet-build }
```

## Notes

- Include shared files (`Directory.Build.props`, `global.json`) and dependency
  folders in `watch-paths`, otherwise changes there are not noticed.
- Added, deleted, moved and modified paths all count as changes.
- The first build (empty `from-revision`) always reports `changed=true`.