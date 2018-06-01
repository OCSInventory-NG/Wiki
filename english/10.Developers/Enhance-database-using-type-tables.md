# Enhance database using type tables

One of the major problem with actual database scheme, is the search requests time on large computers IT
(80000 computers or more). Indeed, some requests on softwares table can take over 20 minutes and often
this mean a timeout because requests take to much time.

To solve the problem, OCS engine can now feed inventory in "type" tables. The goal is to explode actual
tables fields in various "type" tables containing only an _ID_ field and a _NAME_ field. The actual tables
fields are modified to contain only the ID of the entry to the relative type table. We use foreign keys
guarantee the data consitency.

In this page, we will explode the actual softwares table because it is one of the problematic table in
the actual database scheme.

## Create type tables

We deceided to explode all of the _softwares_ table fields in differents type tables. We create a "type"
table per _softwares table field.

    CREATE TABLE type_softwares_name (ID INTEGER NOT NULL AUTO_INCREMENT, NAME VARCHAR(255), PRIMARY KEY (ID)) ENGINE=INNODB, DEFAULT CHARSET=UTF8 ;
    CREATE TABLE type_softwares_publisher (ID INTEGER NOT NULL AUTO_INCREMENT, NAME VARCHAR(255), PRIMARY KEY (ID)) ENGINE=INNODB, DEFAULT CHARSET=UTF8 ;
    CREATE TABLE type_softwares_version (ID INTEGER NOT NULL AUTO_INCREMENT, NAME VARCHAR(255), PRIMARY KEY (ID)) ENGINE=INNODB, DEFAULT CHARSET=UTF8 ;
    CREATE TABLE type_softwares_folder (ID INTEGER NOT NULL AUTO_INCREMENT, NAME VARCHAR(255), PRIMARY KEY (ID)) ENGINE=INNODB, DEFAULT CHARSET=UTF8 ;
    CREATE TABLE type_softwares_comments (ID INTEGER NOT NULL AUTO_INCREMENT, NAME VARCHAR(255), PRIMARY KEY (ID)) ENGINE=INNODB, DEFAULT CHARSET=UTF8 ;
    CREATE TABLE type_softwares_filename (ID INTEGER NOT NULL AUTO_INCREMENT, NAME VARCHAR(255), PRIMARY KEY (ID)) ENGINE=INNODB, DEFAULT CHARSET=UTF8 ;
    CREATE TABLE type_softwares_filesize (ID INTEGER NOT NULL AUTO_INCREMENT, NAME VARCHAR(255), PRIMARY KEY (ID)) ENGINE=INNODB, DEFAULT CHARSET=UTF8 ;
    CREATE TABLE type_softwares_source (ID INTEGER NOT NULL AUTO_INCREMENT, NAME VARCHAR(255), PRIMARY KEY (ID)) ENGINE=INNODB, DEFAULT CHARSET=UTF8 ;

## Modify softwares table

We modify the actual _softwares_ table to change fields names and fields types because this
field will point to _ID_ fields of the "type" tables:

    ALTER TABLE softwares CHANGE COLUMN NAME NAME_ID INTEGER NOT NULL ;
    ALTER TABLE softwares CHANGE COLUMN PUBLISHER PUBLISHER_ID INTEGER NOT NULL ;
    ALTER TABLE softwares CHANGE COLUMN VERSION VERSION_ID INTEGER NOT NULL ;
    ALTER TABLE softwares CHANGE COLUMN FOLDER FOLDER_ID INTEGER NOT NULL ;
    ALTER TABLE softwares CHANGE COLUMN COMMENTS COMMENTS_ID INTEGER NOT NULL ;
    ALTER TABLE softwares CHANGE COLUMN FILENAME FILENAME_ID INTEGER NOT NULL ;
    ALTER TABLE softwares CHANGE COLUMN FILESIZE FILESIZE_ID INTEGER NOT NULL ;
    ALTER TABLE softwares CHANGE COLUMN SOURCE SOURCE_ID INTEGER NOT NULL ;

## Create foreign keys

We create foreign keys to lie new _softwares_ table fields to _ID_ fields of the "type" tables:

    ALTER TABLE softwares ADD FOREIGN KEY (NAME_ID) REFERENCES type_softwares_name(ID) ;
    ALTER TABLE softwares ADD FOREIGN KEY (PUBLISHER_ID) REFERENCES type_softwares_publisher(ID) ;
    ALTER TABLE softwares ADD FOREIGN KEY (VERSION_ID) REFERENCES type_softwares_version(ID) ;
    ALTER TABLE softwares ADD FOREIGN KEY (FOLDER_ID) REFERENCES type_softwares_folder(ID) ;
    ALTER TABLE softwares ADD FOREIGN KEY (COMMENTS_ID) REFERENCES type_softwares_comments(ID) ;
    ALTER TABLE softwares ADD FOREIGN KEY (FILENAME_ID) REFERENCES type_softwares_filename(ID) ;
    ALTER TABLE softwares ADD FOREIGN KEY (FILESIZE_ID) REFERENCES type_softwares_filesize(ID) ;
    ALTER TABLE softwares ADD FOREIGN KEY (SOURCE_ID) REFERENCES type_softwares_source(ID) ;

## Modify Map.pm

We have to modify _Map.pm_ file to say to OCS engine to feed the new "type" table and various
ID in _softwares_ table during inventory:

      softwares => {
       mask => 65536,
       multi => 1,
       auto => 1,
       delOnReplace => 1,
       sortBy => 'NAME',
       writeDiff => 1,
       cache => 1,
       fields =>  {
           PUBLISHER_ID => { type=>1 },
           NAME_ID => { type =>1 },
           VERSION_ID => { type =>1 },
           FOLDER_ID => { type =>1 },
           COMMENTS_ID => { type=>1 },
           FILENAME_ID => { type=>1 },
           FILESIZE_ID => { fallback=>0, type=>1 },
           SOURCE_ID => { fallback=>0, type=>1 }
       }
      },

Restart Apache to take care of modifications in Map.pm :

    /etc/init.d/apache2 restart

(On Debian based distributions)

or

    /etc/init.d/httpd restart

(On Redhat based distributions)