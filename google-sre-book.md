---
title:  Google SRE book notes
tags:
  - reading
  - sre
  - devops
---

# Site Reliability Enginerering

* Site: Keep the entire ecosystem running
* Reliability: USer swant to use something, if they can't use it, they wont stay
* Engineering: Apply engineering principles to solve problems

## What does an SRE do?

SRE spend 50% in operations and 50% in development (rule to strive for). As an SRE grows, more time
sh ould be able to be spend in development, because systems are more **automatic** then manual
work.

There is an important difference between automatic en automated systems:

* automated: There is still a level of human interaction needed.
* automatic: No humna interaction is needed, only human oversight is required

## What tools does an SRE have?

### Error budget

100% reliability is an impossible goal. The end user does not know the difference between 100% en
99.9999% reliability. There are too many parts between the product and them to notice this. This
gives SRE's a certain margin of error to work with, the so called error budget.

This error budget gives SRE's the space to work with. This budget can be spend in whatever way
needed, as long as it is not overspend. Overspending in the error budget, causes problems for end
users.

### Monitoring

Monitoring is one of the primary tools that an SRE uses. There are 3 kinds of monitoring output:

* Logging: Only used for diagnostics and forensic purposes only.
* Tickets: Human action is needed, but not urgently. Days can pass.
* Alerting: Human action is needed immediately.

### Emergency response

2 metrics are introduced here. Mean Time To Failuire (MTTF) and Mean Time To Repair (MTTR).
The goal in emergency response is to keep the MTTR as low as possible.
This can be done by preparing actions needed to be taken in a playbook.

### Change management

Outages occur mostly when changes in a system occur (70% of outages). To minimalise the eefect of
chagnes there are 3 principles to follow:

* Implementing progressive rollouts
* Quickly and accurately determining problems
* Rolling back changes safely when problems arise

# Demand forecasting and capacity planning

SRE's should look to ensure that a system has enough resources to grow and be redundant.

* organic grow should be taken into account
* inorganic grow also (new feature releases, ...)
* load testing to correlate raw server capacity to service capacity

### Provisioning


> In our experience, provisioning must be conducted quickly and only when necessary, as capacity is expensive. This exercise must also be done correctly or capacity doesn’t work when needed. Adding new capacity often involves spinning up a new instance or location, making significant modification to existing systems (configuration files, load balancers, networking), and validating that the new capacity performs and delivers correct results.

### Efficiency and performance

> SREs provision to meet a capacity target at a specific response speed, and thus are keenly interested in a service’s performance. SREs and product developers will (and should) monitor and modify a service to improve its performance, thus adding capacity and improving efficiency.

just a reminder where I was in https://landing.google.com/sre/sre-book/chapters/service-level-objectives/
