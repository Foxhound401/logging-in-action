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
4. Good logging practices and frameworks to maximize log value  (this may answer for the question of what **metrics** to save)


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


