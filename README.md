# docker-deploy-practice

## 🎯 목표
✅ **Spring Boot JAR 파일을 Docker Image로 빌드**  

✅ **Docker Image를 최적화하여 Deploy 효율성 고려**

<br>

## 🔍 Docker Image가 필요한 이유
**1️⃣ Image 크기 감소** : 불필요한 파일 및 라이브러리 제거 및 경량 베이스 이미지 사용

**2️⃣ 빌드 및 배포 속도 향상** : 작은 이미지는 빠르게 다운로드 및 배포 가능 

**3️⃣ 컨테이너 실행 속도 및 성능 향상** : 적은 리소스로 컨테이너 실행 가능 (RAM, CPU 사용 절감)

<br>

## 1. JAR 생성

## 2. Dockerfile 작성
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

<br>

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

### 🚀 eclipse-temurin:17-jre vs. eclipse-temurin:17-jre-alpine 차이점
| 이미지 | 설명 | 크기 |
|--------|------|------|
| **eclipse-temurin:17-jre** | Debian 기반, 안정적이고 범용적으로 사용 가능 | 🚀 약 **157MB** |
| **eclipse-temurin:17-jre-alpine** | Alpine Linux 기반, 초경량 버전 | ⚡ 약 **43MB** |

**1️⃣ 운영체제 기반**
- `eclipse-temurin:17-jre` → **Debian 기반**
- `eclipse-temurin:17-jre-alpine` → **Alpine Linux 기반 (가벼움)**

**2️⃣ 컨테이너 크기**
- `eclipse-temurin:17-jre` → **157MB** (Debian 기반으로 안정적)
- `eclipse-temurin:17-jre-alpine` → **43MB** (초경량)

**3️⃣ 패키지 관리자**
- `eclipse-temurin:17-jre` → `apt` 사용 (Debian 기반)
- `eclipse-temurin:17-jre-alpine` → `apk` 사용 (Alpine Linux 기반)

**4️⃣ 성능 및 호환성**
- **Alpine은 `musl` 라이브러리를 사용** (`glibc` X)
- 일부 Java 애플리케이션이 **Alpine에서 제대로 실행되지 않을 수 있음**
- **특히, JNI(Java Native Interface) 기반의 네이티브 라이브러리 사용 시 문제 발생 가능**

## 3. Docker Container 실행 정리 






