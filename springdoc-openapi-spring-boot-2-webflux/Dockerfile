FROM ghcr.io/graalvm/graalvm-ce:ol8-java11-21.1.0 as deps
  
RUN microdnf install maven

WORKDIR /opt/app
COPY springdoc-openapi-spring-boot-2-webflux/pom.xml springdoc-openapi-spring-boot-2-webflux/pom.xml
COPY pom.xml .

WORKDIR springdoc-openapi-spring-boot-2-webflux
RUN mvn dependency:go-offline

FROM ghcr.io/graalvm/graalvm-ce:ol8-java11-21.1.0 as builder

COPY --from=deps /root/.m2 /root/.m2
COPY --from=deps /opt/app/ /opt/app
COPY springdoc-openapi-spring-boot-2-webflux/src /opt/app/springdoc-openapi-spring-boot-2-webflux/src

RUN microdnf install zip
RUN microdnf install curl
RUN microdnf install maven
RUN gu install native-image

WORKDIR /opt/app/springdoc-openapi-spring-boot-2-webflux

RUN ls -rtla

# copy your other files
RUN mvn -Pnative-image clean package

FROM oraclelinux:8-slim
WORKDIR /opt
COPY --from=builder "/opt/app/springdoc-openapi-spring-boot-2-webflux/target/org.springdoc.demo.app3.webfluxdemoapplication" org.springdoc.demo.app3.webfluxdemoapplication
CMD [ "sh", "-c", "./org.springdoc.demo.app3.webfluxdemoapplication" ]

