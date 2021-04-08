# Freddie
A lightweight, simple and crazy-idea server capable of executing workflows made with [KNIME Analyics Platform](https://www.knime.com/knime-analytics-platform).

## What/who is Freddie?

Freddie is a lightweight, simple and crazy-idea server capable of executing workflows created with KNIME Analytics Platform. It is designed to be minimalist, simple to use and free (as in free beer!) to anyone who does not have a few tens of thousand dollars for a real thing.

## What can it do?

For now, Freddie can execute KNIME workflows on the schedule and it can expose a REST endpoint of a workflow if so configured. 

The short list of features is as follows:
* execution of scheduled workflows
* triggering of a workflow with a REST call with passing of an arbitrary data in the JSON form
* fast execution of workflows by maintaining a pool of already running instances of KNIME platform
* executing a workflow on a specific version of KNIME platform by creating an execution environments

## Is Freddie a replacement for the [original KNIME server](https://www.knime.com/knime-server)?

Absolutely not! It was never meant to be one. The original KNIME server (henceforth known as the ‘[KNIME Server](https://www.knime.com/knime-server)’) has many more features and is more integrated into KNIME than Freddie. Besides, Freddie runs workflows as a batch job and KNIME Server uses additional plugins. Freddie is a separate product from the analytics platform and should be used as such. 

**TL;DR**: if you are a multimillion dollar company whose business success relies on the KNIME products you should certainly buy a KNIME Server because it is more stable, more feature packed and comes with support. 
If you are an individual who needs a simple KNIME executor and cannot afford a full KNIME Server, please give Freddie a try. 

## How to install it?

Freddie is in essence a Java servlet application. So it needs a Java application server to run. You can use any Java Servlet capable server but do note that Freddie was developed using Wildfly. I would recommend to use Wildfly to run Freddie before it is tested on other servers. 

Also, for the time being I would recommend to use Linux based OS - maybe CentOS or Ubuntu.

Steps to install Freddie:
* install Wildfly per available documentation
* create folders: `mkdir -p /srv/freddie/{workspace, repo, config}`
* make sure Wildfly has read/write accesa to these folders
* install KNIME Analytics Platform to arbitrary folder
* create configuration file
* test with a Test repository by copying it to the `repo` folder

## Documentation

This section describes various inner workings of Freddie and how you can use different features it provides. 

:exclamation: TODO!

## Known bugs

*Unknown bugs are a nuisance. Known bugs are a feature!*

* Sometimes when a KNWF file contains an empty folder not all files are extracted to disk. The consequence in that workflow execution fails with a log entry that a workflow folder could not be found. 

## Future development

* Full API for server management
* Webportal for executing workflows inside a browser
* Browser workflow repository (similar to KNIME Hub)
* Security (login, workflow public/private,...)
* Cluster execution (eg. running KNIME instances on more than one server) - don’t mix with workflow cluster execution (if you need parallel workflow node execution you need to buy a KNIME Server with [cluster execution](https://www.knime.com/knime-cluster-execution) capability - guys at KNIME can help you with that)
* Eclipse/KNIME plugin for Freddie
