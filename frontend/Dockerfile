FROM nginx:1.24.0

RUN rm -rf /usr/share/nginx/html/*
RUN rm -f /etc/nginx/conf.d/default.conf

COPY static/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf