
# check_space 

Source: [check_space.cf](/services/check_space.cf)

This bundles check the given filesystems if there is enought freespace. When there is
not enough freespace will:
 * report it
 * run an specified command, if set
 * run an specified bundles, if set
 * set a class bases on the directories: `canonify("check_space_$(path))")`

## Usage

The bundle can be run via:
 * `def.scl_services_enabled`
```json
"vars": {
    "scl_services_enabled": [
            "...",
            "check_space",
            "..."
    ]
}
```

The bundle will aways read the [default.json](/templates/check_space/json/default.json) file
and extra json file(s) can be specified via:
 * def.cf
```
vars:
    any::
        "check_space_json_files" slist => { "tmp.json" };
```

The variable must be `check_space_json_files` and with this setup 1 extra json file will be  merged.

### Debug

If you want to debug this bundle set the `DEBUG_check_space` class, eg:
    * `-DDEBUG_check_space`

## def.cf/json

The `default.json` is an empty file. The structure is as follows:
```json
"path": {
    "freespace": "Absolute or percentage minimum disk space that should be available before",
    "command": "If specified then run this command if condition is met",
    "run_bundle": "If specified then run this bundle if condition is met"
}
```

You can specfy more then one path, The example below will
check:
  1.  `/tnp` and run a command if it is low on space:
  1.  `/boot` and run a bundle if it is low on space:
```json
{
    "/tmp": { "freespace": "99%", "command": "/bin/echo hello check_space"  },
    "/boot": { "freespace": "98%", "run_bundle": "check_space_run_bundle_test" }
}
```

### Some examples

Here is an example how to use it in:
 * def.cf
```
vars:
    "check_space" data => parsejson('{
        "json_files": [ "tmp.json" ],
        "/scratch": { "freespace": "10%" }
    }');
```

 * def.json
```json
"vars": {
    "check_space": {
        "json_fles": [ "tmp.json" ],
        "/scratch": { "freespace": "5%" }
    }
}
```
