# Configuration repository Extension

## Overview

Go supports (as beta feature) writing configuration plugins starting 16.2.0.

Configuration plugins allow users to keep **pipeline and environment** configurations
in all version control systems supported by Go.

The format in which configurations are stored is fully controlled by the plugin,
so pipelines and environments can be stored for example in JSON, YAML, XML, dot files or any convention that can store necessary information.

There are certain limits involved when using configuration repository:

 * It is not possible to edit remote pipeline or environment using UI
 * Elements defined via UI cannot refer to remote ones. The other way around is possible.
 * Pipeline templates and parameters are not supported

### Merging configurations

Go server polls all materials from pipelines and user-defined configuration repositories.
The final server configuration is merged from all elements defined via UI and remote elements in repositories.
It is important to pay attention to errors introduced in repositories. Go behavior changes once there are errors in at least one of the repositories. It is **highly recommended** to remove any errors as soon as Go reports them. For configuration repositories the only way to so is by pushing changes to faulty repository.
All configuration errors are displayed in server health messages.

#### Pipeline groups

Pipelines can be defined in many configuration repositories. Each has no restrictions
on group that it should belong to. So if you have group `project1` and many configuration
repositories with pipelines that all declare belonging to group `project1` then in Go server
all these pipelines will show up in group `project1`.

#### Environments

Similarly to pipeline groups, final members of environments are a sum of elements in
all configuration repositories. E.g.
 * in repository A we can define that `pipeline1` is member of environment `development`
 * in repository B we can define that `pipeline2` and `pipeline3` is member of environment `development`
 * in repository C we can define that `pipeline3` is member of environment `development`

Then final pipelines in environment `development` are `pipeline1`, `pipeline2`, `pipeline3`.
Notice that `pipeline3` membership in environment `development` was declared twice.

Same approach is used for agents.

Environment variables have additional check. If you assign same environment variable
with 2 different values then it is considered configuration merge conflict.

#### Conflicts

In some situations it is not possible to create merged configuration from configuration repositories
and main Go configuration. Server health messages will report **Invalid Merged Configuration** then.

The common cases that will cause this error are:
 * pipelines with same name are declared in many configuration repositories.
 * one of the pipelines refers to non-existing pipeline. Or more generally some configuration element refers to other that does not exist.

## References:

* [Developer docs](http://www.go.cd/documentation/developer/writing_go_plugins/configrepo/json_message_based_configrepo_extension.html)
* [Configuration repository plugins](http://www.go.cd/community/plugins.html#configuration-repository-plugins)
* [Github issue](https://github.com/gocd/gocd/issues/1133) including very long debate on how Go should behave
* [Example configuration repository in JSON](https://github.com/tomzo/gocd-json-config-example)
* [Example configuration repository in XML](https://github.com/tomzo/gocd-indep-config-part)
