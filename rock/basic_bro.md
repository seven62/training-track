<!-- time marker 49:00 -->

# Bro Class


## Overview

**What is it?**

* Protocol Analyzer

* Language - turing complete
  * programming
  * scripting

* Modes
  * always watching
  * feed it `.pcap`

* drone feed

**What is it *not*?**

* _*not*_ an alert engine
* _*not*_ for signature matching


## Core

* Event Engine
  * decribes what has been seen
  * does not assess for malicious traffic

* Scripting Language
  * out-of-the-box scripts
  * event engine output to consumable logs

* ASCII Logs
  * default output format
  * easily manipulated
  * small in comparison

## Generating Logs

1. listen on an interface: `bro -i eth0 local`
1. analyze a PCAP file: `bro -r somefile.pcap local`

## Class Setup

> PE time: replay pcap file and extract "wifi password"
> students log into islet instance / general setup


## Bro Structure / Internals

* install path varies between OS and packages. for us: `/opt/bro/`

```
bro/bin - binaries
bro/etc - config files
bro/lib - specific libraries needed to run
bro/share - scripts / policies / customizations
bro/spool - current batch of log files
bro/logs -  archival log area, rotated out to grows over time
```
#### etc/

* node.cfg - controls how Bro operates in standalone mode

* networks.cfg - network ranges / cidr loaded here to match response env / determine internal and external traffic

* broctl.cfg - determines behavior when run consistantly (longterm)

#### ./broctl

* `help`
* `check` - verifies configuration before starting
* `install` - pushes to workers
* `start` -
* `cleanup --all` - restore to cleanness
* `deploy` - shortcut to check /start / etc

These commands can be run by entering the "broctl" shell:

```
sudo broctl
>
```

Or run straight from the command line: `sudo /opt/bro/bin/broctl status`



















## Extending Bro



## Hunting Exercises



## Rock Demo



## Home Lab
