---

copyright:
  years: 2016, 2018
lastupdated: "2018-10-9"
---
<!-- Copyright info at top of file: REQUIRED
    The copyright info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be surrounded by 3 dashes.
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    Indentation as per the previous template must be preserved.
-->

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Environment properties and resources
{: #deliverypipeline_environment}

The following properties and resources are available by default in pipeline environments.

<!--##Contents
* [Environment properties](#env)
    * [General purpose properties](#gen)
    * [Runtime and tool properties](#runtime)
    * [Deployment properties](#deployment)
* [Pre-installed resources](#resources)
    * [Runtimes and tools](#tools)
    * [Node modules](#node)-->

## Environment properties
{: #deliverypipeline_envprop}

### General-purpose properties

| Environment property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| ARCHIVE_DIR | The directory to archive or to download archives into. |
| BUILD_ID | The unique ID for the current job execution.  |
| BUILD_DISPLAY_NAME | The BUILD_ID value, prefixed with "#". |
| BUILD_NUMBER | The incremental stage ID that is shown in the pipeline UI.  |
| GIT_BRANCH | The Git branch that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| GIT_COMMIT | The Git commit that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| GIT_PREVIOUS_COMMIT | The Git commit value of the job's last successful run. This property is only available in jobs that use a Git repository as input. |
| GIT_URL | The Git repository URL that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| IDS_JOB_ID | The unique ID of the job's configuration. |
| IDS_JOB_NAME | The name of the job's configuration. |
| IDS_OUTPUT_PROPS | Comma-separated names of your stage environment properties. |
| IDS_PROJECT_NAME | The name of the project, for example, <code>Owner - Project Name</code>. |
| IDS_STAGE_NAME | The name of the current stage. |
| IDS_URL | The URL for the current pipeline. |
| IDS_VERSION | The number for the build that is being deployed or the SCM identifier. This property is available only in deploy jobs.
| JOB_NAME | The unique job ID in the context of the current pipeline. |
| PIPELINE_KUBERNETES_CLUSTER_NAME | The name of the Kubernetes cluster that is selected in the current job. |
| PIPELINE_STAGE_INPUT_JOB_ID | The ID of the job that is input for the current stage. |
| PIPELINE_STAGE_INPUT_REV | The revision of the input for the current stage. |
| PIPELINE_INITIAL_STAGE_EXECUTION_ID | The unique ID for the run of the pipeline. |
| PIPELINE_TRIGGERING_USER | The current user for the pipeline job|
| TASK_ID | The unique ID of the job's current run. |
| TMPDIR | A directory location where temporary files are stored. |
| WORKSPACE | The path for the current working directory. |

### Runtime and tool properties

| Environment property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| ANT_HOME | The path to Apache Ant 1.9.2. |
| ANT_JAVA8_HOME | The path to a 1.10+ version of Apache Ant that requires Java 8. |
| GRADLE_HOME | The path to Gradle 1.11. |
| JAVA_HOME | The path to IBM&reg; Java&trade; 7. |
| JAVA7_HOME | The path to IBM Java 7. |
| JAVA8_HOME | The path to IBM Java 8. |
| MAVEN_HOME | The path to Apache Maven 3.2.1. |
| NODE_HOME | The path to Node.js 0.10.29. |

To use Apache Ant 1.10+ in your pipeline's scripts, set `ANT_HOME` to `$ANT_JAVA8_HOME` and `JAVA_HOME` to `$JAVA8_HOME`.
{: tip}

### Deployment properties

| Environment property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| CF_APP | For deployments, the name of the app to deploy. This property is required for deployment and can be specified in the script itself, the deploy job configuration interface, or the project's `manifest.yml` file. |
| CF_ORG | For deployments, the name of the organization (org) to deploy to. |
| CF_ORGANIZATION_ID | For deployments, the ID of the org to deploy to. |
| CF_SPACE | For deployments, the name of the space to deploy to. |
| CF_SPACE_ID | For deployments, the ID of the space to deploy to.  |
| CF_TARGET_URL | For deployments, the URL of {{site.data.keyword.Bluemix_short}} or Cloud Foundry. |
| IDS_VERSION | For deployments, the version of the app that is being deployed or the source identifier. |

## Preinstalled resources
{: #deliverypipeline_resources}

Several runtimes, tools, and Node modules are preinstalled in every pipeline.

### Runtimes and tools

All links are in the home directory.
{: tip}

| Resource | Link name | Path |
|----------|-----------|-----------|
|Apache Ant 1.9.2|ant |/opt/IBM/ant |
|Cloud Foundry CLI 6.14 |cf | /opt/IBM/cf |
|Gradle 1.12|gradle |/opt/IBM/gradle |
|Gradle 2.9 |gradle2 |/opt/IBM/gradle2 |
|IBM Java (default)|java |/opt/IBM/java |
|IBM Java 7 x86_64-71 |java7 |/opt/IBM/java7 |
|IBM Java 8 x86_64-80|java8 |/opt/IBM/java8 |
|Apache Maven 3.2.1 |maven |/opt/IBM/maven |
|IBM Node |node |/opt/IBM/node |
|IBM Rational Team Concert&trade; SCM Tools |RTC-SCM-Tools |/opt/IBM/RTC-SCM-Tools |

The pipeline environment offers 64-bit versions of IBM Node 0.10, 0.10.48, 0.12, 0.12.17, 4.2, 4.4.5, 4.6.0, 6.2.2, and 6.7.0. To choose a version, use the export command.

For example, to use Node 0.12.7, type this command: `export PATH=/opt/IBM/node-v0.12/bin:$PATH`

To use Node 4.2.2, type this command: `export PATH=/opt/IBM/node-v4.2/bin:$PATH`

### Node modules

The following Node modules are globally installed in the pipeline environment:

* grunt@0.4.5
* grunt-cli@0.1.13
* grunt-contrib-concat@0.4.0
* grunt-contrib-jshint@0.10.0
* grunt-contrib-nodeunit@0.4.1
* grunt-contrib-qunit@0.5.1
* grunt-contrib-uglify@0.5.0
* grunt-contrib-watch@0.6.1
* karma@0.12.23
* karma-cli@0.0.4
* karma-jasmine@0.1.5
* karma-phantomjs-launcher@0.1.4
* phantomjs@1.9.10
