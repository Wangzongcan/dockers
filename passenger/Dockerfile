FROM phusion/passenger-customizable:0.9.19

RUN apt-get update
RUN /pd_build/utilities.sh
RUN /pd_build/ruby-2.3.*.sh
RUN /pd_build/nodejs.sh

RUN apt-get install -y imagemagick libmagickwand-dev

# SSH
RUN sed 's/#ClientAliveInterval 0/ClientAliveInterval 60/' -i /etc/ssh/sshd_config
RUN rm -f /etc/service/sshd/down
EXPOSE 22

# App
RUN passwd -u app
RUN usermod -a -G docker_env app
RUN apt-get install -y rsync

# Nginx and Passenger
RUN sed 's/www-data/app/' -i /etc/nginx/nginx.conf
ADD nginx-passenger.conf /etc/nginx/conf.d/passenger.conf
RUN rm -f /etc/nginx/sites-available/default
ADD nginx-site-default.conf /etc/nginx/sites-available/default
RUN rm -f /etc/service/nginx/down

# Cleanup
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
