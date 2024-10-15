FROM nginx:1.24

RUN groupadd -g 1000 app \
 && useradd -g 1000 -u 1000 -d /var/www -s /bin/bash app
RUN touch /var/run/nginx.pid
RUN mkdir /sock

RUN chown -R app:app /sock /var/cache/nginx/ /var/run/nginx.pid

COPY ./default.conf /etc/nginx/conf.d/

EXPOSE 80

USER app:app
WORKDIR /var/www
