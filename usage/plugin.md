# plugin

`tarifa plugin` wraps `cordova plugin` and edits the [`tarifa.json`](../project/index.md#tarifajson-and-privatejson) file.

```
Usage: tarifa plugin <cmd> [platform]

Add, remove or list cordova plugins in your project.

Available commands:

    add <plugin>
        Add a new plugin identified by <platform>
    remove <plugin>
        Remove the plugin with the name <plugin>
    list
        List installed plugins

Options:

    --help, -h     Show this message
    --verbose, -V  Be more verbose on everything

Examples:

    tarifa plugin add https://github.com/peutetre/cordova-plugin-hockeyapp.git#0.0.0
    tarifa plugin add org.apache.cordova.vibration
    tarifa plugin add ./relative/path/to/a/cordova/plugin

```
