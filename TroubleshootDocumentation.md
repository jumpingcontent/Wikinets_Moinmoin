
Here's the resulting documentation for our troubleshooting and other issues.


--------------------------------------------


**Summary of first issues:**

1. sudo apt-get update

2. sudo apt-get upgrade

3. cd /usr/local/share/moin

// Changing to the directory

4. sudo cp server/moin.wsgi

5. sudo nano moin.wsgi

-----

Below the import sys/ os line:

sys.path.insert(0, '/usr/local/lib/python2.7/dist-packages/')

sys.path.insert(0, '/usr/local/share/moin/')

-----

6. sudo nano uwsgi.ini

Lines to add to the file:

-----

[uwsgi]

uid = www-data

gid = www-data

socker = /usr/local/share/moin/moin.sock

chmod-socket = 660

logto = /var/log/uwsgi/uwsgi.logchdir = /usr/local/share/moin/

wsgi-file = moin.wsgimaster

workers = 3

max-requests = 200

harakiri = 30

die-on-term

-----

// Creating the uwsgi.ini file

7. sudo mkdir -p /var/log/uwsgi sudo chown www-data /var/log/uwsgi

8. sudo nano /etc/init/moin.conf

Add the lines below:

-----

description "moin uwsgi service"start on runlevel [2345]

stop on runlevel [!2345]chdir /usr/local/share/moin

exec /usr/local/bin/uwsgi /usr/local/share/moin/uwsgi.ini

respawn

-----

// Config file

9. cd /user/local/share/moin

10. sudo cp config/wikiconfig.py

11. sudo nano wikiconfig.py

Changing the file lines:

-----

sitename = u'Wikinet'page_front_page = u"moinwiki"

superuser = [u"Admin", ]

-----

12. sudo chown -R www-data: /usr/local/share/moin sudo chmod -R 775 /usr/local/share/moin

// Changing permissions

13. sudo start moin

// We're currently at this step

// The MoinMoin status is still showing as inactive

* We're currently troubleshooting the issue of the inactive status of this MoinMoin

--------------------------------------------


**Start Status Issue:**

( )


-----
