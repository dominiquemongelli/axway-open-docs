{
"title": "Upgrade YAML configuration using CLI",
"linkTitle": "Upgrade YAML configuration using CLI",
"weight":"80",
"date": "2021-04-15",
"description": "Learn how to use the YAML configuration CLI to upgrade a YAML configuration."
}

The `upgrade` option may be used to upgrade YAML configurations. Both full configurations, and configuration fragments may be upgraded in the same way.

{{% alert color="warning" title="Upgrade Caution" %}}
* YAML configurations upgrade is supported starting with version 7.7 May 2021 
* If you need to upgrade an old YAML configuration dated before May 2021, you can upgrade the XML federated configuration using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics/#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref/) and convert it again to YAML configuration using the latest YAML CLI [fed2yaml](/docs/apim_yamles/apim_yamles_cli/yamles_cli_convert/#convert-your-xml-configuration-to-a-yaml-configuration) or [frag2yaml](/docs/apim_yamles/apim_yamles_cli/yamles_cli_convert/#convert-your-xml-configuration-fragment-to-a-yaml-configuration-fragment).
{{% /alert %}}

## Upgrade a YAML configuration

The `upgrade` option has the same goal as the [projupgrade](/docs/apim_reference/devopstools_ref/) tool existing for XML federated configurations. It enables you to upgrade YAML configurations or YAML configuration fragments from earlier versions (7.7 May2021 and later) to the latest. 
The `upgrade` option is the only entry point available to upgrade YAML configuration. 

**Example 1:**

* Specify the directory for the YAML configuration to upgrade.
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml-upgraded`.

```
./yamles upgrade --source /home/user/yaml --output-dir /home/user/yaml-upgraded --targz yaml-upgraded.tar.gz
```

**Example 2:**

* Specify the URL for the YAML configuration to upgrade.
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.

```
./yamles upgrade --source yaml:file:///home/user/yaml --output-dir /home/user/yaml-upgraded
```

**Example 3:**

* Specify the `.tar.gz` file for the YAML configuration to upgrade.
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml-upgraded`.

```
./yamles upgrade --source yaml-to-upgrade.tar.gz --output-dir /home/user/yaml-upgraded --targz yaml-upgraded.tar.gz
```

**Example 4:**

* Specify the `.tar.gz` file for the YAML configuration to upgrade.
* Upgrade the YAML configuration with a non-default passphrase   
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml-upgraded`.

```
./yamles upgrade --source yaml-to-upgrade.tar.gz --passphrase secret --output-dir /home/user/yaml-upgraded --targz yaml-upgraded.tar.gz
```

{{% alert color="note" title="Upgrade passphrase" %}}
YAML configuration upgraded will be encrypted with the same passphrase than the non-upgraded YAML configuration
{{% /alert %}}

**Example 5:**

* Specify the `.tar.gz` file for the YAML configuration to upgrade.
* Upgrade the YAML configuration with a non-default passphrase
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml-upgraded`.

```
./yamles upgrade --source yaml-to-upgrade.tar.gz --passphrase secret --output-dir /home/user/yaml-upgraded --targz yaml-upgraded.tar.gz
```

{{% alert color="note" title="Upgrade passphrase" %}}
YAML configuration upgraded will be encrypted with the same passphrase than the non-upgraded YAML configuration
{{% /alert %}}

You can run the following help command for more details on each parameter:

```
yamles upgrade --help
```


### Disable entity reference check

In some cases, you might have a valid configuration that points to an entity that exists in another configuration. This is the case for [Team Development](/docs/apim_yamles/apim_yamles_references/yamles_team_development) when you want to keep common configuration in a separate project for reuse purposes. You might need to validate each configuration separately before merging without generating errors for fields that point to entities that exist elsewhere. This might also occur for configuration fragments if they were generated without full closure, which means entities referred to by selected entities are not included in the fragment. Use the `--allow-invalid-ref` option to ensure that an error is not generated in this case. With this parameter enabled, warnings are traced but no error is generated.

To learn how to solve validation errors, see [known validation errors](/docs/apim_yamles/apim_yamles_references/yamles_known_validation_errors). Note that errors are shown on stdout. Warnings will only be listed on stdout if there are other errors and if only warnings are found, they are listed in the trace file only.
