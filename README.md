## Taking note of what I read in the Book Logging in Action - Phil Wilkins - Manning

I got around 3 - 4 days to setup the logging but have no idea what to log and simply know nothing about logging.

Define Goal

- Setup basic logging, answer the question ? what metrics to log
- have a rotate storage that last for 6 month and auto rotate without me having to delete it manually
- Have a way to display the metrics so it help with the debugging of the application or the performance analysis

If it take me more than 3 days for any of the above goal then I should probably spend more time on this and come back later.
In worst case let use SAAS like Sentry, Datadog...

4 main part

1. From Zero to "hello world" ( hope i can skim through since i know about docker and kubernetes)
2. Fluentd in depth ( Should read this a bit careful to understand what it offer and how it work )
3. Beyond the basics ( where thing get interested, the meat of it )
4. Good logging practices and frameworks to maximize log value (this may answer for the question of what **metrics** to save)

## Four Golden signals

~Observability~ was probably the first of the modern monitoring concepts to develop. Disccusstion around observability started to go mainstream around 2016
Observability essentially states that we should track or observe and mearsure what software is doing to manage and understand a system. Industry thinking has evolved this premise to the trakcing of four specific signals, often referred through
as the ~~four goldern signals~~ of SRE: latency, errors, traffic, saturation

What the signals mean:

- ~~Latency~~ -- How long it's taking to address a request. A growing latency indicates potential performance issues from the increasing deamand of need, or lack of perfomance tuning, for software or configuration.
- ~~Errors~~ -- Problems that can impact the service and the freqeuncy, and whether they are self-recovered (e.g., not getting a DB connection means fall back and try again).
- ~~Traffic~~ -- Increased traffic can indicate growing demand or malicious intent, depending on the gain or loss of effectivenss if traffic drops.
- ~~Saturation~~ -- Reflects how full or heavily used a system is (e.g., CPU and dis utilization). Once a system passes acertain saturation threshold, performance degradation will be epxeriencd as the operating system has to dedicate more effort to manage its limited resources.

### Three pillars of observability

The type of information gathered when monitoring can be described by one of several definitions. As a result, observability is made up of three pillars, or core ideas:

- ~~Metrids~~ -- Typically numerical and quantify the state of things. We then regularly sample the data points in the environement
- ~~Logs~~ -- Primarily texttual but event-based, there fore having characteristics of time and description
- ~~Traces~~ -- Tracking execution flows and the time it takes for transactions and sub-transactions to execute different steps. Trace logs are largely numerical, being made up of timestamps as code executions enter and leave different parts of the solution. To prodive these times with context, identifiers, such as transaciton ID and the entry and exit points, are identified.

- By measuring the data that each signal describes, the signal will indicate whether some-thing is right or wrong. More importantly, do the changes in the signals being received show a trend or pattern that at least means that the solution being monitored is not degrading anymore?
  I deally, we want a trend indicating continued improvement.

Regardless, this information will not give you information on the root problem. For example, signals showing a higly saturated system won't tell you why the system is saturated, which can occur if code is stuck in an infinite loop. For this, you still need to understand what the software is doing. This is not to say signals are wrong;
They are, without adoubt, the best way to provide a cue that there's an issue. But it is through the lens of the three pillars, a deeper appreciation of what is or isn't happening can be achieved with the sight of cause and effect in the way software is behaving.

### Log unification

Fluentd, Logstash, and other related tools are sometimes referred to as _log unification tools_. But what is meant by this, and what values(s) should a unification tool have?

_unification_ describes "the act or process of bringing together or combining things or people". This is what we use Fluentd for --collecting log evetns from diverse sources and bringing them together with a single tool so the log evetns can be processed and sent to the appropriate endpint solution(s)

This ability is essential, as it provides many significant benefits:

