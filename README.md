opsworks-shoryuken
================

Opsworks shoryuken cookbook for Ubuntu and Rails or non-rails shoryuken deploys

## Installation instructions

1) Add this cookbook to your list of Custom Cookbooks

2) Add the deploy recipe in this cookbook to your Application's Deploy custom recipe.  This should be place AFTER your application is deployed to ensure shoryuken uses the new code checked out.

3) Configure your shoryuken custom JSON to specify shoryuken should be deployed with this app:


### Supported Options

Currently supported options for the shoryuken deploy recipe are:

* start_command

The command to start shoryuken.  This will run relative to the root of the current release path.

Defaults to:

```bash
bundle exec shoryuken -R -C config/shoryuken.yml 2>&1 >> log/shoryuken.log
```

### Sample Chef JSON configuration

Opsworks will underscore and lowercase your application name. "My Application" becomes "my_application". In the JSON below you **must** provide the converted application name, otherwise the service file (shoryuken.conf) will not be correctly configured.

Here is the minimum deploy config required:
```json
{
  "deploy": {
    "your_app_name": {
      "shoryuken": {}
    }
  }
}
```

Here is an example Custom JSON which overrides overrides the start_command to not load the Rails application and read workers from a boot.rb file: 

```json
{
  "deploy": {
    "your_app_name": {
      "shoryuken": {
        "start_command": "bundle exec shoryuken -C config/shoryuken.yml -r ./config/boot.rb 2>&1 >> log/shoryuken.log"
      }
    }
  }
}
```

### Environment variables

All Opsworks environment_variables defined within your application will be exposed to the shoryuken process via the upstart script.
