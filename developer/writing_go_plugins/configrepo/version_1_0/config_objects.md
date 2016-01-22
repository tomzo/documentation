# Configuration objects supported by `configrepo` extension point

This page should be used as reference on valid configuration objects in `configrepo` extension point.
All these can be part of response for [parse-directory message](parse_directory.md)

## Similarity to API

Objects used by extension point are similar to one in the [Go API](https://api.go.cd/current).
But beware that this is a different model, some of the differences are:

 * pipeline declares `group` which it belongs to.
 * object tree is more "flat" here. E.g. `"run_if" : "passed"`  vs `"run_if" : [ "passed" ]` in API.
 * there is no split between `type` and `attributes` in JSON objects

## Location field

`location` is optional field on all JSON objects. It is used to generate error messages and might be used in future UI to show origin of configuration element. It is recommended to assign `location` a relative file path and/or line of the configuration element.

### Index

1. [Environment](#environment)
1. [Environment variables](#environment-variables)
1. [Pipeline](#pipeline)
    * [Mingle](#mingle)
    * [Tracking tool](#tracking tool)
    * [Timer](#timer)
1. [Stage](#stage)
    * [Approval](#approval)
1. [Job](#job)
    * [Property](#property)
    * [Tab](#tab)
1. [Tasks](tasks.md)
    * [rake](tasks.md#rake)
    * [ant](tasks.md#ant)
    * [nant](tasks.md#nant)
    * [exec](tasks.md#exec)
    * [fetch](tasks.md#fetch)
    * [pluggabletask](tasks.md#plugin)
1. [Materials](materials.md)
    * [dependency](materials.md#dependency)
    * [package](materials.md#package)
    * [git](materials.md#git)
    * [svn](materials.md#svn)
    * [perforce](materials.md#perforce)
    * [tfs](materials.md#tfs)
    * [hg](materials.md#hg)
    * [pluggable scm](materials.md#pluggable)

# Environment

Configures a [Go environment](http://www.go.cd/documentation/user/current/configuration/managing_environments.html)

```json
{
  "location" : "src/environments/dev.yaml",
  "name": "dev",
  "environment_variables": [
    {
      "name": "key1",
      "value": "value1"
    }
  ],
  "agents": [
    "123"
  ],
  "pipelines": [
    "mypipeline1"
  ]
}
```

# Environment variables

Environment variables is a JSON array that can be declared in Environments, Pipelines, Stages and Jobs.

Any variable must contain `name` and `value` or `encrypted_value`.

```json
{
  "environment_variables": [
    {
      "name": "key1",
      "value": "value1"
    },
    {
      "name": "keyd",
      "encrypted_value": "v12@SDfwez"
    }
  ]
}
```

# Pipeline

```json
{
  "location" : "src/pipelines/pipe2.yaml",
  "group": "group1",
  "name": "pipe2",
  "label_template": "foo-1.0-${COUNT}",
  "enable_pipeline_locking": true,
  "mingle": {
    "base_url": "http://mingle.example.com",
    "project_identifier": "my_project"
  },
  "tracking_tool": null,
  "timer": {
    "spec": "0 15 10 * * ? *"
  },
  "environment_variables": [],
  "materials": [
    ...
  ],
  "stages": [
    ...
  ]
}
```

Please note:

 * templates are not supported
 * parameters are not supported
 * pipeline declares a group to which it belongs

### Mingle

```json
{
    "base_url": "https://mingle.example.com",
    "project_identifier": "foobar_widgets",
    "mql_grouping_conditions": "status > 'In Dev'"
}
```

### Tracking tool

```json
{
  "link": "http://your-trackingtool/yourproject/${ID}",
  "regex": "evo-(\\d+)"
}
```

### Timer

```json
{
    "spec": "0 0 22 ? * MON-FRI",
    "only_on_changes": true
}
```

# Stage

```json
{
  "name": "test",
  "fetch_materials": true,
  "never_cleanup_artifacts": false,
  "clean_working_directory": false,
  "approval" : null,
  "environment_variables": [
    {
      "name": "TEST_NUM",
      "value": "1"
    }
  ],
  "jobs": [
    ...
  ]
}
```

### Approval

```json
{
  "type": "manual",
  "users": [],
  "roles": [
    "manager"
  ]
}
```

# Job

```json
{
  "name": "test",
  "environment_variables": [],
  "tabs": [
     {
       "name": "test",
       "path": "results.xml"
     }
   ],
   "resources": [
    "linux"
  ],
  "artifacts": [
    {
      "source": "src",
      "destination": "dest",
      "type": "test"
    }
  ],
  "properties": [
    {
      "name": "perf",
      "source": "test.xml",
      "xpath": "substring-before(//report/data/all/coverage[starts-with(@type,\u0027class\u0027)]/@value, \u0027%\u0027)"
    }
  ],
  "tasks": [
    ...
  ]
}
```

### Property

```json
{
  "name": "coverage.class",
  "source": "target/emma/coverage.xml",
  "xpath": "substring-before(//report/data/all/coverage[starts-with(@type,'class')]/@value, '%')"
}
```

### Tab

```json
{
      "name": "cobertura",
      "path": "target/site/cobertura/index.html"
}
```
