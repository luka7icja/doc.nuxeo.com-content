---
title: Nuxeo Server LTS 2021 Release Notes
description: Discover what's new in LTS 2021.6 / LTS 2021 HF06
review:
   comment: ''
   date: '2021-07-16'
   status: ok
labels:
    - release-notes
toc: true
tree_item_index: 10000
---

{{! multiexcerpt name='nuxeo-server-updates-2021'}}
# What's New in LTS 2021.6 / LTS 2021 HF 06

## Nuxeo Server

### Packaging / Distribution / Installation

#### Tomcat 9.0.50 {{> tag 'dev'}} {{> tag 'admin'}}

The Nuxeo Platform now relies on Tomcat 9.0.50.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30506](https://jira.nuxeo.com/browse/NXP-30506)

## Major bug fixes

### Fix temporary blob move to transient storage when parent folders are deleted by the Garbage Collector

Empty directories are no more deleted from transient stores.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30465](https://jira.nuxeo.com/browse/NXP-30465)

### Handle permission restriction in `DocumentModelResolver`

`DocumentModelResolver` computes the Document entity only if the Read permission is granted.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30192](https://jira.nuxeo.com/browse/NXP-30192)

### Azure - Fix "The specified block list is invalid" error with Azure

The known issue with Azure storage where concurrent upload requests could produce the error "The specified block list is invalid" is now fixed.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30465](https://jira.nuxeo.com/browse/NXP-30465)

# Learn more

[More information about released changes and fixed bugs](https://jira.nuxeo.com/secure/ReleaseNote.jspa?projectId=10011&version=21422) is available in our bug tracking tool.

{{#> callout type='info' heading='Upgrade Notes'}}
Refer to the [LTS 2021.1 upgrade notes]({{page page='upgrade-from-lts-2019-to-lts-2021'}}) to transition to this version.
{{/callout}}


{{! /multiexcerpt}}