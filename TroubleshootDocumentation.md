
Here's the resulting documentation for our troubleshooting and other issues.


--------------------------------------------


**Summary of first issues:**

1. sudo apt-get update

2. sudo apt-get upgrade

3. install apache2

4. sudo apt-get install python-pip python-dev

// Installing pip (the python package manager)

5. sudo pip install http://projects.unbit.it/downloads/uwsgi-lts.tar.gz

// Ready to proceed to download moinmoin

6. wget http://static.moinmo.in/files/moin-1.9.9.tar.gz

// Downloading MoinMoin from https://moinmo.in/MoinMoinDownload

7. tar -zxvf moin-1.9.9.tar.gz

8. cd moin-1.9.9

9. sudo python setup.py install --prefix=/usr/local

10. cd /usr/local/share/moin

// Changing to the directory

11. sudo cp server/moin.wsgi

12. sudo nano moin.wsgi

-----

Below the import sys/ os line:

sys.path.insert(0, '/usr/local/lib/python2.7/dist-packages/')

sys.path.insert(0, '/usr/local/share/moin/')

-----

13. sudo nano uwsgi.ini

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

14. sudo mkdir -p /var/log/uwsgi sudo chown www-data /var/log/uwsgi

15. sudo nano /etc/init/moin.conf

Add the lines below:

-----

description "moin uwsgi service"start on runlevel [2345]

stop on runlevel [!2345]chdir /usr/local/share/moin

exec /usr/local/bin/uwsgi /usr/local/share/moin/uwsgi.ini

respawn

-----

// Config file

16. cd /user/local/share/moin

17. sudo cp config/wikiconfig.py

18. sudo nano wikiconfig.py

Changing the file lines:

-----

sitename = u'Wikinet'page_front_page = u"FrontPage"

superuser = [u"Admin", ]

-----

19. sudo chown -R www-data: /usr/local/share/moin sudo chmod -R 775 /usr/local/share/moin

20. sudo start moin

// We're currently at this step

// The MoinMoin status is showing as inactive

--------------------------------------------
