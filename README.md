General Information:
--------------------

This is a setup for a TOR based shared hosting server. It is provided as is and before putting it into production you should make changes according to your needs. This is a work in progress and you should carefully check the commit history for changes before updating.

Installation Instructions:
--------------------------

The configuration was tested with a standard Debian sid and Ubuntu 16.04 LTS installation. It's recommended you install Debian sid on your server, but with a little tweaking you may also get this working on other distributions and/or versions.

Uninstall packages that may interfere with this setup:
```
apt-get purge apache2* resolvconf
```

If you are on Ubuntu, add the following PPA:
```
LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php
```
On debian stable this may be worth a look: https://deb.sury.org/

To get the latest tor version, you should follow these instructions to add the official tor repository for your distribution: (https://www.torproject.org/docs/debian)

The following command will install all required packages:
```
apt-get --no-install-recommends install apt-transport-tor aspell curl dovecot-imapd dovecot-pop3d git dnsmasq haveged hunspell iptables locales-all logrotate mariadb-server nginx-light postfix postfix-mysql \
php7.0-bcmath php7.0-bz2 php7.0-cli php7.0-curl php7.0-dba php7.0-enchant php7.0-fpm php7.0-gd php7.0-gmp php7.0-imap php7.0-intl php7.0-json php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-opcache php7.0-pspell php7.0-readline php7.0-recode php7.0-soap php7.0-sqlite3 php7.0-tidy php7.0-xml php7.0-xmlrpc php7.0-xsl php7.0-zip \
php7.1-bcmath php7.1-bz2 php7.1-cli php7.1-curl php7.1-dba php7.1-enchant php7.1-fpm php7.1-gd php7.1-gmp php7.1-imap php7.1-intl php7.1-json php7.1-mbstring php7.1-mcrypt php7.1-mysql php7.1-opcache php7.1-pspell php7.1-readline php7.1-recode php7.1-soap php7.1-sqlite3 php7.1-tidy php7.1-xml php7.1-xmlrpc php7.1-xsl php7.1-zip \
php7.2-bcmath php7.2-bz2 php7.2-cli php7.2-curl php7.2-dba php7.2-enchant php7.2-fpm php7.2-gd php7.2-gmp php7.2-imap php7.2-intl php7.2-json php7.2-mbstring php7.2-mysql php7.2-opcache php7.2-pspell php7.2-readline php7.2-recode php7.2-soap php7.2-sqlite3 php7.2-tidy php7.2-xml php7.2-xmlrpc php7.2-xsl php7.2-zip \
php7.3-bcmath php7.3-bz2 php7.3-cli php7.3-curl php7.3-dba php7.3-enchant php7.3-fpm php7.3-gd php7.3-gmp php7.3-imap php7.3-intl php7.3-json php7.3-mbstring php7.3-mysql php7.3-opcache php7.3-pspell php7.3-readline php7.3-recode php7.3-soap php7.3-sqlite3 php7.3-tidy php7.3-xml php7.3-xmlrpc php7.3-xsl php7.3-zip \
phpmyadmin php-apcu php-gnupg php-imagick sasl2-bin ssh subversion tor vsftpd && apt-get --no-install-recommends install adminer
```

For optimum spell checking capabilities you can optionally install the following packages:
```
apt-get install aspell-am aspell-ar aspell-ar-large aspell-bg aspell-bn aspell-br aspell-ca aspell-cs aspell-cy aspell-da aspell-de aspell-el aspell-en aspell-eo aspell-eo-cx7 aspell-es aspell-et aspell-eu aspell-eu-es aspell-fa aspell-fo aspell-fr aspell-ga aspell-gl-minimos aspell-gu aspell-he aspell-hi aspell-hr aspell-hsb aspell-hu aspell-hy aspell-is aspell-it aspell-kk aspell-kn aspell-ku aspell-lt aspell-lv aspell-ml aspell-mr aspell-nl aspell-no aspell-or aspell-pa aspell-pl aspell-pt aspell-pt-br aspell-pt-pt aspell-ro aspell-ru aspell-sk aspell-sl aspell-sv aspell-ta aspell-te aspell-tl aspell-uk aspell-uz \
hunspell-af hunspell-an hunspell-ar hunspell-be hunspell-bg hunspell-bn hunspell-br hunspell-bs hunspell-ca hunspell-cs hunspell-da hunspell-de-at hunspell-de-ch hunspell-de-de hunspell-el hunspell-en-au hunspell-en-ca hunspell-en-gb hunspell-en-med hunspell-en-us hunspell-en-za hunspell-es hunspell-eu hunspell-eu-es hunspell-fr hunspell-fr-comprehensive hunspell-gd hunspell-gl hunspell-gu hunspell-he hunspell-hi hunspell-hr hunspell-hu hunspell-is hunspell-it hunspell-kk hunspell-kmr hunspell-ko hunspell-lo hunspell-lt hunspell-ml hunspell-ne hunspell-nl hunspell-no hunspell-oc hunspell-pl hunspell-pt-br hunspell-pt-pt hunspell-ro hunspell-ru hunspell-se hunspell-si hunspell-sk hunspell-sl hunspell-sr hunspell-sv hunspell-sw hunspell-te hunspell-th hunspell-tools hunspell-uk hunspell-uz hunspell-vi
```

Note that both, debian and the torproject have hidden service package archives, so you may want to edit /etc/apt/sources.list to load from those instead:
```
deb tor+http://vwakviie2ienjx6t.onion/debian sid main
deb tor+http://sdscoq7snqtznauu.onion/torproject.org sid main
```

Copy (and modify according to your needs) the site files in var/www to /var/www and the configuration files in etc to /etc after installation has finished. Then restart tor:
```
service tor restart
```

Now there should be an onion domain in /var/lib/tor/hidden_service/hostname:
```
cat /var/lib/tor/hidden_service/hostname
```

Replace the default domain with your domain in the following files:
```
/etc/nginx/sites-enabled/default
/etc/postfix/sql/alias.cf
/etc/postfix/sender_login_maps
/etc/postfix/main.cf
/var/www/skel/www/index.hosting.html
/var/www/common.php
/etc/postfix/canonical
/etc/postfix-clearnet/canonical
```

In /etc/postfix(-clearnet)/canonical don't change the line that has hosting.danwin1210.me in it. It is a clearnet/tor address rewriting rule, and if you have your own clearnet domain, you should copy this and modify your copy to preserve sending mail to my host via tor and not via clearnet:

To allow sasl authentication add postfix to the sasl group:
```
usermod -aG sasl postfix
```

This setup has two postfix instances, one for receiving and sending mail to other .onion services and one for rewriting addresses to pass them on to a clearnet facing mail relay. You may or may not want to create the second instance by running
```
postmulti -e init
postmulti -I postfix-clearnet -e create
postmulti -i clearnet -e enable
postmulti -i clearnet -p start
```
If you created an instance, uncomment the clearnet relay related config in etc/postfix/main.cf and make sure to copy and modify the configuration files from etc/postfix-clearnet too

After copying (and modifying) the posfix configuration, you need to create databases out of the mapping files (also each time you update those files):
```
postmap /etc/postfix/canonical /etc/postfix/sender_login_maps /etc/postfix/transport
postmap /etc/postfix-clearnet/canonical /etc/postfix-clearnet/sasl_password /etc/postfix-clearnet/transport #only if you have a second instance
```

To save temporary files in memory, add the following to /etc/fstab
```
tmpfs /tmp tmpfs defaults 0 0
tmpfs /var/log/nginx tmpfs rw,user 0 0
```

If you expect a large number of registrations (10.000 or more), you should make sure your system has enough UIDs to assign. The easiest way to do so is by limiting newusers to one ID per user by adding the following to /etc/login.defs
```
SUB_GID_COUNT 1
SUB_UID_COUNT 1
```

As time syncronisation is important, you should configure ntp servers in /etc/systemd/timesyncd.conf and make them match with the entries in /etc/rc.local iptables configuration

To create all required tor and php instances run the following commands:
```
for instance in 2 3 4 5 6 7 a b c d e f g h i j k l m n o p q r s t u v w x y z; do(tor-instance-create $instance) done
for instance in default 2 3 4 5 6 7 a b c d e f g h i j k l m n o p q r s t u v w x y z; do(systemctl enable php7.0-fpm@$instance; systemctl enable php7.1-fpm@$instance; systemctl enable php7.2-fpm@$instance; systemctl enable php7.3-fpm@$instance;) done
```

For web based mail management grab the latest squirrelmail and install it in /var/www/html/squirrelmail:
```
cd /var/www/html/ && svn checkout https://svn.code.sf.net/p/squirrelmail/code/trunk/squirrelmail && cd squirrelmail && ./configure && mkdir /var/local/squirrelmail /var/local/squirrelmail/data /var/local/squirrelmail/attach && chown www-data:www-data /var/local/squirrelmail /var/local/squirrelmail/data /var/local/squirrelmail/attach
```

Once it is downloaded, it will ask you for configuration. Things to change are:
```
D. > select dovecot
2. Server Settings > 1. Domain > Set your own .onion domain here
2. Server Settings > B. Update SMTP settings > 7. SMTP Authentication -> y -> plain -> n User are authenticated using their username + password
4. General Options > 9. Allow editing of identity > n Users should not be able to fake email addresses > y They should be able to change display name > y They should be able to set a reply to mail > y additional headers are not required
10. Language settings > 4. Enable aggressive decoding
11. Tweaks > 2. Ask user info on first login > n (commonly confuses users)
11. Tweaks > 4. Use php recode functions > y
11. Tweaks > 5. Use php iconv functions > y
```

Create a mysql user with all permissions for our hosting management:
```
mysql
CREATE USER 'hosting'@'localhost' IDENTIFIED BY 'MY_PASSWORD';
GRANT ALL PRIVILEGES ON *.* TO 'hosting'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
quit
```

Then edit the database configuration in /var/www/common.php and /etc/postfix/sql/alias.cf

Last but not least setup the database by running
```
php /var/www/setup.php
``` 

Enable systemd timers to regularly run various managing tasks:
```
systemctl enable hosting-del.timer && systemctl enable hosting.timer
```

Final step is to reboot wait about 5 minutes for all services to start and check if everything is working by creating a test account.

Live demo:
----------

If you want to see the setup in action or create your own site on my server, you can visit my [TOR hidden service](http://dhosting4okcs22v.onion) or via [my clearnet proxy](https://hosting.danwin1210.me) if you don't have TOR installed.
