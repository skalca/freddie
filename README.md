# Freddie
A lightweight, simple and crazy-idea server capable of executing workflows made with [KNIME Analytics Platform](https://www.knime.com/knime-analytics-platform).

## What/who is Freddie?

Freddie is a lightweight, simple and crazy-idea server capable of executing workflows created with KNIME Analytics Platform. It is designed to be minimalist, simple to use and free (as in free beer!) to anyone who does not have a few tens of thousand dollars for a real thing (the real thing being the [original KNIME server](https://www.knime.com/knime-server)).

## What can it do?

For now, Freddie can execute KNIME workflows on the schedule and it can expose a REST endpoint of a workflow if so configured. 

The short list of features is as follows:
* execution of scheduled workflows
* triggering of a workflow with a REST call with passing of an arbitrary data in the JSON form
* fast execution of workflows by maintaining a pool of already running instances of KNIME platform
* executing a workflow on a specific version of KNIME platform by creating an execution environments

## Is Freddie a replacement for the original KNIME Server?

Absolutely not! It was never meant to be one. The original KNIME server (henceforth known as the ‘KNIME Server’) has many more features and is more integrated into KNIME than Freddie. Besides, Freddie runs workflows as a batch job and KNIME Server uses additional plugins. Freddie is a separate product from the analytics platform and should be used as such. 

**TL;DR**: if you are a multimillion dollar company whose business success relies on the KNIME products you should certainly buy a KNIME Server because it is more stable, more feature packed and comes with support. 
If you are an individual who needs a simple KNIME executor and cannot afford a full KNIME Server, please give Freddie a try. 

## How to install it?

Freddie is in essence a Java servlet application. So it needs a Java application server to run. You can use any Java Servlet capable server but do note that Freddie was developed using Wildfly. I would recommend to use Wildfly to run Freddie before it is tested on other servers. 

Also, for the time being I would recommend to use Linux based OS - maybe CentOS or Ubuntu.

Steps to install Freddie:
* install [OpenJDK-8](https://adoptopenjdk.net/?variant=openjdk8&jvmVariant=hotspot)
* install [Wildfly Application Server](https://www.wildfly.org/) per available documentation
* create folders: `mkdir -p /srv/freddie/{workspace,repo,repo/env,repo/env,config,logs,logs/jobs}`
* make sure `wildfly` user has read/write access to `workspace` and `logs` folders, and read-only access to `repo` and `config` folders
* install KNIME Analytics Platform to arbitrary folder (usually under `/opt/knime/knime_<version>`)
* create configuration file `config/config.json` as per instructions below
* test with a Test repository by copying it to the `repo` folder

## Documentation

This section describes various inner workings of Freddie and how you can use different features it provides. 

### Creating configuration file

Freddie expects to find a configuration file in the `config` folder named `config.json`. Its struture is as follows.

```javascript
{
  "envs" : [
    {
      "name" : "default",
      "exec" : "/opt/knime/knime_X.Y.Z/knime",
      "base_wf_dir" : "env/default",
      "prefs" : "<path-to-preferences.epf-file>"
    }
  ],
  "pool.init" : 1
  "pool.max"  : 0,
  "pool.runtime" : 30,
  "pool.wait" : 2,
  "sched.runtime" : 60,
  "sched.max" : 10
}
```

Configuration file can contain multiple *objects* under `envs` element. This element describes each KNIME execution environment and where to find it. 

#### Element `envs`

Element name | Description | Example value
------------ | ------------ | ------------
name | Arbitrary name of an environment | default
exec | Path to KNIME executable | /opt/knime/knime_4.1.3/knime
base_wf_dir | Path to this environment's base directory under `repo` folder | env/default
prefs | Path to 'preferences' file for KNIME Analytics Platform used on each run | null, empty or *path*

#### Other elements

Element name | Description | Example value
------------ | ------------ | ------------
pool.init | Number of instances that are fired up for each environment | 1
pool.max | Number of instances that can be running at the same time for each environment | 5
pool.runtime | Number of seconds the routing instance can execute before being killed | 30
pool.wait | Number of seconds to wait for a free routing instance before failing | 2
sched.runtime | Number of seconds a scheduled instance can rune before being killed | 3600
sched.max | Maximum number of scheduled instances running at the same time | 10

### Creating environments

!! TODO:   env/default,  copy freddie_base_wf.knwf file,   name environment

## Known bugs

*Unknown bugs are a nuisance. Known bugs are a feature!*

* Sometimes when a KNWF file contains an empty folder not all files are extracted to disk. The consequence is that workflow execution fails with a log entry that a workflow folder could not be found. 

## Future development

*Something to think about during the night*

* Full API for server management
* Webportal for executing workflows inside a browser
* Browser workflow repository (similar to KNIME Hub)
* Security (login, workflow public/private,...)
* Cluster execution (eg. running KNIME instances on more than one server) - don’t mix with workflow cluster execution (if you need parallel workflow node execution you need to buy a KNIME Server with [cluster execution](https://www.knime.com/knime-cluster-execution) capability - guys at KNIME can help you with that)
* Eclipse/KNIME plugin for Freddie

## Open-source, not open-contribution

Freddie is open source but closed to code contributions. I am grateful for community involvement, bug reports & feature requests. 
