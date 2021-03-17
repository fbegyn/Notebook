---
date: 2021-02-07T12:58
---

# FOSDEM 2021 notes

## Proper monitoring - monitoring devroom

better alerts -> context and action step

context -> what caused the alert? Do it extensively not just "service is down", explain why the alert
has been triggered. Be very clear in communicating a alert.
    -> why is this alert triggered  and how does this impact the businees
    
action steps -> give indication of what the respondee should do. What to do and who to escelate to.

don't be looking at dashboards all the time

dashboards are there to validate operating parameters (like in a call)

dashboards should not look cool, they should desing for troubleshooting
-> context and actions steps
-> name metrics well -> Prometheus metrics naming
-> dashboards should give context (what service is it for? what service does it relate to? where are dashboards for those services, how to validat graph behavior)
-> text widgets

actions steps -> what should I do?

implement software engineering principles: documentation, testing, revision control, ...
dashboards and alerts as code, export the dashboards and store them in git, Terraform, ...

test with chaos engineering

## fastclick and beyond

packet metadata -> lenght checksum -> driver

user metadata -> src/dst address, VLAN ID, ...

fastclick model:
copy data from the driver to the application (2 copies)

bess model:
1 copy from the dpdk descripter, but lots of unneeded info

x-change introduces custom buffers for the dpdk layer
