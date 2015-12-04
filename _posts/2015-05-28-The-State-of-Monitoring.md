---
layout: post
title: The State of Monitoring
tags: monitoring
comments: True
permalink: the-state-of-monitoring
---

System and Service Monitoring is something that I believe doesn’t get enough attention. Having visibility into the services you’re responsible for is an absolute must for any operations team. Finding out via a phone call or escalation that a critical service can certainly be a resume generating event. While some organizations have a dedicated team for such systems, most Sysadmins and Operations teams are left to figure this critical component out on their own.

The average monitoring software performs a check every few minutes, determines if the current state falls within a certain threshold, and then alerts someone to the change in status. I can hear you now – “Duh! What else would it do?” The problem with this archaic interpretation of monitoring is that thresholds are arbitrarily set and may not indicate an actual issue. A storage volume that was at 84% utilization yesterday, and is now 85% utilized today, is likely not a problem. Just because a service is running, doesn’t mean it’s functioning as expected. Why would I not want a database server to not use 90% of it’s memory? These are just some of the flaws in static threshold monitoring systems.


_So what’s the solution?_

There’s no one correct answer. There’s certainly a place for static threshold monitoring systems, and I’m not recommending you dismantle your Nagios installation just yet! Instead, augment your systems with more advanced monitoring methods.

##Functionality is king.

Start by reviewing the business requirements. Surely you don’t tell your CEO that you simply provide “Exchange” – you likely are required to provide the business with a service that allows for electronic communication to external and internal parties. Just because your Exchange services are running, doesn’t mean you’re actually providing that service.

Instead of obsessing over the state of a bunch of different services, instead augment your current monitoring system with custom scripts that send, receive and verify that e-mail is functioning as expected. You should be sure to have an alerting method that’s not e-mail based. After all, if e-mail is down, you may never know!

##Trends and Relative Thresholds

What does “normal” mean, exactly? It’s all relative. How do you know what the right state and condition of your services and servers are? Do you review performance data on a regular basis for each one? Wouldn’t it make sense that the file cluster that serves your roaming profiles  would be utilized more in the morning and evening when your users log on and off?

The threshold for alerting changes with context. A deviation from the norm is actually what you want to know about. If there’s a backup job that runs each night that kicks off at midnight, getting an alert for high disk I/O would generate unnecessary noise and perhaps diminish the value of alerts. In fact, my team may want to be alerted at that time if the disk I/O is too low – that means our backup job might have failed and gives us an opportunity to resolve the issue before the backup window closes.

Additionally, being able to detect a deviation from the norm when it comes to storage utilization is incredibly helpful. Obviously having a static threshold for this type of thing is a requirement, but combining it with a time-series based system (like OpenTSDB) is a killer setup. Being able to check average growth for an interval and comparing it to the last period makes it possible to detect a deviation from the norm.

My personal favorite, is [Bosun](http://bosun.org). Bosun is developed by the team at Stack Exchange (as well as many contributors) and is based on OpenTSDB. I won’t go too far into detail, as their site explains it pretty well.

##What we use

A combination of Nagios, Bosun+OpenTSDB, and a plethora of custom checks and monitors that verify intended functionality of various services.

We’re currently planning an implementation of [logstash](https://www.elastic.co/products/logstash) or [graylog](https://www.graylog.org/) to collect syslog and Event Viewer data for analysis and further alerting.


