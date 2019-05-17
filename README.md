### Zoneminder 1.33

This was pulled from the https://github.com/ZoneMinder/zmdockerfiles/blob/master/development/ubuntu/xenial/Dockerfile
Almost all the information here was taken from that repo above and I'm sure I'm not crediting them correctly

#### Install on most platforms

On other platforms, you can run this docker container with the following command:

`docker run -d --name="zoneminder" --privileged=true -v /path/to/config:/config:rw -v /etc/localtime:/etc/localtime:ro -p 80:80 magicalyak/docker-zoneminder`

#### Tips and Setup Instructions:

ZoneMinder uses /dev/shm for shared memory and many users will need to increase the size significantly at runtime like so:

```sh
docker run -d -t -p 1080:443 \
    --shm-size="512m" \
    --name zoneminder \
    magicalyak/zoneminder
```

ZoneMinder checks the TZ environment variable at runtime to determine the timezone.
If this variable is not set, then ZoneMinder will default to UTC.
Alternaitvely, the timezone can be set manually like so:

```sh
docker run -d -t -p 1080:443 \
    -e TZ='America/Los_Angeles' \
    --name zoneminder \
    magicalyak/zoneminder
```

ZoneMinder can write its data to folders outside the container using volumes.

```sh
docker run -d -t -p 1080:443 \
    -v /disk/zoneminder/events:/var/lib/zoneminder/events \
    -v /disk/zoneminder/mysql:/var/lib/mysql \
    -v /disk/zoneminder/logs:/var/log/zm \
    --name zoneminder \
    magicalyak/zoneminder
```

ZoneMinder can use an external database by setting the appropriate environment variables.

```sh
docker run -d -t -p 1080:443 \
    -e ZM_DB_USER='zmuser' \
    -e ZM_DB_PASS='zmpassword' \
    -e ZM_DB_NAME='zoneminder_database' \
    -e ZM_DB_HOST='my_central_db_server' \
    -v /disk/zoneminder/events:/var/lib/zoneminder/events \
    -v /disk/zoneminder/logs:/var/log/zm \
    --name zoneminder \
    magicalyak/zoneminder
```

Here is an example using the options described above with the internal database:

```sh
docker run -d -t -p 1080:443 \
    -e TZ='America/Los_Angeles' \
    -v /disk/zoneminder/events:/var/lib/zoneminder/events \
    -v /disk/zoneminder/mysql:/var/lib/mysql \
    -v /disk/zoneminder/logs:/var/log/zm \
    --shm-size="512m" \
    --name zoneminder \
    magicalyak/zoneminder
```

Here is an example using the options described above with an external database:

```sh
docker run -d -t -p 1080:443 \
    -e TZ='America/Los_Angeles' \
    -e ZM_DB_USER='zmuser' \
    -e ZM_DB_PASS='zmpassword' \
    -e ZM_DB_NAME='zoneminder_database' \
    -e ZM_DB_HOST='my_central_db_server' \
    -v /disk/zoneminder/events:/var/lib/zoneminder/events \
    -v /disk/zoneminder/logs:/var/log/zm \
    --shm-size="512m" \
    --name zoneminder \
    magicalyak/zoneminder
```

#### Important:
- The web gui will be available at http://serverip:port/zm
- On first start, open zoneminder settings, go to the paths tab and enter the following for PATH_ZMS: ```/zm/cgi-bin/nph-zms```
- The default timezone for php is set as America/New_York if you would like to change it, edit the php.ini in the config folder. Here's a list of available timezone options: http://php.net/manual/en/timezones.php

#### Changelog:
- 2019-05-17 - Updated to official docker image and 1.33 
- 2016-03-26 - Fixed the images, events and temp folder paths
- 2016-03-14 - Fixed the Cambozola location
- 2016-03-10 - Release
- 2016-02-02 - forked from tokyis and aptalca to magicalyak for 1.30
