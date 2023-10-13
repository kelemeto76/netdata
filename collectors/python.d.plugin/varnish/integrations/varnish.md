<!--startmeta
custom_edit_url: "https://github.com/netdata/netdata/edit/master/collectors/python.d.plugin/varnish/README.md"
meta_yaml: "https://github.com/netdata/netdata/edit/master/collectors/python.d.plugin/varnish/metadata.yaml"
sidebar_label: "Varnish"
learn_status: "Published"
learn_rel_path: "Data Collection/Web Servers and Web Proxies"
message: "DO NOT EDIT THIS FILE DIRECTLY, IT IS GENERATED BY THE COLLECTOR'S metadata.yaml FILE"
endmeta-->

# Varnish


<img src="https://netdata.cloud/img/varnish.svg" width="150"/>


Plugin: python.d.plugin
Module: varnish

<img src="https://img.shields.io/badge/maintained%20by-Netdata-%2300ab44" />

## Overview

This collector monitors Varnish metrics about HTTP accelerator global, Backends (VBE) and Storages (SMF, SMA, MSE) statistics.

Note that both, Varnish-Cache (free and open source) and Varnish-Plus (Commercial/Enterprise version), are supported.


It uses the `varnishstat` tool in order to collect the metrics.


This collector is supported on all platforms.

This collector only supports collecting metrics from a single instance of this integration.

`netdata` user must be a member of the `varnish` group.


### Default Behavior

#### Auto-Detection

By default, if the permissions are satisfied, the `varnishstat` tool will be executed on the host.

#### Limits

The default configuration for this integration does not impose any limits on data collection.

#### Performance Impact

The default configuration for this integration is not expected to impose a significant performance impact on the system.


## Metrics

Metrics grouped by *scope*.

The scope defines the instance that the metric belongs to. An instance is uniquely identified by a set of labels.



### Per Varnish instance

These metrics refer to the entire monitored application.

This scope has no labels.

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| varnish.session_connection | accepted, dropped | connections/s |
| varnish.client_requests | received | requests/s |
| varnish.all_time_hit_rate | hit, miss, hitpass | percentage |
| varnish.current_poll_hit_rate | hit, miss, hitpass | percentage |
| varnish.cached_objects_expired | objects | expired/s |
| varnish.cached_objects_nuked | objects | nuked/s |
| varnish.threads_total | None | number |
| varnish.threads_statistics | created, failed, limited | threads/s |
| varnish.threads_queue_len | in queue | requests |
| varnish.backend_connections | successful, unhealthy, reused, closed, recycled, failed | connections/s |
| varnish.backend_requests | sent | requests/s |
| varnish.esi_statistics | errors, warnings | problems/s |
| varnish.memory_usage | free, allocated | MiB |
| varnish.uptime | uptime | seconds |

### Per Backend



This scope has no labels.

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| varnish.backend | header, body | kilobits/s |

### Per Storage



This scope has no labels.

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| varnish.storage_usage | free, allocated | KiB |
| varnish.storage_alloc_objs | allocated | objects |



## Alerts

There are no alerts configured by default for this integration.


## Setup

### Prerequisites

#### Provide the necessary permissions

In order for the collector to work, you need to add the `netdata` user to the `varnish` user group, so that it can execute the `varnishstat` tool:

```
usermod -aG varnish netdata
```



### Configuration

#### File

The configuration file name for this integration is `python.d/varnish.conf`.


You can edit the configuration file using the `edit-config` script from the
Netdata [config directory](https://github.com/netdata/netdata/blob/master/docs/configure/nodes.md#the-netdata-config-directory).

```bash
cd /etc/netdata 2>/dev/null || cd /opt/netdata/etc/netdata
sudo ./edit-config python.d/varnish.conf
```
#### Options

There are 2 sections:

* Global variables
* One or more JOBS that can define multiple different instances to monitor.

The following options can be defined globally: priority, penalty, autodetection_retry, update_every, but can also be defined per JOB to override the global values.

Additionally, the following collapsed table contains all the options that can be configured inside a JOB definition.

Every configuration JOB starts with a `job_name` value which will appear in the dashboard, unless a `name` parameter is specified.


<details><summary>Config options</summary>

| Name | Description | Default | Required |
|:----|:-----------|:-------|:--------:|
| instance_name | the name of the varnishd instance to get logs from. If not specified, the local host name is used. |  | True |
| update_every | Sets the default data collection frequency. | 10 | False |
| priority | Controls the order of charts at the netdata dashboard. | 60000 | False |
| autodetection_retry | Sets the job re-check interval in seconds. | 0 | False |
| penalty | Indicates whether to apply penalty to update_every in case of failures. | yes | False |
| name | Job name. This value will overwrite the `job_name` value. JOBS with the same name are mutually exclusive. Only one of them will be allowed running at any time. This allows autodetection to try several alternatives and pick the one that works. |  | False |

</details>

#### Examples

##### Basic

An example configuration.

```yaml
job_name:
  instance_name: '<name-of-varnishd-instance>'

```


## Troubleshooting

### Debug Mode

To troubleshoot issues with the `varnish` collector, run the `python.d.plugin` with the debug option enabled. The output
should give you clues as to why the collector isn't working.

- Navigate to the `plugins.d` directory, usually at `/usr/libexec/netdata/plugins.d/`. If that's not the case on
  your system, open `netdata.conf` and look for the `plugins` setting under `[directories]`.

  ```bash
  cd /usr/libexec/netdata/plugins.d/
  ```

- Switch to the `netdata` user.

  ```bash
  sudo -u netdata -s
  ```

- Run the `python.d.plugin` to debug the collector:

  ```bash
  ./python.d.plugin varnish debug trace
  ```

