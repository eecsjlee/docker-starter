# 도커 처음 시작
  
## Dockerfile 기본 구성 요소

1. **`FROM`** – 베이스 이미지 선택
2. **`WORKDIR`** – 작업 디렉터리 지정
3. **`COPY`** – 소스 파일 복사
4. **`RUN`** – 쉘 명령 실행 (설치 등)
5. **`ENV`** – 환경 변수 설정 (선택)
6. **`EXPOSE`** – 노출할 포트
7. **`CMD` 또는 `ENTRYPOINT`** – 컨테이너 시작 시 실행할 명령


## Node.js 애플리케이션 예시

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


## Spring Boot 애플리케이션 예시 (JAR 파일)

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
docker run -d 이미지:태그명
docker run -p 8081:8080 -d 이미지명:태그명
이미지를 실행할 때 포트를 설정해주고 싶으면 -p 넣고 내컴퓨터포트:컨테이너포트를 집어 넣으면 됨
누가 내 컴퓨터 8081 포트로 들어오면 컨테이너의 8080포트로 안내해주라는 뜻


### 컨테이너 관련 명령어
docker ps
실행중인 컨테이너들을 살펴 볼 수 있습니다.

docker logs

docker exec -it 컨테이너이름 sh

docker stop 컨테이너이름

docker rm 컨테이너이름
정지 안된 컨테이너를 삭제하려면 -f 옵션을 붙여줌

아래는 `README.md` 파일에 적절한 형식으로 정리한 **Docker 컨테이너 관련 명령어 설명**입니다. 각 명령어는 초보자도 이해할 수 있도록 자세한 설명과 함께 작성했으며, 실무에서 자주 사용되는 유용한 명령어들도 함께 추가했습니다.


## 🐳 Docker 컨테이너 관련 명령어 정리

### 1. 실행 중인 컨테이너 확인

```bash
docker ps
```

* 현재 실행 중인 컨테이너 목록을 확인할 수 있습니다.
* 컨테이너 ID, 이미지 이름, 상태, 포트 매핑 등의 정보를 확인할 수 있습니다.
* `-a` 옵션을 추가하면 **중지된 컨테이너**도 함께 표시됩니다.

```bash
docker ps -a
```

---

### 2. 컨테이너 로그 확인

```bash
docker logs [컨테이너이름 또는 ID]
```

* 컨테이너에서 출력된 로그를 확인할 수 있습니다.
* 에러 확인이나 디버깅 시 유용하게 사용됩니다.
* `-f` 옵션을 추가하면 실시간 로그를 스트리밍 형태로 볼 수 있습니다.

```bash
docker logs -f [컨테이너이름]
```

---

### 3. 실행 중인 컨테이너에 접속 (쉘)

```bash
docker exec -it [컨테이너이름 또는 ID] sh
```

* 실행 중인 컨테이너 내부에 접속하여 셸을 사용할 수 있습니다.
* 만약 `sh`가 없고 `bash`가 설치되어 있다면 `bash`를 사용합니다.

```bash
docker exec -it [컨테이너이름] bash
```

### 4. 컨테이너 중지

```bash
docker stop [컨테이너이름 또는 ID]
```

* 실행 중인 컨테이너를 정상적으로 종료합니다.


### 5. 컨테이너 삭제

```bash
docker rm [컨테이너이름 또는 ID]
```

* **중지된** 컨테이너를 삭제합니다.
* 컨테이너가 실행 중이라면 삭제되지 않으며, 이 경우 `-f` 옵션을 사용하여 강제로 삭제할 수 있습니다.

```bash
docker rm -f [컨테이너이름]
```


### 6. 사용하지 않는 컨테이너, 이미지, 볼륨, 네트워크 정리

```bash
docker system prune
```

* 필요 없는 리소스를 한번에 정리할 수 있습니다.
* `-a` 옵션을 붙이면 **중지된 컨테이너와 사용하지 않는 모든 이미지**도 삭제합니다.

```bash
docker system prune -a
```


### 7. 이미지로부터 컨테이너 생성 및 실행

```bash
docker run -d --name [컨테이너이름] -p [호스트포트]:[컨테이너포트] [이미지이름]
```

