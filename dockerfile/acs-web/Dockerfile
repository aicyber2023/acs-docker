FROM nginx:1.23
RUN  rm -f /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/
COPY acs_nginx.conf /etc/nginx/conf.d/
COPY customer-service-chat /opt/customer-service-chat
COPY customer-service-server /opt/customer-service-server
EXPOSE 28001 28002
CMD ["nginx","-g","daemon off;"]