- It ease the task of locating and retrieving logs and log events. Through a single paltform, locating relevant log events becomes far easier. We can route the log evetns to a convenient location/tool, rather than needing to access multiple platforms with potentially many different locations and ways of accessing the log events.
- With virtualization, containerization, and more recently functions as a service, the hosting of logic becomes transient, so the means to easily gather log information before it is lost is more critical than ever. using Fluentd, we can configure lightweight processes into tehse transient environements that push log evetns to a drurable location.
- A single technology brings logs events together regardless of the source or target. As a result, log event management becomes eaier and more accessbile. we don't have to master how all the differen way s to log evetns can be captured and stored (e.g., Syslog, SNMP, Lof5J, and the many other log forms and protocols), as Fluentd makes this easier.
- Operating systems are complex, made up of many descrete processes and applicatoins. Often, discrete components come with their own logs. We need to bring tehse together to trae an event through the different components. Some of this has been solved with operating systems and network equipment adopting a small group of standards like syslog ans SNMP traps
- Applications pass events through many different managed devices, creating a real change in the number of places where our communications could be disrupted. Unifying the log evetns at this scale of distribution brings the problem to manageable proportions.

The log-based insights include the following:

- it is easier to create holistic view(s) of log evetns, allowing us to see the cause and effect more easily.
- With logs unified into an analytics platform, the data can be capitalized on with processes such as
  ..- Searching across the logs in one accessible location
  ..- Identifying trends and patterns in the production environment
  ..- Extracing analytical data enablind forecasting future likely behavior
  ..- Looking at user behavior to determine if the systems are subject to misuse or patterns of malicious actions

- A shift from reactive, post-event analysis approach to identifying issues and then proactively acting on them as they occur. This potentially can extend to a position where we identify warning signs and proactively perform actions to avoid a problem. The ability to become proactive comes from the unification tool's ability to filter, route, and apply meaning to log events.
- Infrastructure as a Service and Platform as a Service have brought whole new levels of dynamic change and routing complexity. As a result, the unifying of logs reduces the cale of the challenge of trakcing what coul dbe impacting our solution.

### Unifying logs vs. log analytics

**DEFINITION** _Log routing_ is when log evetns are taken and then directed through a middleware tool, such as Fluentd, to the applications that need those log events.
**DEFINITOIN** _Log aggregation_ means log events are taken and sent to a central location to be processed.

### ELK stack

The best-known stack within the software landscape for log processing is ELK (Elastic-search, Logstash, Kibana). The combination of products provides the ability to perform log analytics with ElasticSearch, sisualization through Kibana, and log Routing and aggregation with Logstash.

Fluentd compete with logstash in **Log aggregation/ unification**

#### Comparing Fluentd and Logstash

| Aspect                                     | Fluentd                                                                        | Logstash                                                        |
| ------------------------------------------ | ------------------------------------------------------------------------------ | --------------------------------------------------------------- |
| Primary contributor and product governance | Treasure Data governed by CNCF                                                 | Elastic                                                         |
| Commercially suported versions             | Yes                                                                            | Yes ( more robust options, as support can cover the full stack) |
| Plugins availabl                           | ~500                                                                           | ~200                                                            |
| Configuration style                        | Declarative -- use of tags                                                     | Procedural -- use of if-then-else constructs.                   |
| Performance                                | Comparatively (to Logstash) lower memory footprint                             | Comparatively (to Fluentd) higher memory                        |
| Caching                                    | Highgly configurable cache options with file and memory caching out of the box | In memory queue with a fixed size                               |
| Language / run-time machine                | CRUby -- no run time required for core                                         | JRudy with dependency on Java run time (JVM)                    |




### Log event life cycle

Another perspective worth considering is the life cycle of a log event. When a 
software componenet of some kind genrates a log entry, to et vaue from it, it needs 
to be passed through a life cycle

