FROM nginx:1.13.8-alpine

LABEL image=<%= image %>:stable \
      maintainer="<%= authorName %> <<%= authorEmail %>> (<%= authorUrl %>)" \
      base=alpine:3.5

RUN apk add --no-cache \
      supervisor

COPY stable/config/ /tmp/config

RUN mkdir -p /usr/local/sbin && \
    mv /tmp/config/entrypoint.sh /usr/local/sbin/entrypoint.sh && \
    mv /tmp/config/supervisord.conf /etc/supervisord.conf && \
    mv /tmp/config/nginx.conf /etc/nginx/conf.d/default.conf && \
    chmod -R +x /usr/local/sbin && \
    rm -rf /tmp/* /tmp/.* &>/dev/null || true

WORKDIR /var/www/html

COPY stable/html /var/www/html

RUN chown -R nobody:nobody /var/www/html && \
    find /var/www/html -type f -exec chmod 644 {} \; && \
    find /var/www/html -type d -exec chmod 777 {} \;

EXPOSE 8080

ENTRYPOINT ["/bin/sh", "/usr/local/sbin/entrypoint.sh"]
