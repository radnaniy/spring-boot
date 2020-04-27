FROM openjdk:8u252-slim
EXPOSE 8080
#COPY target/spring-boot-sample-web-ui-2.1.14.BUILD-SNAPSHOT.jar .
COPY /var/lib/jenkins/workspace/spring_boot_project/spring-boot-samples/spring-boot-sample-web-ui/target/spring-boot-sample-web-ui-2.1.14.BUILD-SNAPSHOT.jar .
CMD java -jar ./spring-boot-sample-web-ui-2.1.14.BUILD-SNAPSHOT.jar