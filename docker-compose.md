
# docker-compose.yml

`docker-compose.yml` 파일은 여러 컨테이너(서비스)를 하나의 파일로 정의하고 관리할 수 있게 해주는 구성 파일입니다.
기본적으로 **어떤 이미지를 사용할지, 어떤 포트를 열지, 환경 변수나 볼륨은 어떤 걸 설정할지** 등을 정의합니다.


## 기본 구조 예시

```yaml
version: '3.8'  # 사용할 Compose 파일 버전

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"   # 호스트:컨테이너 포트
    volumes:
      - ./html:/usr/share/nginx/html
    restart: always

  app:
    build: ./app   # 해당 디렉토리에 Dockerfile이 있어야 함
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    depends_on:
      - db

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: myapp
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

## 주요 섹션 설명

| 키             | 설명                                                  |
| ------------- | --------------------------------------------------- |
| `version`     | Compose 파일 포맷 버전 (보통은 `'3.8'` 또는 `'3'`)             |
| `services`    | 컨테이너 단위로 구성 요소 정의                                   |
| `build`       | Dockerfile이 있는 디렉토리 경로                              |
| `image`       | 사용할 도커 이미지 (`nginx`, `mysql`, `yourname/app:tag` 등) |
| `ports`       | 포트 매핑 (호스트:컨테이너)                                    |
| `volumes`     | 호스트 ↔ 컨테이너 간 파일 공유 설정                               |
| `environment` | 환경 변수 설정 (배열 또는 딕셔너리 형태)                            |
| `depends_on`  | 다른 컨테이너가 먼저 실행되도록 순서 지정                             |
| `restart`     | 컨테이너 자동 재시작 정책 (예: `always`, `on-failure`)          |
| `volumes:`    | 서비스 간 공유되는 이름있는 볼륨 정의                               |

---

## 실제 상황별 예시

* Node.js + MongoDB:
* Spring Boot + MySQL:
* Nginx + React 앱 프록시 구성: