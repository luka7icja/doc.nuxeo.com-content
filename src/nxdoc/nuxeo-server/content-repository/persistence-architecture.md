---
title: Persistence Architecture
review:
    comment: ''
    date: '2015-12-01'
    status: ok
labels:
    - content-review-lts2016
tree_item_index: 200
history:
    -
        author: Florent Guillaume
        date: '2014-11-04 16:00'
        message: ''
        version: '4'
    -
        author: Alain Escaffre
        date: '2014-09-19 16:09'
        message: ''
        version: '3'
    -
        author: Alain Escaffre
        date: '2014-09-19 16:02'
        message: ''
        version: '2'
    -
        author: Alain Escaffre
        date: '2014-09-19 15:26'
        message: ''
        version: '1'

---
The persistence layer of the Nuxeo Platform repository is configurable. Several options can be chosen depending on your scalability, integration, and performance constraints.

The repository stores:

*   **Documents**
    For the documents, you can either choose the&nbsp;[VCS repository implementation]({{page page='vcs'}})&nbsp;that stores the data on an RDBMS, or the&nbsp;[DBS - MongoDB implementation]({{page page='dbs'}}), that stores the data on a MongoDB document-based system.

*   **Binaries**
    T<span style="line-height: 21.58px;">he[ binary store documentation page]({{page page='file-storage'}}) describes the options available for storing binaries: file system, cloud-based, encrypted.</span>