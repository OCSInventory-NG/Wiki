# The situation

For languages which mainly use Latin characters (English, German, French etc.) migration should not be difficult.

For others, migration will be difficult,many installations may seems like correct, but in real it is not, because of following reason:

1) **without 'bugs' in perl**

   1. agent send xml in native windows charset but with ISO8859-1 header
   2. perl parse it in ISO8859-1 and write in to database in latin1
   3. php get from database as latin1, decode it from utf8 (do nothing)
   4. show page according charset in language file

So user will see that data are correct, but when we will convert using mysqldump we need to know
from which codepage we should to convert.

2) **On some installations we will have some think like this:**

    1. agent send xml in native windows charset but with ISO8859-1 header
    2. perl parse it in utf8 but write in to database in latin1
    3. php get from database as latin1 decode it from utf8
    4. show page according charset in language file

So user will see that data are correct, but when we will convert using mysqldump we will have problem.

Because real data in database it is windows charset symbols in utf8 code page.

Second problem we need to know from which codepage we should convert.

This is on my experience for ubuntu/debian, may be same situations on some another linux.

3) Worst situation,

       in databse data with different encodings!
       For example French and Polish, add to this  two previous  variations.

May be we should get some statistic who and how install ocs?

## Possible decisions

First of all, we should understand which data must be "unchanged" after update.

On update, possible, will be changed some software names with some special characters,
which not present in latin1.

**Manual steps**

to convert database from one encoding(except ubuntu):

1. make backup in to file
2. update database to ocs 1.3 structure (Gonéri> I don't think we'll be ready for 1.3)
3. dump data
4. mysqldump -u ocs -p --default-character-set=latin1 -a --skip-set-charset --insert-ignore
--no-create-info ocsweb > ./ocsweb.sql
5. recode -f \<windows charset>..UTF8 ./ocsweb.sql
6. drop databse and create new one for version 1.3
7. import data from ./ocsweb.sql
8. mysql -u ocs -p ocsweb \<./ocsweb.sql

**Write perl script,**

witch will rename old database, create new one for ocs 1.3, copy data with decoding from old database
in to new one.

Script should ask for codepage and show preview of decoded data, before process all database.

Script also may use charsets from language files, so just ask user to select language from list,
or set encoding manual.

## Agents

### **Windows agent**

The current agent depend on the Windows localization (France/Russian/China/etc) but the XML present
the content ISO8859-1 → Bad data. Linvinus patches for the server can be used here.

###**unified UNIX agent**

MacOSX applications name can use non UTF-8 charset.

## Server

### **Default**

The server save in DB what is supposed to be ISO8859-1 according to the XML header but the XML may have
different content. Charset are mixed in DB.

### **UTF8 mode**

The server understand XML with UTF-8 and exotic charset. If the UTF8 mode is enabled, the information
are saved as UTF8.