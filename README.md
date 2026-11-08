# GTA 네온 하버 ONLINE — 새로 배포하기

이 저장소에는 **파일 4개만** 있으면 됩니다. 폴더는 없습니다.

| 파일 | 역할 |
|---|---|
| `worker.js` | 게임 화면(HTML·CSS·JS) + 멀티플레이 서버(방·채팅·동기화)가 모두 들어 있는 파일 |
| `wrangler.json` | Cloudflare 설정 (Durable Object `GameRoom`, SQLite 저장소) |
| `package.json` | 배포 도구(wrangler) 버전 |
| `README.md` | 이 설명서 |

## 1. GitHub 저장소 만들기
1. github.com → 오른쪽 위 **+** → **New repository**
2. Repository name: **`neon-harbor`** → **Create repository**
3. 나온 화면에서 **uploading an existing file** 클릭
4. 파일 4개(`worker.js`, `wrangler.json`, `package.json`, `README.md`)를 한꺼번에 끌어다 놓기 → **Commit changes**
5. 저장소 첫 화면에 파일 4개가 **나란히** 보이면 성공

## 2. Cloudflare에 연결
1. dash.cloudflare.com → 왼쪽 **Workers & Pages** (Compute) → **Create application**
2. **Import a repository** (Git 저장소 가져오기) → GitHub 연결 → **`neon-harbor`** 선택
3. 설정 화면에서
   - **Project name**: `neon-harbor` (wrangler.json 의 name 과 같아야 함)
   - **Build command**: 비워 두기
   - **Deploy command**: `npx wrangler deploy`
   - **Root directory / Path**: 비워 두기 (또는 `/`)
4. **Create and deploy** (배포하기)

1~2분 뒤 `https://neon-harbor.<내계정>.workers.dev` 주소가 생깁니다. 이 주소가 **게임 주소**입니다.

## 3. 확인
- 게임 주소 → 로비(닉네임 · 방 만들기 · 방 코드로 참가)가 나오면 성공
- `게임주소/api/health` → `{"ok":true,...}` 가 나오면 서버도 정상
- 친구에게는 게임 주소나, 방을 만든 뒤 나오는 **초대 링크**를 보내면 됩니다.

## 조작
- `Enter` / `T` 채팅 · `Tab` 접속자 목록 · 나머지는 원작과 같음
- 채팅 명령어: `/help` · `/who` · `/nick 새닉네임` · `/pvp on|off` (방장)

## 오류가 나면
| 로그 내용 | 해결 |
|---|---|
| `entry-point file ... was not found` | 저장소 맨 위에 `worker.js` 가 있는지, Root directory 가 비어 있는지 확인 |
| `name` 이 다르다는 오류 | `wrangler.json` 의 `"name"` 을 Cloudflare 프로젝트 이름과 똑같이 맞추기 |
| `exports` / Durable Object 관련 오류 | 로그 전체를 복사해서 문의 |

## 무료 플랜
Durable Objects 는 SQLite 저장소만 사용하므로 Workers 무료 플랜에서 동작합니다. 위치 정보는 움직일 때 초당 10회, 가만히 있을 때 초당 1회만 보내 무료 한도를 아낍니다.
