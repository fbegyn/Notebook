---
date: 2020-09-03T16:45
---

# coretcd

Coretcd aims to make TC easy to be used in datacenters. Instead of throwing around random scripts and programs on all the network metal, coretcd aggregates the control of all these things.

When coretcd starts up, it registers itself against a central SD application. Through this applications it becomes possile to see which cortcd are running.

A central key-value store is used to push the configuration out the the specific coretcd programs.

## requirements

For this to work, th following requirements must be met:
* central key value store
* central SD program
* library that can modify and update TC configs (maybe [cruise control](https://github.com/fbegyn/cruise-control))
