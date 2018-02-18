---
layout: post
title: Copy at Your Own Risk
tags: PostgreSQL
---

This is the tale of an attempt to move a PostgreSQL database from a
stand-alone instance to Amazon RDS. It was my first effort to set up RDS, and
I was not (and at the time of writing, still not) comfortable with the
configuration options provided through Amazon.


## First, `pg_restore`

The first pass at migrating the database was to do a dump and then restore
into RDS. For many databases, this can work out just fine.
