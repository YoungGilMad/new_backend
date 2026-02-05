````md
# new_backend (NestJS Backend)

모바일 앱 백엔드(NestJS) 프로젝트입니다.  
로컬 개발 환경에서 **Docker(PostgreSQL)** 를 표준으로 사용합니다.

---

## 0. Requirements

- macOS (Apple Silicon 포함)
- Node.js (권장: LTS, 예: 20.x)
- Docker Desktop
- VS Code (권장)

---

## 1. Project Setup

### 1) Install dependencies

```bash
npm ci
```
````

> 처음 세팅/패키지 변경 후에는 `npm ci` 또는 `npm install`을 사용합니다.

### 2) Environment variables

환경변수 파일은 **커밋하지 않습니다**.

- `.env.example` : 커밋 O (템플릿)
- `.env` : 커밋 X (로컬용)

```bash
cp .env.example .env
```

---

## 2. Database (PostgreSQL via Docker Compose)

이 프로젝트는 로컬 DB를 Docker로 실행합니다.

### 1) Start DB

```bash
npm run db:up
```

또는 직접:

```bash
docker compose up -d
```

실행 확인:

```bash
docker ps
```

예시로 `backend_postgres` 컨테이너가 `Up` 상태면 정상입니다.

### 2) View DB logs (중요)

로그 확인은 아래 중 하나로 합니다.

✅ 권장 (npm 스크립트):

```bash
npm run db:logs
```

✅ 직접 실행:

```bash
docker compose logs -f postgres
```

✅ 컨테이너 이름으로:

```bash
docker logs -f backend_postgres
```

> ❌ 주의: `docker run db:logs` 는 "이미지 실행" 명령이라서 로그 확인용이 아닙니다.
> `docker run db:logs`를 입력하면 `db:logs`라는 이미지를 찾으려다 오류가 납니다.

### 3) Stop DB

```bash
npm run db:down
```

또는:

```bash
docker compose down
```

### 4) Connect to Postgres (optional)

로컬에 psql이 없어도 컨테이너 내부에서 접속할 수 있습니다.

```bash
docker exec -it backend_postgres psql -U postgres -d app
```

접속 후 테스트:

```sql
SELECT 1;
\q
```

---

## 3. Run Server

개발 모드 실행:

```bash
npm run start:dev
```

기본 포트는 `.env`의 `PORT` 값을 따릅니다(예: 3000).

---

## 4. Prisma (DB Schema / Migration)

> Prisma는 DB 스키마/마이그레이션 기반으로 개발하기 위한 도구입니다.

### 1) Install & init (최초 1회)

```bash
npm i prisma @prisma/client
npx prisma init
```

### 2) DB 연결 확인

DB가 실행 중인 상태에서:

```bash
npx prisma db push
```

에러 없이 종료되면 `DATABASE_URL` 연결이 정상입니다.

---

## 5. Recommended VS Code Setup

권장 확장:

- ESLint
- Prettier

권장 설정(`.vscode/settings.json`) 예시는 레포에 포함합니다(있는 경우).

---

## 6. Scripts

자주 쓰는 명령어:

```bash
npm run start:dev     # dev server
npm run build         # build
npm run test          # unit tests
npm run lint          # lint

npm run db:up         # start postgres via docker compose
npm run db:down       # stop postgres
npm run db:logs       # follow postgres logs
```

---

## 7. Troubleshooting

### Q1. `docker run db:logs` 를 쳤더니 "pull access denied"가 나옵니다.

- `docker run`은 이미지를 실행하는 명령입니다. 로그 확인이 목적이면 아래 중 하나를 사용하세요:

```bash
npm run db:logs
# 또는
docker compose logs -f postgres
# 또는
docker logs -f backend_postgres
```

### Q2. DB 포트(5432)가 이미 사용 중이라고 나옵니다.

- 다른 Postgres가 이미 실행 중일 수 있습니다.
- 사용 중인 프로세스를 종료하거나, `docker-compose.yml`에서 포트를 변경하세요.

### Q3. `npx prisma db push`가 실패합니다.

- 우선 DB 컨테이너가 실행 중인지 확인:

```bash
docker ps
```

- `.env`의 `DATABASE_URL`이 올바른지 확인:
  - 예: `postgresql://postgres:postgres@localhost:5432/app?schema=public`

---

## 8. Remote Repository

원격 레포:

- [https://github.com/YoungGilMad/new_backend.git](https://github.com/YoungGilMad/new_backend.git)

```

```
