# Ubuntu web server custom configuration with EasyEngine

This is step by step guide of my initial web server configuration with EasyEngine, on a clean Ubuntu 16.04 LTS installation.
Do not hesitate to share your tips or configurations by opening an issue or with a pull request.

--------

**1) System update and packages cleanup**

```
apt-get update && apt-get upgrade -y && apt-get autoremove -y && apt-get clean
```

**2) Install useful packages**
```
apt install haveged curl git unzip zip fail2ban htop
```

**3) Tweak Kernel sysctl configuration**
```
sysctl -e -p <(curl -Ss https://git.virtubox.net/virtubox/debian-config/raw/master/etc/sysctl.conf)
```

**4) Set your email instead of root@localhost**
```
echo "root: my.email@address.com" >> /etc/aliases
newaliases
```

**5) Install netdata monitoring**
```
bash <(curl -Ss https://my-netdata.io/kickstart-static64.sh) all
```

**6) Install MariaDB 10.2** <br>
Follow instructions available in my [KnowledgeBase article](https://kb.virtubox.net/knowledgebase/install-latest-mariadb-release-easyengine/) 

```
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup \
| sudo bash -s -- --mariadb-server-version=10.2 --skip-maxscale
apt update
apt install mariadb-server
```

**7) Install EasyEngine**
```
wget -qO ee rt.cx/ee && bash ee
ee stack install
sudo wp --allow-root cli update --nightly
```

**8) Install Composer and Fix phpmyadmin error 
```
bash <(wget --no-check-certificate -O - https://git.virtubox.net/virtubox/debian-config/raw/master/composer.sh)
ee stack install --phpmyadmin
sudo -u www-data composer update -d /var/www/22222/htdocs/db/pma/
```




