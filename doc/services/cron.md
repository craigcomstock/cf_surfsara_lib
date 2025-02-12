
# cron

Source: [cron.cf](/services/cron.cf)

This bundle will generate these/this file(s) from mustache templates:
 * /etc/cron.allow
 * /etc/cron.deny

If one of the files is changed then the following ""class"" will be set:
 * scl_etc_cron_allow
 * scl_etc_cron_deny

These templates are located in:
 * templates/cron
 * templates/cron/json

## Usage

The bundle can be run via:
 * `def.scl_services_enabled` 
```json
"vars": {
    "scl_services_enabled": [
            "...",
            "cron",
            "..."
    ]
}
```

The bundle will always read the [default.json](/templates/cron/json/default.json) file
and extra json file(s) can be specified via:
 * def.cf
```
vars:
    any::
        "cron_json_files" slist => { "sys_users.json" };
```

The variable must be `cron_json_files` and with this setup 1 extra json file will be  merged.

###  DEBUG 

If you want to debug this bundle set the `DEBUG_cron` class, eg:
 * `-DDEBUG_cron`

## def.cf/json

See [default.json](/templates/cron/json/default.json) what the default values are and
which variables can be overriden.

The following must be set in host specific host file: `def.json`
```json
"vars": {
    "cron": {
        "allow_users": [ "root", "bas", "jaap" ],
        "deny_users": [ "remy"  ]
    }
}
```
