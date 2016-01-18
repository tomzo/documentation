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
All configuration errors are displayed

## References:

* [Developer docs](http://www.go.cd/documentation/developer/writing_go_plugins/configrepo/json_message_based_configrepo_extension.html)
* [Configuration repository plugins](http://www.go.cd/community/plugins.html#configuration-repository-plugins)
* [Github issue](https://github.com/gocd/gocd/issues/1133) including very long debate on how Go should behave
* [Example configuration repository in JSON](https://github.com/tomzo/gocd-json-config-example)
* [Example configuration repository in XML](https://github.com/tomzo/gocd-indep-config-part)
