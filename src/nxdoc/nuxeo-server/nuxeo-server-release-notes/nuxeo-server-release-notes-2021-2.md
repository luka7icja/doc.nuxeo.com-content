---
title: LTS 2021.2 / LTS 2021-HF02
description: Discover what's new in LTS 2021.2 / LTS 2021-HF02
review:
  comment: ''
  date: '2021-06-16'
  status: ok
labels:
  - release-notes
toc: true
tree_item_index: 9500
---

{{! multiexcerpt name='nuxeo-server-updates-2021-2'}}

# What's New in LTS 2021.2 / LTS 2021-HF02

## Nuxeo Server

### Core Storage

#### Asynchronous Blob Digest Calculation {{> tag 'dev'}} {{> tag 'admin'}}

Some files are uploaded to Nuxeo using external components as "direct upload" and therefore their content is never seen by Nuxeo, which makes it impossible to synchronously compute their digest and use this digest as the blob key.

Having the blob key be a digest is useful for:

- deduplication,
- compliance with customer rules that require keys to be digests.

To fix this, we have introduced a process to asynchronously compute the digest of each new blob (after downloading it) and "renaming" the blob key. This renaming involves to move the blob in the blob provider, and to find all documents that have this blob key (thanks to NXP-29516) to change them to use the new key.

To enable this feature, we added a new property to compute the blob digest asynchronously when Nuxeo doesn't see the blob content at upload time:

