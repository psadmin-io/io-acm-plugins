# psadmin.io ACM Plugins

This repository is a collection of custom ACM plug-ins. The PeopleCode is in plain text files can be copied into a custom `PTEM_CONFIG%` application package.

## Register the Plugin

1. Navigation to "PeopleTools > Automated Config Manager > ACM Utilities > Manage Plugins"
1. Enter the name of the ACM App Class and a description.
1. Click "Add to Group" and select (or create) an ACM Group to add the plugin to.
1. Click Save.

The plugin will be available on ACM templates.

## IOGatewayServiceURLs

This plug has 5 parameters:

* `gateway_host`: URL for Gateway
* `gateway_port`: 8000
* `gateway_ssl_port`: 8443
* `use_ssl_gateway`: false|true
* `default_local_node`: '%LocalNode'

## IOGatewayLBURLs

* `gateway_port`: 8000 # This is the local PIA HTTP port
* `gateway_ssl_port`: 443 # This is the Load Balancer HTTPS port
* `lb_gateway_url`: "%{hiera('dns_name')}"
* `use_ssl_gateway`: true
* `default_local_node`: "%{hiera('gateway_node_name')}"
* `enable_lb_gateway`: true|false
* `gateway_lb_host`: "%{::fqdn}" # This host will be added to the list of IB Gateways that are load balanced

## IOProcessSchedulerXCopyReportNode

This plugin has four parameters: 

* `distnodename`: The name of the Report Node
* `opsys`: (Not used)
* `winnetworkpath`: The file path to the Report Repository
* `url`: The URL for the `psreports` servlet

## IOTruncateTable

This plugin has one parameter: `tablelist`. `tablelist` takes a comma separated list of table names and will run `truncate table %table(&tablename)` for each table in the list.

## IOWebProfileAuthorizedSites

This plugin requires the following parameters:

* `webprofilename`:  The webprofile to add authorized sites
* `authorized_sites_count`: The number of authorized site entries
* `site1_authsiteproto`: 'H' for HTTP or 'S' for HTTPS
* `site1_authsitehostname`: The authorized site host
* `site1_authsiteportnum`: The authorized site port
* `site1_ptcorstype`: CORS Checkbox 'Y' or 'N'
* `site1_ptsecuritytype1`: Framable Checkbox 'Y' or 'N'
* `site1_ptsecuritytype2`: CheckToken Checkbox 'Y' or 'N'

Repeat `siteX_*` parameters for each authorized site entry (matching number in `authorized_sites_count`)

## IOProcessSchedulerDaemonConfig

This plugin requires the following parameters:

* `servername`:  Process Scheduler server to configure
* `daemonenabled`: Enabled? '0' or '1'
* `daemonsleeptime`: Number, sleep time
* `daemoncyclecnt`: Number, recycle count
* `daemonprcsinst`: Number, Daemon process instance
* `daemongroup`: String, Daemon process group


# Using the ACM Plugins in the DPK

Add the code as an AppClass to `PTEM_CONFIG` and you can reference the plugin like any delivered plugin.

```yaml
component_postboot_setup_list:
  integration_broker_lb:
    run_control_id:                       intbroker_lb
    os_user:                              "%{hiera('domain_user')}"

    db_settings:
      db_name:                            "%{hiera('db_name')}"
      db_type:                            "%{hiera('db_platform')}"
      db_opr_id:                          "%{hiera('db_user')}"
      db_opr_pwd:                         "%{hiera('db_user_pwd')}"
      db_connect_id:                      "%{hiera('db_connect_id')}"
      db_connect_pwd:                     "%{hiera('db_connect_pwd')}"

    acm_plugin_list:
      IOGatewayLBURLs:
        env.gateway_port:                 "%{hiera('pia_http_port')}"
        env.gateway_ssl_port:             443
        env.lb_gateway_url:               "%{hiera('dns_name')}"
        env.use_ssl_gateway:              true
        env.default_local_node:           "%{hiera('gateway_node_name')}"
        env.enable_lb_gateway:            true
        env.gateway_lb_host:              "%{::fqdn}"
  
    acm_plugin_order:
      - IOGatewayLBURLs 

component_postboot_setup_order:
  - integration_broker_lb
```
