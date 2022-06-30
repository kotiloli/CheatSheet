# PSQL Build-in shell commands

| Command                            | Description                                       |
| :----------------------------------|:--------------------------------------------------|
| `\l` or `\list` or `l+`            | List all databases                                |
| `\connect [dbname]` or `\c dbname` | Connect a database                                |
| `\dt`                              | Display tables of connected database              |
| `\du`                              | Display users                                     |
| `\db+`                             | Show Tablespaces                                  |
|`pg_restore.exe -U USER -d DATABASE C:\...path....\dvdrental.tar`| Load a database backup using pg_restore.exe|