If you are looking for a particular plugin, go directly [to the dedicated section](https://pydio.com/docs/references/plugins/).

### Introduction
Pydio comes fully loaded with various plugins to ease the integration with other systems, help you develop your own specific installation, or even use Pydio as a very generic explorer-like GUI, for browsing any type of data (see access.ajxp_conf and access.mysql for example).

Plugins can be seen as “features”. They come packed with their own actions and parameters, and can drastically change the way Pydio works.  A plugin is composed of a folder “plugintype.pluginname” containing at least an XML file describing the plugin, the **manifest.xml**. When you want to customize, duplicate, understand a plugin, a good start is always to look at the manifest.xml.

    <?xml version="1.0" encoding="UTF-8"?>
    <logdriver name="text" label="CONF_MESSAGE[Text logger]" description="CONF_MESSAGE[Stores the logs as readable tab delimited text.]" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="file:../core.ajaxplorer/ajxp_registry.xsd">
        <class_definition filename="plugins/log.text/class.textLogDriver.php" classname="textLogDriver"/>
        <client_settings>
            <resources>
                <i18n namespace="text_logger" path="plugins/log.text/i18n"/>
            </resources>
        </client_settings>
        <server_settings>
            <param name="LOG_PATH" type="string" label="CONF_MESSAGE[Logpath]" description="CONF_MESSAGE[The path to where the logs are kept.]" mandatory="true"/>
            <param name="LOG_FILE_NAME" type="string" label="CONF_MESSAGE[Logfile]" description="CONF_MESSAGE[The name of the log file]" mandatory="true"/>
            <param name="LOG_CHMOD" type="integer" label="CONF_MESSAGE[Files Permissions]" description="CONF_MESSAGE[]" default="0770" mandatory="true"/>
        </server_settings>
    </logdriver>

The manifest.xml defines all the parameters a plugin can handle. Parameters can be “global” (i.e. shared by all instances of this plugin) or “local” (specific for an instance). This apply to plugins that are linked to a workspace : the “local”  or “instance” parameters can be different from one workspace to another. It can also import options from the Pydio **mixins**, that allow the factorization of common behaviors across plugins. All these parameters are self-documented, and the “Plugins” section of this site gives you a description of each parameter.

The **core* plugin type is a specific type that also factorize behaviors : given any “ptype” plugin type, if there is a “core.ptype” plugin, it will be automatically loaded and the parameters will be added to any “ptype.pname” plugin. For example, all “auth” plugins inherits from the “core.auth” plugin. At the top of this is the **core.ajaxplorer** that is untouchable, and comes bundled with various basic resources.

### Troubleshooting plugins : the cache
As all plugins are loaded dynamically to construct the full state of the application, and particularly under the form of a big XML registry, it was quite soon necessary to introduce a caching mechanism that improved performances drastically. However, there is no automatic clearing of the cache and one is very often faced to the fact that nothing is changing although a plugin has been modified.

Most of the time, you’ll have to simply CLEAR THE SERVER CACHE : simply remove the two files **_/data/cache/plugins_requires.ser_**  and **_/data/cache/plugins_cache.ser_**. Another way to clear the cache without accessing the server directly is to edit any plugin configuration via the GUI (for example disable then re-enable a plugin) : the cache is automatically cleared at this moment.

You can use he following commands to clear the cache : `rm -f plugins_*`.
Or you can use the Refresh cache button located at the top in **All Plugins > Available Plugins**
### Understanding plugins lifecycle
By default, all plugin have the following lifecycle :

**Detected** by the application (folder has been dropped in the plugins folder) => **Special checks performed** (if wrong, cannot be activated) => **Enabled** (i.e ready for use) => **“Activated”**, either through the GUI (example editor.diaporama), through specific configuration files (exemple auth.serial), or through the activation of a workspace that explicitly makes use of this plugin (example meta.exif), see next section.

Plugins can define dependencies to other plugins, making them inactive if the given dependencies cannot be found. When troubleshooting a plugin, fist thing is to check it’s defined in the ACTIVE_PLUGINS of the config file, next it can be interesting to verify that all these dependancies are indeed active. Again, see the dependencies in the manifest.xml.For example, the editor.codepress plugin is a source code editor, that is based on the editor.text plugin. In the manifest file, you can see

    <dependencies>
           <pluginResources pluginName="editor.text"/>
           <!-- Stream Wrapper Access -->
          <activePlugin pluginName="access.fs|access.ftp|access.demo|access.remote_fs"/>
    </dependencies>

This shows that the plugin needs the editor.text resources to be loaded (so the editor.text must be indeed available), and also that this plugin will be active only if one of the defined access plugin is also active. Concretely, this means that editor.codepress is active when the current workspace is an access.fs workspace, but not when the current workspace is based on access.mysql for example. Which is logical, as editor.codepress can only handle a given type of data, a file.
