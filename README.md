# Sensu plugin for monitoring Chrony NTP

[![Sensu Bonsai Asset](https://img.shields.io/badge/Bonsai-Download%20Me-brightgreen.svg?colorB=89C967&logo=sensu)](https://bonsai.sensu.io/assets/sensu-plugins/sensu-plugins-chrony)
[![Build Status](https://travis-ci.org/sensu-plugins/sensu-plugins-chrony.svg?branch=master)](https://travis-ci.org/sensu-plugins/sensu-plugins-chrony)
[![Gem Version](https://badge.fury.io/rb/sensu-plugins-chrony.svg)](https://badge.fury.io/rb/sensu-plugins-chrony)
[![Code Climate](https://codeclimate.com/github/sensu-plugins/sensu-plugins-chrony/badges/gpa.svg)](https://codeclimate.com/github/sensu-plugins/sensu-plugins-chrony)
[![Test Coverage](https://codeclimate.com/github/sensu-plugins/sensu-plugins-chrony/badges/coverage.svg)](https://codeclimate.com/github/sensu-plugins/sensu-plugins-chrony)
[![Dependency Status](https://gemnasium.com/sensu-plugins/sensu-plugins-chrony.svg)](https://gemnasium.com/sensu-plugins/sensu-plugins-chrony)
[![Community Slack](https://slack.sensu.io/badge.svg)](https://slack.sensu.io/badge)

## Sensu Chrony Checks Plugin

- [Overview](#overview)
- [Files](#files)
- [Usage](#usage)
- [Sensu Asset](#sensu-asset)
- [Installation](#installation)
  - [Sensu Go](#sensu-go)
    - [Asset registration](#asset-registration)
    - [Check definition](#check-definition)
  - [Sensu Core](#sensu-core)
    - [Check definition](#check-definition)
- [Author](#author)

### Overiew

A sensu plugin to monitor Chrony NTP. There is also a metrics plugin for collecting things like offset, delay etc.

The plugin generates multiple OK/WARN/CRIT/UNKNOWN check events via the sensu client socket (https://sensuapp.org/docs/latest/clients#client-socket-input) so that you do not miss state changes when monitoring offset, stratum and status.

### Files

* bin/check-chrony.rb
* bin/metrics-chrony.rb

### Usage

The plugin accepts the following command line options:

```
Usage: check-chrony.rb (options)
    -c, --chronyc-cmd <PATH>         Path to chronyc executable (default: /usr/bin/chronyc)
        --crit-offset <OFFSET>       Critical if OFFSET exceeds current offset (ms)
        --crit-stratum <STRATUM>     Critical if STRATUM exceeds current stratum
        --dryrun                     Do not send events to sensu client socket
        --handlers <HANDLERS>        Comma separated list of handlers
        --warn-offset <OFFSET>       Warn if OFFSET exceeds current offset (ms)
        --warn-stratum <STRATUM>     Warn if STRATUM exceeds current stratum
```

Use the --handlers command line option to specify which handlers you want to use for the generated events.


## Sensu Asset  
  The Sensu assets packaged from this repository are built against the Sensu ruby runtime environment. When using these assets as part of a Sensu Go resource (check, mutator or handler), make sure you include the corresponding Sensu ruby runtime asset in the list of assets needed by the resource.  The current ruby-runtime assets can be found [here](https://bonsai.sensu.io/assets/sensu/sensu-ruby-runtime) in the [Bonsai Asset Index](bonsai.sensu.io).

## Installation

`sensuctl asset add jBRNDnl/sensu-plugins-chrony --rename jbrndnl/sensu-plugins-chrony`

### Sensu Go
#### Asset registration

Assets are the best way to make use of this plugin. If you're not using an asset, please consider doing so! If you're using sensuctl 5.13 or later, you can use the following command to add the asset: 

`sensuctl asset add jBRNDnl/sensu-plugins-chrony --renamd jbrndnl/sensu-plugins-chrony`

If you're using an earlier version of sensuctl, you can download the asset definition from [this project's Bonsai asset index page](https://bonsai.sensu.io/assets/jBRNDnl/sensu-plugins-chrony).

#### Check definition

```yaml
---
type: CheckConfig
api_version: core/v2
metadata:
    name: check-chrony
    namespace: default
    labels: null
    annotations: null
spec:
  check_hooks: null
  command: check-chrony.rb
  env_vars: null
  handlers: []
  interval: 60
  publish: true
  round_robin: false
  runtime_assets:
  - jbrndnl/sensu-plugins-chrony
  - sensu/sensu-ruby-runtime
  subscriptions:
  - linux
```

### Sensu Core
System-wide installation:

    $ gem install sensu-plugins-chrony

Embedded sensu installation:

    $ /opt/sensu/embedded/bin/gem install sensu-plugins-chrony

#### Check definition
```json
{
  "checks": {
    "check-chrony": {
      "command": "check-chrony.rb",
      "subscribers": ["ALL"],
      "interval": 60,
      "refresh": 60
    }
  }
}
```

## Author
Matteo Cerutti - <matteo.cerutti@hotmail.co.uk>
