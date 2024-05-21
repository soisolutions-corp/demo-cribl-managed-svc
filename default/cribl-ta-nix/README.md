# Cribl Pack for Nix
----
The linux pack is designed to support the processing of linux OS data. It currently only support data being sent by a splunk universal forwarder. This pack includes sample logs for most of the inputs found in the Splunk TA for Nix, and includes three pipelines for processing said data. 

# Important Information
---
```
WARNING:
This pack will be incompatible with existing Splunk dashboards. Our recommendation is to either conver your searches to macros so that you only need to update one place, 
or create aliases to the new event format.

Please review various Splunk add-ons and configuration files such as props.conf or transforms.conf and make adjustments as necessary.
```

# Event Breaking Notes
---
During the creation of this pack I noticed that the 'PS' scripted event input can often overrun the default buffer for the standard cribl event breaker. Below is detailed instruction for setting up a new rule for PS events that should enable you to properly break these events.

WARNING: you need to be careful expanding buffer memory for input this large, try slowly stepping up the Maximum Bytes field with a sample log until you are able to capture the whole event.

## Create a new customer Event breaker
* Go the the knowledge tab in cribl and select 'Event Breaker Rules'
* Click '+ Add New' and name it 'Linux Scripted Events'
* Click "+ Add Rule" and name it 'PS - Break on Header'
* In the filter field use:  ```/^.+?(?=\n[^\r]\s+)/.test(_raw) && sourcetype=='ps'```
* Set the type to 'Regex' and set the regex field to:  ```^USER[\w\s]+ARGS\\n```
* Set the 'Maximum Event Bytes' field to:  ```1024000```    !!!!!!!! see the above warning !!!!!!!!
* Save the rule and navigate to the sources page
* Open the Splunk input that your linux events come in on
* In the input configuration page, select 'Event Breakers'
* Click '+ Add Ruleset' and select the 'Linux Script Events' ruleset we created
* Save the config, commit the code, and deploy it!

# What to Expect
---
DISCLAIMER:
The sample data set provided in the pack is the only data set tested against, your datasets will see a different reduction percentatge.

### [Splunk TA] linux scripted events: 
- BANDWIDTH - estimated at ~70% raw length reduction, ~28% full event reduction
- CPU - estimated at ~94% raw length reduction, ~71% full event reduction
- DF - estimated at ~93% raw length reduction, ~88% full event reduction
- HARDWARE - estimated at ~47% raw length reduction, ~35% full event reduction
- INTERFACES - estimated at ~77% raw length reduction, ~59% full event reduction
- IOSTAT - estimated at ~75% raw length reduction, ~71% full event reduction
- LASTLOG - estimated at ~64% raw length reduction, ~40% full event reduction
- LINUX:* - estimated at ~% raw length reduction, ~% full event reduction
- LSOF - estimated at ~97% raw length reduction, ~97% full event reduction
- NETSTAT - estimated at ~81% raw length reduction, ~79% full event reduction
- PACKAGE - estimated at ~82% raw length reduction, ~82% full event reduction
- PS - estimated at ~80% raw length reduction, ~80% full event reduction
- TIME - estimated at ~62% raw length reduction, ~52% full event reduction
- TOP - estimated at ~94% raw length reduction, ~94% full event reduction
- UNIX:* - estimated at ~% raw length reduction, ~% full event reduction
- VMSTAT - estimated at ~69% raw length reduction, ~52% full event reduction

### [Splunk TA] linux scripted metrics:
- TBD% reduction in the event size.


# Installation
---
## Setup
To use this Pack, follow these steps:
1. Create a Route with with a filter for your Splunk TA Nix events and metrics and system logs, (see section below for recommendations on filters)
2. Select the `Linux` pack as the pipeline.

## Filter Suggestions
below are a few suggestions on filtering data to send to the Linux Pack.
- strict matching for linux scripted events: 
- strict matching for linux scripted metrics:
- match against host pattern: ```if all hosts with a specific name pattern run linux, this could be very effective```
- match against index: (ie: index=='linux')```if you are routing all your linux data to a specific index, this could be very effective```

# Updates
---
You can find the most recent release at our repo [`releases page`](https://github.com/criblpacks/cribl-linux-events/releases) ~ link currently broken

## Release Notes

### Version 1.0.0 - 2022-3-22
- Rebuilt routes and all scripted event pipelines
- Significant performance improvements when processing events (most notably LSOF and PS events)
- Added IOSTAT.sh support
- Renamed pack to reflect that this pack specifically supports the Splunk Nix addon 
- Removed support for Linux filesystem logs
- default ouput is now CSV format (this increases data reduction dramatically)

### Version 0.2 - 2021-11-30
almost completed pipeline for linux scripted event inputs from the 'Splunk TA for nix' and a generic pipeline for processing of metrics.

### Version 0.1 - 2021-10-31
partial pipeline for linux scripted event inputs from the 'Splunk TA for nix'.

# Odd & Ends
---
## Contributing to the Pack
To contribute to the Pack, please do the following:

email the author at the email address listed below

## Contact
To contact us please email acain@cribl.io

## License
This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