* 지정한 이미지로부터 컨테이너를 생성하고 백그라운드에서 실행합니다.
* 포트 포워딩 설정도 함께 할 수 있습니다.
  예시:

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

---

### 8. 컨테이너 상태 확인

```bash
docker inspect [컨테이너이름 또는 ID]
```

* 컨테이너의 상세 정보(환경 변수, 네트워크, 볼륨 등)를 JSON 형식으로 출력합니다.

---

### 9. 컨테이너 재시작

```bash
docker restart [컨테이너이름 또는 ID]
```

* 컨테이너를 재시작합니다.

---

### 참고

* `컨테이너이름` 대신 `컨테이너 ID`를 사용할 수도 있습니다.
* 자주 사용하는 컨테이너는 의미 있는 이름(`--name`)을 부여하면 관리가 편리합니다.
* `docker-compose`를 사용하면 여러 컨테이너를 한번에 관리할 수 있습니다.


## Docker Hub

https://hub.docker.com/

docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
docker pull image:tagname

## Docker Network
  
docker network create [네트워크 이름]
docker network
docker network ls //도커 네트워크 목록
  
docker run -d -p 80:80 --network mynet1 --name nginx-container nginx:1
docker run -d -p 8080:8080 --network mynet1 --name nodeserver-container nodeserver:v1.0
--network 네트워크 이름 // 네트워크 안에 컨테이너를 담아 달라  
--name 컨테이너 이름 // 컨테이너 이름을 마음대로 정할 수 있음  

docker exec -it 컨테이너이름 /bin/sh
curl 웹서버컨테이너ID

## Volumes
볼륨에 데이터를 저장해둘 수 있지만
데이터베이스를 굳이 컨테이너에 담아 쓰지 않는다. 쓰는 이점이 없다.

## Docker Compose
`docker compose`는 여러 컨테이너를 함께 정의하고 실행할 수 있도록 해주는 도구입니다. 아래는 자주 사용하는 `docker compose` 명령어들을 정리한 목록입니다:


### 기본 명령어

| 명령어                      | 설명                                     |
| ------------------------ | -------------------------------------- |
| `docker compose up`      | `docker-compose.yml`에 정의된 서비스들을 실행합니다. |
| `docker compose up -d`   | 백그라운드(detached) 모드로 실행합니다.             |
| `docker compose down`    | 실행 중인 모든 서비스와 네트워크, 볼륨 등을 종료 및 정리합니다.  |
| `docker compose build`   | 서비스 이미지를 빌드합니다. (`Dockerfile` 기반)      |
| `docker compose restart` | 모든 서비스를 재시작합니다.                        |
| `docker compose stop`    | 모든 서비스를 중지합니다.                         |
| `docker compose start`   | 중지된 서비스를 다시 시작합니다.                     |


### 상태 확인 및 디버깅

| 명령어                                    | 설명                                                                      |
| -------------------------------------- | ----------------------------------------------------------------------- |
| `docker compose ps`                    | 현재 상태를 확인합니다.                                                           |
| `docker compose logs`                  | 전체 서비스 로그를 확인합니다.                                                       |
| `docker compose logs -f`               | 실시간 로그를 확인합니다 (`tail -f`처럼).                                            |
| `docker compose exec [서비스명] bash`      | 실행 중인 컨테이너 내에서 bash 쉘 접속 (예: `web`, `db` 등).                            |
| `docker compose run --rm [서비스명] [명령어]` | 일회성 컨테이너 실행 (예: `docker compose run --rm web python manage.py migrate`) |


### 기타 유용한 명령어

| 명령어                     | 설명                                  |
| ----------------------- | ----------------------------------- |
| `docker compose config` | 병합된 설정 파일을 출력하여 유효성 검사를 수행합니다.      |
| `docker compose pull`   | 서비스에 필요한 이미지를 Docker Hub 등에서 가져옵니다. |
| `docker compose push`   | 빌드한 이미지를 레지스트리에 업로드합니다.             |
| `docker compose rm`     | 종료된 컨테이너를 삭제합니다.                    |

  

docker compose up -d
docker compose stop
docker compose down
docker compose --compatibility up

## 참고자료
[Reverse Proxy 정리](./reverse-proxy.md)