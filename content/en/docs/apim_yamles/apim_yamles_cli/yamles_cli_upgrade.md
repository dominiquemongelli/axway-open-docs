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
* If you need to upgrade an old YAML configuration dated before May 2021, you can upgrade the XML federated configuration using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics/#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref/#projupgrade-command-options) and convert it again to YAML configuration using the latest YAML CLI [fed2yaml](/docs/apim_yamles/apim_yamles_cli/yamles_cli_convert/#convert-your-xml-configuration-to-a-yaml-configuration) or [frag2yaml](/docs/apim_yamles/apim_yamles_cli/yamles_cli_convert/#convert-your-xml-configuration-fragment-to-a-yaml-configuration-fragment).
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

{{< alert title="Note">}}
YAML configuration upgraded will be encrypted with the same passphrase as the source YAML configuration
{{< /alert >}}

**Example 5:**

* Specify the `.tar.gz` file for the YAML configuration to upgrade.
* Upgrade the YAML configuration with a non-default passphrase
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml-upgraded`.

```
./yamles upgrade --source yaml-to-upgrade.tar.gz --passphrase secret --output-dir /home/user/yaml-upgraded --targz yaml-upgraded.tar.gz
```

{{< alert title="Note">}}
YAML configuration upgraded will be encrypted with the same passphrase as the source YAML configuration
{{< /alert >}}

You can run the following help command for more details on each parameter:

```
yamles upgrade --help
```
