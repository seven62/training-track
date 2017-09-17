# Bro Basic


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

> PE time: replay pcap file and extract "wifi password"

listen on an interface: `bro -i eth0 local`

analyze a PCAP file: `bro -r somefile.pcap local`


## Structure

/bro/etc
