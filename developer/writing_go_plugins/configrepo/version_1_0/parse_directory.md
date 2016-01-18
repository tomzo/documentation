## Message: Parse Directory

This message is sent by the server, when it has completed a checkout of configuration repository and wants the plugin to parse it.

### Request - From the server

***Request name***: `parse-directory`

***Request parameters***: empty

***Request headers***: empty

***Request body***: This contains the directory to be parsed as a JSON string:

```json
{
  "directory" : "<full path to checkout>"
}
```

***Example request***:

```json
{
  "directory" : "/var/lib/go-server/pipelines/flyweight/145cefce-a972-46a6-ab40-954b08b75cf2"
}
```

### Response - From the plugin

***Expected response body***: The plugin is expected to send a response, which contains partial configuration of pipelines and environments defined in the requested directory. Plugin may also return error in response as needed.

Note: If plugin responds with errors Go Server shows those in "server health messages" and treats configuration repository as invalid until plugin returns no errors.

***Example response***:

```json
{
    "errors": [
        {
          "location" : "mypipeline.json",
          "message" : "file is not in JSON format"
        }
    ],
    "pipelines" : [
      {
        "label_template": "${COUNT}",
        "enable_pipeline_locking": false,
        "name": "my_pipeline",
        "tracking_tool": null,
        "timer": null,
        "environment_variables": [],
        "materials": [
          {
            "type": "git",
            "url": "git@github.com:example/sample_repo.git",
            "destination": "code",
            "filter": {
                "ignore": [
                        "**/*.*",
                        "**/*.html"
                ]
            },
            "name": "git",
            "auto_update": true,
            "branch": "master",
            "submodule_folder": null
          }
        ],
        "stages": [
          {
            "name": "my_stage",
            "fetch_materials": true,
            "clean_working_directory": false,
            "never_cleanup_artifacts": false,
            "approval": {
              "type": "success",
              "authorization": {
                "roles": [],
                "users": []
              }
            },
            "environment_variables": [],
            "jobs": [
              {
                "name": "my_job",
                "run_instance_count": null,
                "timeout": 0,
                "environment_variables": [],
                "resources": [
                      "Linux",
                      "Java"
                ],
                "tasks": [
                  {
                    "type": "exec",
                    "run_if": [
                      "passed"
                    ],
                    "on_cancel": {
                      "type": "exec",
                      "attributes": {
                        "command": "ls",
                        "working_directory": null
                      }
                    },
                    "command": "sleep",
                    "arguments": [
                      "10"
                    ],
                    "working_directory": null
                  }
               ],
                "tabs": [
                  {
                    "name": "cobertura",
                    "path": "target/site/cobertura/*.xml"
                  }
                ],
                "artifacts": [
                  {
                    "source": "target",
                    "destination": "result",
                    "type": "build"
                  },
                  {
                    "source": "test",
                    "destination": "res1",
                    "type": "test"
                  }
                ],
                "properties": null
              }
            ]
          }
        ]
      }
    ],
    "environments" : [
      {
          "name" : "dev",
          "pipelines" : [ "pipeline1" ]  
      }
    ]
}
```