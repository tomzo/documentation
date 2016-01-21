# Material objects in `configrepo` extension point

All materials:

 * must have `type` - `git`, `svn`, `hg`, `p4`, `tfs`, `dependency`, `package`, `plugin`.
 * can have `name` and must have `name` when there is more than one material in pipeline

SCM-related materials have `destination` field.

## Git

```json
{
  "url": "http://my.git.repository.com",
  "branch": "feature12",
  "filter": {
    "ignore": [
      "externals",
      "tools"
    ]
  },
  "destination": "dir1",
  "auto_update": false,
  "name": "gitMaterial1",
  "type": "git"
}
```

## Svn

```json
{
  "url": "http://svn",
  "username": "user1",
  "password": "pass1",
  "check_externals": true,
  "filter": {
    "ignore": [
      "tools",
      "lib"
    ]
  },
  "destination": "destDir1",
  "auto_update": false,
  "name": "svnMaterial1",
  "type": "svn"
}
```

Instead of plain `password` you may specify `encrypted_password` with encrypted content
which usually makes more sense considering that value is stored in SCM.

## Hg

```json
{
  "url": "repos/myhg",
  "filter": {
    "ignore": [
      "externals",
      "tools"
    ]
  },
  "destination": "dir1",
  "auto_update": false,
  "name": "hgMaterial1",
  "type": "hg"
}
```

## Perforce

```json
{
  "port": "10.18.3.102:1666",
  "username": "user1",
  "password": "pass1",
  "use_tickets": false,
  "view": "//depot/dev/src...          //anything/src/...",
  "filter": {
    "ignore": [
      "lib",
      "tools"
    ]
  },
  "destination": "dir1",
  "auto_update": false,
  "name": "p4materialName",
  "type": "p4"
}
```

Instead of plain `password` you may specify `encrypted_password` with encrypted content
which usually makes more sense considering that value is stored in SCM.

## Tfs

```json
{
  "url": "url3",
  "username": "user4",
  "domain": "example.com",
  "password": "pass",
  "project": "projectDir",
  "filter": {
    "ignore": [
      "tools",
      "externals"
    ]
  },
  "destination": "dir1",
  "auto_update": false,
  "name": "tfsMaterialName",
  "type": "tfs"
}
```

Instead of plain `password` you may specify `encrypted_password` with encrypted content
which usually makes more sense considering that value is stored in SCM.

## Dependency

```json
{
  "pipeline": "pipeline2",
  "stage": "build",
  "name": "pipe2",
  "type": "dependency"
}
```

## Package

```json
{
  "package_id": "apt-repo-id",
  "name": "myapt",
  "type": "package"
}
```

## Pluggable SCM

```json

```