```
                                 .-------------------------------------------.
                                 | Information source capture                |  .
                                 | - Infrastructure such as CPU, memoryu use |   \
                                 | - JVM use                                 |    '
                                 | - App log files                           |    |
                                 | - SNMP traps                              |    |
                                 '-------------------------------------------'    '
                                                       |
                                                       v                        Fluentd's capabilities in      
                                 .-------------------------------------------.  this part of the life cycle    
                                 | Structure & route                         |  are part of its differentiator.
                                 | - Get the raw data to the appropriate     |
                                 | tooling in a format                       |    .
                                 | that can be processed                     |    |
                                 |                                           |    |
                                 '-------------------------------------------'    |
                                                       |                          |
                                                       v                          '
                                 .------------------------------------------.    /
                          .      | Aggregate & analyze                      |   /
                         /       | - Data from multiple sources             |  '
                        /        | - Merge in time series                   |
                       '         |                                          |
                       |         '------------------------------------------'
                       |                               |
                       '                               v
                                .---------------------------------------------.
FLuendt brings value in         | Visualize data                              |
this part of the life cycle     | - Search for log events                     |
by making it easy to feed       | - Present trends (e.g., memory consumption) |
information to tools            | - Rate of storage consumption               |
specializing in these areas.    |                                             |
                                '---------------------------------------------'
                       .                               |
                       |                               v
                       |       .----------------------------------------------.
                       '       | Notify & alert                               |
                        \      | - Push events into JIRA svc Desk, slack, etc |
                         \     | - Rules on severity dictate behaviors        |
                          '    |                                              |
                               '----------------------------------------------'
```

We start with capturing the log event (information source capture), and as the event
flows down, it gains more meaning and value. Based on what we're already discussed, 
any log unification too, including Fluentd, is most effective with the information surce capture and
the *structure and route* phases. The *agregate and analyze* phase will see features for analysis focusing on individual events but
will lean n agregating and analyzing aggregated logs. *Visualize data* is the product's weaket are. Given these tools' routing and connectivity capabilities, the *notify and alert* phase is easily realized by connecting suitable services. not only that, but there is also the potential for this pase to be moved upward, as we don't always need the analytics producst to decide whether it is necessary to notify and alert.


#### Plugins
Inputs
- File storage -- AWS s3, text files (HTTP log files, etc)
- Data(base) source -- MongoDB, MySQL, generic SQL (for all ANSI SQL DBs)
- Event sources -- AWS Kinesis, Kafka, AWS CLoudWatch, GCP Pub,Sub, RabbitMQ
- OS -- System, HTTP Endpoint, dstat, SNMP
- App servers -- ISS (Internet Information Services), WebSphere, Tomcat
Outputs
- File storage -- AWS S3, GOogle CLoud Storage, FIle
- Database storage -- BigQuery, MongoDB, InfluxDB, MySQL, SQL Server
- Event storage -- Kafka, Google Stackdriver, AWS CloudWatch, Prometheus
- Log analytics tools -- Splunk, Datadog, Elasticsearch
- Notifications -- Slack, Mai, HipChat, Twitter, Twilio, PagerDuty
Log/Event Manipulation (parsers, filters, and formatters)
- Map -- Log format mapper
- Numeric monitor -- Generates stas relating to logs
- Text to Json
- Key/Value Parsing
- GeoIP -- Translating IP adddresses to geographis location based on publichsed information 
- JWT
- Redaction -- the masking oif data so sensitive data values can't be seen by those not authorized to do so
- Formatters -- the means to lay out the data into different structures and potentially 
  different notations (e.g., XML to JSON)

Storage
- Caching -- Redis, Memcached
- Persisten storage -- Local file, SQL databases, S3 Block storage
- Service Discovery -- Configuration to find other nodes that understand Fluentd's comms mechanisms

#### How fluendt can be used to make operational tasks easier
- Actinable log evetns
- Making logs more meaningful
- Polyglot environments
- Multiple targets
- Controlling log data costs
- Logs to metrics
- Rapid operational consolidation

 



## Concepts, architecture and deployemnt of fluentd

### Architecture and core concepts
#### The makeup of a log event

Each log event is managed as a single JSON object comprised of three mandatory, nonrepeating 
elements

- *Tag* -- Each log event has a tag associated with it. The tags are typicall linked to the source initially through the configuration but can be susequently manipulated within the configuration. FLuentd can apply condidiontal operations (routing, filtering, etc.) to the log events as necessary by using the tags. WHen using the HTTP interface, the tag can be defined in teh call.
- *Timesatamp* - This is deriv3ed from the log information or is applied by the input plugin. This ensures that the ev3etns are kept in series, an essential considerration when unifying multiple log sources and potentially trying to undersatnd the sequence of events across coponets. This data is held as nanose conds from epoch
- *Record* -- The record is the core event information after separating out the time. This means we can address the log content without worrying about locating the timestamp for the events and the tag needed for basic controls.
