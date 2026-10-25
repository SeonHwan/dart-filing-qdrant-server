# dart-filing-qdrant-server

[`dart-filing-agent`](https://github.com/SeonHwan/dart-filing-agent) 검색 파이프라인 프로젝트의 벡터DB(Qdrant) 서버 쪽만
분리한 저장소. 클라이언트(임베딩 계산, 검색, 에이전트 로직)와 서버(벡터DB)를 물리적으로도
분리하는 구성을 보여주기 위한 목적이며, 실제 운용은 로컬 Docker로 한다 — 상시 호스팅은 하지
않고, 개발하거나 시연할 때만 띄운다. 데이터는 Docker 볼륨에 남아서 컨테이너를
내렸다 올려도 유지된다.

## 실행

```bash
docker compose up -d
```

- REST API: http://localhost:6333
- 헬스체크: `curl http://localhost:6333/healthz` (`200 OK`)
- 대시보드(웹 UI): http://localhost:6333/dashboard
- 버전 확인: `curl -s http://localhost:6333` (`docker-compose.yml`의 이미지 태그와 같아야 한다)

**이 컴퓨터에서만 접속된다**(`127.0.0.1`에 바인딩). 인증이 없는 구성이라 다른 기기에서 접속할 수
있으면 안 된다. gRPC(6334)는 클라이언트가 쓰지 않아 열지 않는다.

## 종료

```bash
docker compose down       # 컨테이너만 내림, 데이터(볼륨)는 유지
docker compose down -v    # 데이터까지 완전 삭제할 때만
```

## 클라이언트 연동

`dart-filing-agent` 쪽 `.env`에 `QDRANT_URL=http://localhost:6333`만 설정하면 로컬에서
접속 가능(인증 없음 — 로컬 전용 구성이라 API 키는 안 씀).
