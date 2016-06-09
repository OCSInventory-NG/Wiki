# Backup/restore of OCS Inventory NG database

You can use tools like phpMyAdmin or MySQL Administrator to backup/restore MySQL databases.

However, MySQL Server provide standard tools usable in command line.

## Backing up OCS Inventory NG database

Tool “mysqldump” allows you to dump all content of a database to a single file. These tool is
available in directory “INSTALL_PATH\xampp\mysql\bin” where INSTALL_PATH is the installation
oath you choosen for OCS Inventory NG Server for Windows, and generally can be directly called under Linux.

Simply run the following command to backup OCS Inventory NG database:

    mysqldump --add-drop-table --complete-insert --extended-insert --quote-names --host=localhost --user=root --password=root password ocsweb > mysqldump_ocsweb.sql

This will save database content of OCS Inventory NG database _ocsweb_ into file _mysqldump_ocsweb.sql_.

**`Note: If you do not have backup software for MySQL then you may find Auto MySQL Backup handy :)`**

## Restoring OCS Inventory NG database

Tool “mysql” allows you to launch SQL queries on a database. These tool is available in directory
“INSTALL_PATH\xampp\mysql\bin” where INSTALL_PATH is the installation oath you choosen for
OCS Inventory NG Server for Windows, and generally can be directly called under Linux.

Just run MySQL command line interpreter to import saveset previously created “ocsweb” database.

    $mysql –u root –p ocsweb
    mysql>source <path_to_saveset>
    mysql>exit

**`Note: You will be prompted from root password. If you haven’t yet set root password, do use "-p"
command line switch.`**

![Linux Command](../../img/Restaure_bdd.jpg)
