# Build Stage
FROM eclipse-temurin:21-jdk-jammy as build

WORKDIR /workspace/app

# common-build 서브모듈의 내용물 복사
COPY common-build/ .
 
# gateway 레포지토리 코드 복사
COPY . ./gateway

# gradlew 실행 권한 부여
RUN chmod +x ./gradlew

# gateway 하위 프로젝트를 빌드합니다.
RUN ./gradlew :gateway:build -x test

# 실행 가능한 fat jar 파일의 이름을 app.jar로 변경합니다.
RUN mv /workspace/app/gateway/build/libs/gateway.jar /workspace/app/gateway/build/libs/app.jar

# Package Stage
FROM eclipse-temurin:21-jdk-jammy
WORKDIR /app
# 이름이 변경된 app.jar 파일을 복사합니다.
COPY --from=build /workspace/app/gateway/build/libs/app.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
