---
layout: post
title: Copy at Your Own Risk
tags: PostgreSQL
---

# Copy at Your Own Risk

This is the tale of my efforts to move a PostgreSQL database from a
stand-alone instance to Amazon RDS. It was my first effort using RDS, and I
was not (and at the time of writing, still not) comfortable with the
configuration options provided through Amazon.


## About the Database

The database to be moved consisted primarily of a table storing two JSONB
fields. There were a couple additional tables used to track correlations
between messages with simple ID references. In production, the database
included north of 10 million records.


## First, `pg_restore`

The first pass at migrating the database was to do a dump and then restore
into RDS. For many databases, this can work out just fine. In fact, this
worked out just fine in two separate test environments, where we had data, but
nowhere near the number of records as in production. For production,
anticipating the load, we tuned the RDS instance for data loading as
recommended by Amazon (e.g., disabling backups, disabling multi-AZ, etc.).

On the first attempt to restore into RDS, it took about six hours to load
data into the tables. ... But, it took another thirty hours to run indices on
that data once it was in place. And, because the point of this data is to
search and navigate the data nested in the JSON structures, the indices are
essential.

Because we had anticipated no more than a couple hours of downtime for this
migration, we had to spin production back up and continue to point at the old
instance while we figured out the next step.


## `COPY`
