# liquibase-demo
There are changelog file types.
example-changelog.sql,example-changelog.xml,example-changelog.yml

To initialize the liquibase project run this following to generate liquibase.properties, example-change.sql.
 `$ liquibase init project`
We can use other type of format instead of sql.

# Demo 1
Run the following commands step by step.
- `$ liquibase init project`
- change db url and credentials in liquibase.properties.
- add mysql jar file into liquibase_libs folder which is to store the specificed jar files.
- download snakeyaml jar and copy into current dir. This is needed for yml file type.
- `$ liquibase update --changelog-file=root-changelog.yml`
  You can check `DATABASECHANGELOG` and `DATABASECHANGELOGLOCK` tables. There are three records in DATABASECHANGELOG table. Because there are three changesets with author,filename,MD5Hash,tag etc.

# Demo with rollback with tag
We can run rollback to restore the database changes by using tag. We already defined tag in yaml file. You can find `tagDatabase` field.
```
  - changeSet:
      id: 1
      author: kyawswar
      changes:
        -  tagDatabase:  
            tag:  version_1.0
```
  This changeset is saved as version_1.1 in tag column of DATABASECHANGELOG table. We can use this tag value to restore this changesets.
  $ liquibase rollback --tag=version_1.0 --changelog-file=example-changelog.yml
  This above command will restore from version 1 changesets and the othere changesets are gone.

# Demo with multiple changelog files
In real world, the single change log file causes confilct or complex to update new changes when we are working together with teams. Liquibase supports two styles of multiple change log: object orient and release orient.

# Object orient
we can define a specified changelog for each function based on query create, update, index or delete etc.

# release orient
we can define changelog based on project release version.

# Demo
I created three change log files.
```
root-changelog.yml
create_tables
  |- person-changelog.yml
release
  |- release_1.0.yml

```

root-changelog.yml
```
---
databaseChangeLog:
   - includeAll:
       path: /create_tables/
   - includeAll:
       path: /releases/
```
Run the update command with root-changelog.yml.

``$ liquibase update --changelog-file=root-changelog.yml``

It will load change log files from create_tables and releases folder.

# generate change log file from existing database
we can use generate-changelog command as follow.

```$ liquibase generate-changelog - changelog-file=example-changelog.xml```

Please read Liquibase documentation for more advance functionalities.
