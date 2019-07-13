---
title: How to setup PostgreSQL on MacOS
date: "2019-01-13T23:35:19.112Z"
layout: post
draft: false
path: '/posts/How-to-setup-PostgreSQL-on-MacOS'
category: "PostgreSQL"
tags:
  - "Web Development"
  - "Database"
  - "PostgreSQL"
description: "PostgreSQL setup reference"
---

![PostgreSQL](./postgresql.jpg)

I would be lying, if I say I don't look up how to setup PostgreSQL - every DAMN time. We know it was hard to do before, but now it isn't, yet most of the guides we find are outdated. So, here we have an updated version of it.

### Installing PostgreSQL

First, install [HomeBrew](https://brew.sh) if you don't have it already, you can use the following script to install it.

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Once you are done with it, update HomeBrew dependencies and install PostgreSQL.

```bash
brew update
brew install postgresql
```
See, I told you it was Easy. Let's check Postgres version

```bash
postgres --version
postgres (PostgreSQL) 11.3
```

### Initializing the DB
A Database needs some space allocated on your hard-disk, also called as "cluster" or "Data directory". It happens automatically, but to be sure run the following
```bash
initdb /usr/local/var/postgres
```
You may see the error message: `initdb: directory “/usr/local/var/postgres” exists but is not empty` if the database was already created when we installed PostgreSQL.

### Start/Stop PostgreSQL

You can do it either ways, by directly calling the following
```bash
pg_ctl -D /usr/local/var/postgres start
pg_ctl -D /usr/local/var/postgres stop
```
or by using brew
```bash
brew services postgres start
brew services postgres stop
brew services postgres restart
```

### Creating a PostgreSQL Database

To create a database run the following
```bash
createdb databasename
```
To remove a database run the following
```bash
dropdb databasename
```
To Connect to a database run
```bash
psql databasename
```
This will open up the psql sheel, where you can run SQL quries, to exit press *<kbd>ctrl+d</kbd>*. Some useful commands, to know.
- `\list` To list all databases.
- `\c databaseName` to connect to `databaseName`.
- `\d` To list relations in current database.
- `\d tableName` To list info of a table.