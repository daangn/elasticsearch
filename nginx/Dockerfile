# http://blog.eagerelk.com/securing-elasticsearch-using-nginx-as-a-proxy/
FROM nginx:1.9.3

# https://github.com/kreuzwerker/envplate
COPY ep /usr/local/bin/ep
RUN chmod +x /usr/local/bin/ep

COPY conf/search.htpasswd /etc/nginx/search.htpasswd
COPY conf/nginx.conf /etc/nginx/nginx.conf

CMD ["/usr/local/bin/ep", "-v", "/etc/nginx/nginx.conf", "/etc/nginx/search.htpasswd", "--", "/usr/sbin/nginx"]
