# 1️⃣ 빌드 단계
FROM gradle:7.4.0-jdk AS builder

WORKDIR /workspace

# gradle 관련 파일 복사
COPY gradle gradle
COPY gradlew gradlew
COPY build.gradle settings.gradle ./

# 소스코드 복사
COPY src src

# jar 빌드
RUN chmod +x gradlew && ./gradlew bootJar --no-daemon


# 2️⃣ 실행 단계
# amazoncorretto 이미지는 arm64 변형이 없어
# exec format error가 발생할 수 있어
# 멀티 아키텍처 지원 eclipse-temurin 사용
FROM eclipse-temurin:8-jre

# 메시지 저장 디렉토리 생성
RUN mkdir -p /usr/app/msa-attach-volume/messages

WORKDIR /usr/app

# 빌드 단계에서 생성된 jar 복사
COPY --from=builder /workspace/build/libs/*.jar app.jar

# 서비스 포트
EXPOSE 8000

# 컨테이너 실행 명령
ENTRYPOINT ["java", "-jar", "/usr/app/app.jar"]


## openjdk8 base image
#FROM eclipse-temurin:8-jre
#
## directory 생성
#RUN mkdir -p /usr/app/msa-attach-volume/messages
## jar 파일이 복사되는 위치
#ENV APP_HOME=/usr/app/
## 작업 시작 위치
#WORKDIR $APP_HOME
## jar 파일 복사
#COPY build/libs/*.jar apigateway.jar
## application port
#EXPOSE 8000
## 실행
#CMD ["java", "-Dspring.profiles.active=${profile:default}", "-jar", "apigateway.jar"]
