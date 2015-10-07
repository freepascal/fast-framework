# Install #

  1. download the framework in /FFW/
  1. download some libs in /libphp/ :
    * Smarty
    * spyc
    * PHPMailer
  1. download some javascript libs in /libjs/ :
    * FCKeditor
  1. put your projet in /mods/
  1. put your website in /www/

So in the end, you have :

```
/FFW/
/libphp/
/libjs/
/mods/
/www/
```

# How it works #

First: FFW only uses two objects: $db for the database, $smarty for the template engine. All the rest is procedural code and associative arrays. If you like OOP, you'll certainly find FFW very ugly.

You call http://www.mysite.com/myModule/myAction/myParams :

  1. your .htaccess file routes the call to /www/index.php

/www/index.php does this :
  1. include spyc
  1. load the YAML config file in $conf associative array.
  1. include /FFW/FFW.php

/FFW/FFW.php does this :
  1. creates $smarty (smarty template engine object)
  1. creates $db (the ADOdb like database object)
  1. creates $user (associative array with all data about the user)
  1. creates $page (associative array with all data about the page)

So now you have it all : your configs, your page, you user, your database and your template engine. FFW.php simply includes /mods/mod\_myModule/myModule.php and lets the module do whatever you want.

If you want to learn how it works in details, simply read /FFW.php, it's not even 250 lines long, and it's code for dummies like me, so it won't take you long to figure out how all this works.

# Usual actions #

Many frameworks use CRUD convention with mainly CReate, Update and Delete actions. Because the display of the form used to create an objet is an action in itself, I use :

  * **create** : displays form to create an object
  * **insert** : receives data from create and process it. You can simply reply saying "thank you" or redirect somewhere else.
  * **edit** : form for object editing
  * **update** : receives data and write in db
  * **list** : lists objects
  * **delete** : deletes an object

Of course, you can add any action you want in your module.

# How to create a module #

Create /mods/mod\_foo/foo.php

This file is your module. You have $conf, $user, $page, $db, $smarty to play with. I often start with a single file with several if ($action=='edit') blocks, and split the module into various foo\_action1.php, foo\_action2.php files when it gets too big.