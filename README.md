# 도커 처음 시작

## Dockerfile 기본 구성 요소

1. **`FROM`** – 베이스 이미지 선택
2. **`WORKDIR`** – 작업 디렉터리 지정
3. **`COPY`** – 소스 파일 복사
4. **`RUN`** – 쉘 명령 실행 (설치 등)
5. **`ENV`** – 환경 변수 설정 (선택)
6. **`EXPOSE`** – 노출할 포트
7. **`CMD` 또는 `ENTRYPOINT`** – 컨테이너 시작 시 실행할 명령

---

## 📘 Node.js 애플리케이션 예시

```dockerfile
# 1. 베이스 이미지
FROM node:18-alpine

# 2. 컨테이너 내 작업 디렉터리 설정
WORKDIR /app

# 3. package.json과 package-lock.json 복사
COPY package*.json ./

# 4. 의존성 설치
RUN npm install --production

# 5. 소스 파일 복사
COPY . .

# 6. 포트 노출
EXPOSE 3000

# 7. 앱 실행
CMD ["node", "index.js"]
```

---

## 📗 Spring Boot 애플리케이션 예시 (JAR 파일)

```dockerfile
# 1. 베이스 이미지
FROM eclipse-temurin:17-jdk-alpine

# 2. 작업 디렉터리 설정
WORKDIR /app

# 3. JAR 파일 복사
COPY target/myapp.jar app.jar

# 4. 포트 노출
EXPOSE 8080

# 5. 앱 실행
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## Dockerfile 작성 팁

| 항목                    | 설명                                    |
| --------------------- | ------------------------------------- |
| `alpine`              | 가볍고 빠른 베이스 이미지 (단, glibc 없는 문제 주의)    |
| `COPY` 순서             | 캐시 최적화를 위해 `package.json` → 전체 소스 순서로 |
| `CMD` vs `ENTRYPOINT` | `CMD`는 디폴트 명령, `ENTRYPOINT`는 고정 실행 지점 |
| `RUN` 최소화             | 이미지 크기 줄이고 캐시 최적화 가능                  |

## Docker 명령어
docker init  
docker build -t nodeserver:v1.0 .  
