# 🏠 온프레미스 도커 기반 서버 구축 가이드

이 프로젝트는 온프레미스 환경에서 서버를 구축하기 위한 Docker Compose 설정 파일을 제공합니다. Nginx Proxy Manager, Cloudflare DDNS, Portainer, Prometheus, Grafana, MariaDB, Redis 등을 포함한 다양한 서비스를 손쉽게 배포할 수 있습니다.

# 🚀 시작하기

1.  리눅스 서버에 Docker 및 Docker Compose 설치

    https://docs.docker.com/engine/install/ubuntu/ 참고하여 Docker 및 Docker Compose를 설치합니다.

2.  클라우드 플레어 도메인 등록 및 API Key 발급

    https://dash.cloudflare.com/ 에서 도메인을 등록하고 API Key를 발급받습니다.

3.  Git Clone

    해당 프로젝트를 클론합니다.

    ```bash
    git clone https://github.com/cmsong111/on-premise-server-docker-setup.git
    cd on-premise-server-docker-setup
    ```

4.  docker-compose.default.yaml 파일 수정

    `docker-compose.default.yaml` 파일을 열어 `CLOUDFLARE_API_TOKEN`과 `DOMAINS` 환경 변수를 수정합니다.

5.  docker-compose.default.yaml 파일 실행

    ```bash
    docker-compose -f docker-compose.default.yaml \
                -f docker-compose.monitor.yaml \
                -f docker-compose.database.yaml up -d
    ```

6.  Nginx Proxy Manager 설정

    `http://<서버 IP>:81`로 접속하여 Nginx Proxy Manager에 로그인합니다.

    기본 계정은

    - `email`: `admin@example.com`
    - `password`: `changeme`

    로그인 후 비밀번호를 변경하고, 도메인을 추가하여 서비스를 배포합니다.

    SSL 인증서 발급 시 Cloudflare DNS API를 이용하여, Let's Encryptd Dns-01 방식으로 인증서를 발급합니다.
    (와일드카드 인증서를 발급하기 위해 DNS-01 방식을 사용합니다.)

    네트워크를 `proxy-network` 로 설정했기 때문에, 도커 DNS를 이용하여 컨테이너명으로 접근할 수 있습니다.

    ex) `portainer.example.com`-> `http://portainer:9000` 으로 프록싱

    기타 서비스들도 동일하게 설정합니다.

# 📌 구성 요소

## Defailt

1. Nginx Proxy Manager

   웹 기반 UI를 제공하는 리버스 프록시 관리자

   SSL 인증서 자동 갱신 및 관리 기능 포함

   기본 포트: 80, 81, 443

2. Cloudflare DDNS

   동적 IP 환경에서 Cloudflare를 이용한 DDNS 업데이트 지원

3. Portainer

   직관적인 UI를 제공하는 Docker 컨테이너 관리 도구

4. Watchtower

   자동 컨테이너 업데이트 관리

   설정된 시간 간격마다 최신 이미지 확인 및 업데이트

## Monitoring

5. Node Exporter

   서버의 시스템 메트릭을 Prometheus에 제공

6. Prometheus

   시계열 데이터 수집 및 저장

   Grafana와 연동하여 모니터링 시스템 구축

7. Grafana

   Prometheus 데이터 시각화 및 대시보드 제공

   기본 포트: 3000

### Database

8. MariaDB

   MySQL 호환 오픈소스 데이터베이스 관리 시스템

9. Redis

   인메모리 데이터 저장소 및 캐시 서비스

10. minio

    S3 호환 분산 객체 스토리지 서버
