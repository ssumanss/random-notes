---
layout: default
title: Quickstart Sqlite
nav_order: 8
---

#  Quickstart Flask

Starting SQlite

```
sqlite3 <database>
```
Viewing Table 
```shell
.tables
```
Viewing tables data 
```shell
SELECT * FROM <table>;
```
**Note:** Notice the `;` at the end of the command.
Exit the sqlite shell
```shell
.quit
```
Updating Database entry
```shell
admin = User.query.filter_by(username='admin').first()
admin.email = 'my_new_email@example.com'
db.session.commit()

user = User.query.get(5)
user.name = 'New Name'
db.session.commit()
```
