# docker-deploy-practice

### 🎯 목표
✅ **Spring Boot JAR 파일을 Docker Image로 빌드**  
✅ **Docker Image를 최적화하여 Deploy 효율성 고려**


### 1. JAR 생성

### 2. Dockerfile 작성
#### 🐋 방법 1) Eclipse Temurin JRE 17 기반 Alpine 사용
```
# 실행 환경: JRE 17이 포함된 경량 Alpine 기반 이미지 사용
FROM eclipse-temurin:17-jre-alpine

# 컨테이너 내부에서 작업할 디렉토리 설정 (/app)
WORKDIR /app

# 호스트의 JAR 파일을 컨테이너 내부로 복사 (Spring Boot 애플리케이션 실행을 위해)
COPY step01_basic-0.0.1-SNAPSHOT.jar app.jar

# 로그 저장을 위한 디렉토리 생성
RUN mkdir -p /app/logs

# 컨테이너 실행 시 Spring Boot 애플리케이션을 자동으로 실행
CMD ["sh", "-c", "java -jar app.jar --server.port=8080 > /app/logs/app.log 2>&1"]
```
- **`--server.port=8080`** : Spring Boot 애플리케이션이 8080 포트에서 실행되도록 설정
    - 실행 시 포트를 동적으로 지정하고 싶다면 **`--server.port=포트번호`** 옵션을 사용
- **`> /app/logs/app.log 2>&1`** : Spring Boot 애플리케이션의 로그를 파일에 저장

#### 🐋 방법 2) Eclipse Temurin JRE 17 사용
```
# 실행 환경: Eclipse Temurin JRE 17 기반의 이미지 사용
FROM eclipse-temurin:17-jre

# 컨테이너 내부에서 작업할 디렉토리 설정 (/app)
WORKDIR /app

# 호스트의 JAR 파일을 컨테이너 내부로 복사 (Spring Boot 애플리케이션 실행을 위해)
COPY step01_basic-0.0.1-SNAPSHOT.jar app.jar

# 컨테이너 실행 시 Spring Boot 애플리케이션을 자동으로 실행
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### 3. Docker Container 실행 정리 





### eclipse-temurin:17-jre vs. eclipse-temurin:17-jre-alpine 차이점