```
nuxeo.core.blobstore.digestAsync=true
```

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30044](https://jira.nuxeo.com/browse/NXP-30044)

This new capability is also used to add the new blob key for old blobs.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30043](https://jira.nuxeo.com/browse/NXP-30043)

Finally, we improved the performance of the search by blob key.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-29516](https://jira.nuxeo.com/browse/NXP-29516)

#### Allow Logging S3 Downloads {{> tag 'dev'}} {{> tag 'admin'}}

Additional logs are now produced to specifically track S3 downloads.

It can be enabled by adding to lib/log4j2.xml:

```
<Logger name="S3_Download" level="debug" />
```

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30173](https://jira.nuxeo.com/browse/NXP-30173)

### Elasticsearch

#### New Parameter to Enable Index Aliases

It is now possible to easily activate the Elasticsearch alias usage using a property:

```
elasticsearch.manageAlias.enabled=true
```

Note that when switching an existing instance to use Elasticsearch alias you first have to drop the existing elastic index for the repository (named nuxeo by default) then reindex your repository.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30309](https://jira.nuxeo.com/browse/NXP-30309)

#### Elasticsearch Passthrough

Elasticsearch passthrough is now configurable with certificates from private certificate authorities.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-29583](https://jira.nuxeo.com/browse/NXP-29583)

### Bulk Service (Aka "Bulk Action Framework")

#### Bulk Scroller Completes Command in Error When Query Times Out {{> tag 'dev'}}

The Bulk scroller now completes the command in error when a query times out.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30233](https://jira.nuxeo.com/browse/NXP-30233)

### Packaging / Distribution / Installation

#### Allow Adding New Templates Without Overriding the Whole `nuxeo.templates` Property

In the previous versions of Nuxeo Platform, there was no other way than overriding the whole `nuxeo.templates` property in `nuxeo.conf` if we wanted to add new templates to be deployed in a given environment: that means we had to know the exact list of templates configured by default and all the installed packages.

You can now use `nuxeo.append.templates.*` properties in `nuxeo.conf`.
All those properties will be sorted alphabetically (for determinism and reproducibility), and all the values will be added after the ones from the nuxeo.templates property when passing everything to the ConfigurationGenerator.

For instance, we could have:

```
nuxeo.templates=default
...
nuxeo.append.templates.infra=mongodb,redis,kafka
nuxeo.append.templates.cloud=cloud-cloudfront, cloud-directupload
```

The effective list will be: default,cloud-cloudfront,cloud-directupload, mongodb,redis,kafka.

This could then be used in our default Nuxeo Helm chart:

{{! NOTE: Double opening curly-braces within the following code block have a [zero width space](https://en.wikipedia.org/wiki/Zero-width_space) between them to avoid them being interpreted by Handlebars. }}

```
...
{​{- if or .Values.mongodb.deploy .Values.tags.mongodb }}
    nuxeo.append.templates.mongo=mongodb
    nuxeo.mongodb.server=mongodb://{​{ .Release.Name }}-mongodb:27017
    nuxeo.mongodb.dbname={​{ .Values.nuxeo.mongodb.dbname }}
{​{- end }}
{​{- if or .Values.postgresql.deploy .Values.tags.postgresql }}
    nuxeo.append.templates.postgres=postgresql
    nuxeo.db.name={​{ .Values.nuxeo.postgresql.dbname }}
    nuxeo.db.user={​{ .Values.nuxeo.postgresql.username }}
    nuxeo.db.password={​{ .Values.nuxeo.postgresql.password }}
    nuxeo.db.host={​{ .Release.Name }}-postgresql
    nuxeo.db.port=5432
{​{- end }}
{​{- if or .Values.redis.deploy .Values.tags.redis }}
    nuxeo.append.templates.redis=redis
{​{- end }}
...
```

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-29920](https://jira.nuxeo.com/browse/NXP-29920)

#### Tomcat 9.0.45 {{> tag 'dev'}} {{> tag 'admin'}}

The Nuxeo Platform now relies on Tomcat 9.0.45.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30330](https://jira.nuxeo.com/browse/NXP-30330)

## Addons

### WOPI - File Version Uses User Change Token {{> tag 'dev'}}

The document user change token is now used as WOPI item version.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30120](https://jira.nuxeo.com/browse/NXP-30120)

## Major Bug Fixes

### Support of the Carriage Return Characters Into the CSV Export

Carriage return characters are now escaped in the CSV export.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30078](https://jira.nuxeo.com/browse/NXP-30078)

### Fix How Enricher Priority Is Taken Into Account

Enricher priority is now taken into account when overriding an enricher.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30205](https://jira.nuxeo.com/browse/NXP-30205)

### Fix Algorithm to Unpublish a Document

Unpublishing a source document now loads only the published documents. This is improving the flow by avoiding searching all the published documents of the section and loading them into the memory.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30090](https://jira.nuxeo.com/browse/NXP-30090)

### When Exiftool Fails, `file:content` Gets Deleted

Previously, File content was deleted in case of ExifTool failure.

File content is now unchanged after an ExifTool failure.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30247](https://jira.nuxeo.com/browse/NXP-30247)

### Timeout on CommandLine Executor Fails for Windows

CommandLine timeout is now disabled on Windows as it is not supported.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30274](https://jira.nuxeo.com/browse/NXP-30274)

### Fix Inheritance of Local Configuration’s Allowed Subtypes in Subfolders of Configured Workspace

The local configuration for subtypes is now inherited to subfolders.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30128](https://jira.nuxeo.com/browse/NXP-30128)

### Allow Configuration of Signature Algorithm for SAML

We can now configure the digest algorithm used for the signature with SAML.

To configure a signature algorithm for SAML, add a SignatureAlgorithm entry to the plugin <parameters>, for instance:

```
<parameter name="SignatureAlgorithm">http://www.w3.org/2001/04/xmldsig-more#rsa-sha256</parameter>
```

The signature algorithms are defined by the various SAML and XML specs, in particular [RFC 6931](https://tools.ietf.org/html/rfc6931). If an algorithm unknown to the current library has to be used, the following more verbose syntax that includes the explicit JCA/JCE key algorithm specifier (RSA in this example) may be used:

```
<parameter name="SignatureAlgorithm.RSA">http://www.w3.org/2001/04/xmldsig-more#rsa-sha256</parameter>
```

In the same way it's possible to define a digest algorithm with the DigestAlgorithm parameter:

```
<parameter name="DigestAlgorithm">http://www.w3.org/2001/04/xmlenc#sha256</parameter>
```

Consult the normative documents like [W3C XML Encryption](https://www.w3.org/TR/xmlenc-core1/) for the algorithms.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-28450](https://jira.nuxeo.com/browse/NXP-28450)

### Fix CAS Authentication Anonymous Client

CAS authentication redirects with an HTTP 302 when anonymous is enabled.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30083](https://jira.nuxeo.com/browse/NXP-30083)

### Remove Picture Migration at Startup

Picture migration (required for the upgrades to 8.10 and 9.10) is not needed anymore. So, it has been removed from startup.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30269](https://jira.nuxeo.com/browse/NXP-30269)

### Increase Bulk Status TTL on Completion With Error

The bulk status is now kept for 24h in case of error.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30182](https://jira.nuxeo.com/browse/NXP-30182)

### Operations `RenderDocument` and `RenderDocumentFeed` Not Working With REST

The operations `RenderDocument` and `RenderDocumentFeed` are now usable with REST.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-28512](https://jira.nuxeo.com/browse/NXP-28512)

### Prevent Creation of Empty Thumbnail on Audio File

We had previously some error cases where the thumbnail generated on audio files were empty.

The thumbnail is now correctly generated for Audio documents.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30312](https://jira.nuxeo.com/browse/NXP-30312)

### LibreOffice 5.3.6.1 Hangs on Some HTML to PDF Conversion

The Nuxeo Platform docker images now use LibreOffice 7.1.1 which fix the issue.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30179](https://jira.nuxeo.com/browse/NXP-30179)

### Nuxeo Enhanced Viewer - Security Policy to Filter Annotations Is Not Working

Security policies are now taken into account by Annotations in Nuxeo Enhanced Viewer, so that you can now filter the annotations displayed on NEV based on the security policy definition.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30238](https://jira.nuxeo.com/browse/NXP-30238)

### Live Connect - Fix Upload of a Google Drive Shared File

Files shared among Google Drive accounts are correctly uploaded with LiveConnect.

<i class="fa fa-long-arrow-right" aria-hidden="true"></i>&nbsp;More on JIRA ticket [NXP-30241](https://jira.nuxeo.com/browse/NXP-30241)

# Learn More

[More information about released changes and fixed bugs](https://jira.nuxeo.com/secure/ReleaseNote.jspa?projectId=10011&version=21177) is available in our bug tracking tool.

{{! /multiexcerpt}}