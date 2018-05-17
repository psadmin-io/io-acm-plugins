# psadmin.io ACM Plugins

This repository is a collection of custom ACM plug-ins. The PeopleCode is in plain text files can be copied into a custom `PTEM_CONFIG%` application package.

## Register the Plugin

1. Navigation to "PeopleTools > Automated Config Manager > ACM Utilities > Manage Plugins"
1. Enter the name of the ACM App Class and a description.
1. Click "Add to Group" and select (or create) an ACM Group to add the plugin to.
1. Click Save.

The plugin will be available on ACM templates.

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

