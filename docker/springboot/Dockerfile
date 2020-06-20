FROM openjdk:8

RUN git clone https://github.com/kayamin/handson_springboot.git
WORKDIR ./handson_springboot

EXPOSE 8080

RUN ./gradlew build

CMD ./gradlew bootRun