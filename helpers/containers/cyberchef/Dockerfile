# This will build a copy of cyberchef on alpine linux with lighttpd as the server
# To run:
#         docker run -d -p 8080:80 chef2
#
FROM alpine:3.7

RUN apk --update add git \
  nodejs nodejs-npm \
  lighttpd lighttpd-mod_auth --no-cache \
  && rm -rf /var/cache/apk/*

RUN npm install -g grunt-cli

RUN git clone https://github.com/gchq/CyberChef.git
WORKDIR /CyberChef
RUN npm install
RUN grunt prod

RUN cp -r /CyberChef/build/prod/* /var/www/localhost/htdocs/

RUN rm -rf /CyberChef

RUN chown -R lighttpd:lighttpd /var/www/localhost/htdocs/

EXPOSE 80

CMD ["lighttpd", "-D", "-f", "/etc/lighttpd/lighttpd.conf"]
