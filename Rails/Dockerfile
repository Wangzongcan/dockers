FROM phusion/passenger-ruby32

ENV HOME=/root APP_PATH=/home/app/www/current

CMD ["/sbin/my_init"]

COPY ./configs /configs

RUN /etc/my_init.d/00_regen_ssh_host_keys.sh \
 && passwd -u app

# Setting Nginx
RUN cp -f /configs/nginx/nginx-site-default.conf /etc/nginx/sites-available/default \
 && sed -i 's/www-data/app/' -i /etc/nginx/nginx.conf \
 && rm -f /etc/nginx/conf.d/mod-http-passenger.conf

RUN rm -f \
    # Enable services
    /etc/service/sshd/down \
    /etc/service/nginx/down

RUN cp -R /configs/puma /etc/service \
 && chmod +x /etc/service/puma/run

RUN npm i -g yarn

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22
