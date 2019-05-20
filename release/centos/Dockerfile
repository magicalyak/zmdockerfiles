FROM centos:7
LABEL maintainer Andrew Bauer <zonexpertconsulting@outlook.com>

# Get the entrypoint script
ADD https://raw.githubusercontent.com/ZoneMinder/zmdockerfiles/master/utils/entrypoint.sh /usr/local/bin/

## Dependencies layer
RUN \
  # Enable the EPEL repo. The repo package is part of centos base so no need fetch it.
  yum -y install epel-release \
  # Fetch and enable the RPMFusion repo
  && yum -y localinstall --nogpgcheck https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm \
  # Install dependencies
  && yum -y install mariadb-server mod_ssl zip file \
  # Clean up yum cache
  && yum clean all \
  # Make sure the entrypoint is executable
  && chmod 755 /usr/local/bin/entrypoint.sh

## ZoneMinder layer
RUN \
  # Install the latest *release* of zoneminder
  yum -y install zoneminder \
  # Clean up yum cache
  && yum clean all \
  # Configure Apache
  && ln -sf /etc/zm/www/zoneminder.conf /etc/httpd/conf.d/ \
  && echo "ServerName localhost" > /etc/httpd/conf.d/servername.conf \
  && echo -e "# Redirect the webroot to /zm\nRedirectMatch permanent ^/$ /zm" > /etc/httpd/conf.d/redirect.conf

# Set our volumes before we do anything else
VOLUME /var/lib/zoneminder/events /var/lib/mysql /var/log/zoneminder

# Expose https port
EXPOSE 443

# This is run each time the container is started
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]