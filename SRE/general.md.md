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

just a reminder where I was in https://landing.google.com/sre/sre-book/chapters/introduction/
