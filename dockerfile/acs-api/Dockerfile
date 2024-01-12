FROM aicyber2023/java-jdk-ttf:8.1
#RUN mkdir /opt/app
ADD  digital-employee-admin.jar /opt/app
WORKDIR /opt/app
EXPOSE 28005 
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ENTRYPOINT ["java","-jar","digital-employee-admin.jar"]
