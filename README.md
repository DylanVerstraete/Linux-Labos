# Linux-Labos

Fork deze repository door rechtsboven in het scherm op Fork te drukken.

Git clone de repository van je eigen github pagina naar je computer.

Indien je wijzigingen wilt laten weten vraag je een pull request aan zodat we deze repository up to date kunnen houden.

# Linux Permissions

Perm         Own   Grp   Size LastMod    Name
----------------------------------------------
drwxrwxr-x 2 jesse jesse 4096 Jan 30 20:15 .
-rw-rw-r-- 1 jesse jesse    0 Jan 30 20:15 cat
-rw-rw-r-- 1 jesse jesse    0 Jan 30 20:15 dog
drwxrwxr-x 9 jesse jesse 4096 Jan 30 20:15 ..

Sets
----
Owner  | xxx
Group  | xxx
Public | xxx

Types of permissions:
---------------------
Read    | 4 | r
Write   | 2 | w
Execute | 1 | x

Decimals
--------
677

O   G   P
------------
rw- rw- r--
6   6   4
rwx rw- r--
7   6   4
rwx rwx rwx
7   7   7
rw- r-- ---
6   4   0

Commands
----------

`$ chmod 664` -> Adds the permissions to a file / directory.
`$ chgrp group item` -> Adds the designated group for that file/directory (inherits group perms)
`$ chown user item` -> Sets owner to a file / directory.

Non Decimal Modifiers
---------------------
Owner   | u
Group   | g
Public  | o

Read    | r
Write   | w
Execute | x

Commands
---------
`chmod u-rwx item` -> Gives all the permissions to the owner
`chmod g-x item` -> Gives the group the execute permission
`chmod o-r item` -> Gives public / others read permission.

