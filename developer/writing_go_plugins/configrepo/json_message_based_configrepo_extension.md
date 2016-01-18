# Configuration repository plugin - JSON API - Message based

The objective of this guide is to explain how to write a [configuration repository plugin](configrepo_plugin_overview.md), for Go.

Useful references:
* [Overview of configuration repository plugins - External link to Go's user documentation](http://www.go.cd/documentation/user/current/extension_points/configrepo_extension.html)
* [Overview of message-based APIs](../json_message_based_plugin_api.md)
* [Structure of a plugin and writing one](../go_plugins_basics.md)
* [A sample Configuration repository plugin - JSON](https://github.com/tomzo/gocd-json-config-plugin)

A configuration repository plugin is a Go plugin, which claims to support to extension name `configrepo` in its identifier, and responds to the messages mentioned below, appropriately.

Go provides a clean checkout of the SCM repository to configuration plugin and expects that plugin will be capable of parsing all necessary files to return pipeline and environment configurations. Therefore any configuration plugin is responsible for:

 * identifying which files to parse in provided directory
 * parsing and validating files and their format
 * returning configuration objects and/or errors found in the checkout directory

If configuration object returned by plugin is invalid, Go server will treat related configuration repository as invalid - it will not merge it into current configuration and it will show errors on the dashboard.

## Messages to be handled by the plugin - ***version 1.0***

[Parse directory](version_1_0/parse_directory.md)

## Other information

* Availability: Go version 16.2.0 onwards
* Extension Name: `configrepo`
