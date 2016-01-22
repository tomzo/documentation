# Task objects in `configrepo` extension point

Every task object must have `type` field. Which can be `exec`, `ant`, `nant`, `rake`, `fetch`, `plugin`

Optionally any task can have `run_if` and `on_cancel`.

 * `run_if` is a string. Valid values are `passed`, `failed`, `any`
 * `on_cancel` is a task object. Same rules apply as to tasks described on this page.

### Exec

```json
{
    "type": "exec",
    "run_if": "passed",
    "on_cancel" : null,
    "command": "make",
    "arguments": [
      "-j3",
      "docs",
      "instal"
    ],
    "working_directory": null      
}
```

### Ant

```json
{
  "build_file": "mybuild.xml",
  "target": "compile",
  "type": "ant",
  "run_if": "any",
  "on_cancel" : null,
}
```

### Nant

```json
{
  "type": "nant",
  "run_if": "passed",
  "working_directory": "script/build/123",
  "build_file": null,
  "target": null,
  "nant_path": null
}
```

### Rake

```json
{
  "type": "rake",
  "run_if": "passed",
  "working_directory": "sample-project",
  "build_file": null,
  "target": null
}
```

### Fetch

```json
{
   "type": "fetch",
   "run_if": "any",
   "pipeline": "upstream",
   "stage": "upstream_stage",
   "job": "upstream_job",
   "is_source_a_file": false,
   "source": "result",
   "destination": "test"
 }
```

### Plugin

```json
{
  "type": "plugin",
  "configuration": [
    {
      "key": "ConverterType",
      "value": "jsunit"
    },
    {
      "key": "password",
      "encrypted_value": "ssd#%fFS*!Esx"
    }
  ],
  "run_if": "passed",
  "plugin_configuration": {
    "id": "xunit.converter.task.plugin",
    "version": "1"
  },
  "on_cancel": null
}
```
