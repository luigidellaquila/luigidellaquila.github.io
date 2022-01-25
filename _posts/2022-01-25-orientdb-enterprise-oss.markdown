---
layout: post
title:  "OrientDB Enterprise Agent is now OSS"
date:   2022-01-25 18:00:00 +0100
categories: orientdb release oss
---

This is an important announcement for me: **OrientDB Enterprise Agent** becomes Open Source!

OrientDB Community Edition has one of the most permissive licenses in the market
(ie. [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) license).
People love it because it allows you to embed OrientDB in your product and to
distribute/sell it with no (or better, very very few, see the license) constraints.

Now SAP decided to extend this license also to OrientDB Enterprise Agent.

You will find references to "**OrientDB Enteprise Edition**" in
the [docs](https://orientdb.org/docs/3.2.x/ee/Enterprise-Edition.html) and in
other web resources. What it typically refers to is just **OrientDB Community Edition** PLUS **OrientDB Enterprise Agent**.

From today, the full bundle is Apache 2.0 licensed.

# Features

You can have a quick look at the [docs](https://orientdb.org/docs/3.2.x/ee/Enterprise-Edition.html), but you
probably want to know what really matters.

In my opinion out of all the features in the Enterprise Agent, there are two that make a HUGE difference:
- **Non-Blocking, Incremental Backup**
- **Delta Sync**

Having **Non-Blocking Backup** in production is critical in many scenarios. Imagine having to stop all the transactions during the backup...
OK, the backup takes only a few hundreds of milliseconds to execute (seconds in some cases), but still, there are scenarios where you cannot tolerate it.
Sure, you can set up a cluster and have a node dedicated to backups, but it's additional complexity.
With the Enterprise Agent you will have non-blocking backups, that means your server will be able to process transactions as usual, even during backups.

**Incremental Backups** is the cherry on the cake: you don't need to do a full backup every time, especially when your DB has just a few
changes. Enterprise Agent enables incremental backups, that allow you to just backup the differences from last backup, that means
faster execution time and smaller footprint on disk.

**Delta Sync**: what's this? Imagine you have a cluster of N nodes. One of the nodes goes down for a few seconds (a machine crash, a network outage, whatever) and remains unaligned with the rest of the cluster. When the node comes up again and rejoins the cluster, it
has to be aligned with the other nodes. The same happens when you add a brand new node to the cluster. In Community Edition, the node asks for a full (blocking!) backup to another node and realigns its db copy.

Imagine the scenario: one node is (re)stating, so it's unavailable; another node is blocked during a backup, so it's unavailable as well... not good uh? And how long does transferring a full backup take...?

With Enterprise Agent you have two possible scenarios:
- The starting node has a recent (but not up to date) copy of the DB: another node provides only last executed transactions (delta sync!) and the new node just re-executes them. The operation takes very little time; the new node is up and running quickly; the other node keeps working as usual during the operation.
- The starting node has a db copy that is too old (or just has no DB at all): another node performs a full (non blocking!) backup and sends it to the new node. The node still needs to wait for the whole db, but in the meantime the other node keeps working as usual.

These are the two features I love the most, but there is more: check all the other [cool things](https://orientdb.org/docs/3.2.x/ee/Enterprise-Edition.html) provided by Enterprise Agent.

# OK, but... why?

That's probably what you are wondering: why is SAP open sourcing OrientDB Enterprise Agent?

You can find an answer in the official [SAP announcement](https://orientdb.org/enterprise-agent-open-source), but I'd like to give
you my very personal point of view.

First of all, some context:

- OrientDB LTD was [acquired](https://www.globenewswire.com/news-release/2017/09/19/1124824/0/en/CallidusCloud-Acquires-Leading-Multi-Model-Database-Technology.html) by CallidusCloud in 2017.
- A few months later, in 2018, CallidusCloud was [acquired](https://www.sap.com/acquired-brands/what-is-calliduscloud.html) by SAP.
- SAP has its own strategy, that is strongly focused on **cloud** solutions.
- OrientDB, as an [On-Premise](https://en.wikipedia.org/wiki/On-premises_software) solution, is definitely out of context...


In this situation, decisions have to be taken.
A company can decide to ignore the product, reduce investments and eventually kill it.

In my opinion SAP took the most reasonable decision: OrientDB **IS** Open Source, the closed source license of the Enterprise Agent
was not in line with OrientDB philosophy. With this move, everything becomes more consistent.

And what is more important, from today the Community will have many more chances to guarantee a future to OrientDB as a whole,
without limitations related to licensing.


# So what happens in the near future?

Again, the answer is in the official announcement: SAP has active customers on OrientDB.

There is a team of OrientDB experts (including me) in SAP that keeps supporting these customers, maintaining the product.

The fixes we do are often related to Enterprise features; from now, all the Community will benefit from this maintenance effort.

Of course, the announcement explicitly refers to a deadline (end of 2023). It's two years from now.

OrientDB has a huge community, in last years [more than 100 people contributed](https://github.com/orientechnologies/orientdb/graphs/contributors) to the code, the repo was forked more than 800 times and has almost 4500 stars.

There is time, there are competences and there is interest. The future is just a matter of commitment.



# Useful resources

- [Official SAP announcement](https://orientdb.org/enterprise-agent-open-source)
- [OrientDB Documentation](https://orientdb.org/docs/3.2.x)
- [Enterprise Agent source code](https://github.com/SAP/orientdb-enterprise-agent)
- [OrientDB source code](https://github.com/orientechnologies/orientdb)
