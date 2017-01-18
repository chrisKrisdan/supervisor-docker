FROM ubuntu:16.04
MAINTAINER perkris <chris.krisdan@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

# Set correct environment variables
ENV HOME /root


MAINTAINER chris.krisdan@google.com

# Install Apache and SuperVisor
RUN apt-get update && apt-get install -qy apache2 apache2-utils supervisor
RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/log/supervisor

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Update apache configuration with this one
ADD apache-config.conf /etc/apache2/sites-available/000-default.conf
ADD apache-ports.conf /etc/apache2/ports.conf
RUN echo "ServerName localhost" > /etc/apache2/conf-available/fqdn.conf

### Add Shell Script for starting Apache2:
RUN a2enconf fqdn
RUN service apache2 start

# USE PORT 8080
EXPOSE 8080

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Start Supervisor to manage processes
CMD ["/usr/bin/supervisord"]