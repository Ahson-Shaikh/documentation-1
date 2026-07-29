---
id: backup
title: Backup HumHub
---

Whether you are upgrading your installation of HumHub using the updater module or the manual, you must make sure you have your own set of backup data, one you can rely on and to which you can get back on your own terms.

Create a Backup of HumHub Data
------------------------------

* Create a full backup of the HumHub database
* Backup instance files
  - `/protected/config`
  - `/protected/modules`
  - `/uploads`
  - `/themes/yourtheme` (if you're running an own theme)


Restore HumHub from Backup Data
--------------------------------

* Restore and import backup database
* Restore instance files
  - `/protected/config`
  - `/protected/modules`
  - `/uploads`
  - `/themes/yourtheme` (if you're running an own theme)
* Rebuild the search index, see [Search chapter](search.md)

:::note
When uploading your files to `/uploads` by ftp make sure to use `binary` transfer mode.
:::

Example: Backup on Linux
------------------------

The following examples show one possible way to create backups on a Linux server.
Adjust database name, user and paths to your own environment.

### Database Dump

```bash
mariadb-dump -h <DB_HOST> -u <DB_USER> -p \
    --default-character-set=utf8mb4 \
    --single-transaction \
    --skip-extended-insert \
    <DB_NAME> \
    | gzip -9 > <BACKUP_DIR>/humhub_database_`date +'%Y%m%d%H%M%S'`.sql.gz
```

On MySQL servers the same command can be used with `mysqldump` instead of `mariadb-dump`. All
options above are supported by both clients.

### Instance Files

```bash
cd <HUMHUB_ROOT>/..
sudo tar czf <BACKUP_DIR>/humhub_files_`date +'%Y%m%d%H%M%S'`.tar.gz \
    humhub/protected/config/ \
    humhub/protected/modules/ \
    humhub/uploads/ \
    humhub/themes/
```

:::note
Store your backups outside of the web root and outside of the `/uploads` folder, so they are not publicly accessible, and not backuped recursively by the next run.
:::
