# MEGA RABBIT — 공식 홈페이지

정적 사이트입니다. 빌드 과정이 없고, GitHub Pages에 그대로 올리면 동작합니다.

```
.
├── index.html            홈
├── privacy.html          개인정보처리방침 (구글플레이 제출용)
├── terms.html            이용약관
├── 404.html              에러 페이지
├── CNAME                 커스텀 도메인 (mega-rabbit.com)
├── .nojekyll             Jekyll 빌드 비활성화 (필수)
├── favicon.ico
├── robots.txt
├── sitemap.xml
├── site.webmanifest
└── assets/
    ├── css/site.css      전체 스타일 (다크 전용)
    ├── fonts/            Anton, Inter (self-hosted)
    └── img/              로고, 마크, 파비콘, OG 이미지
```

---

## 1. 배포하기 전에 — `[[ ]]` 채우기

`privacy.html`과 `terms.html`에 노란 형광펜으로 표시된 자리표시자가 있습니다.
아래 값을 실제 값으로 바꾸고, `<span class="todo">` 태그도 함께 지우세요.

| 자리표시자 | 넣을 값 |
|---|---|
| `[[YOUR BUSINESS NAME]]` | 구글플레이 개발자명과 **동일하게** |
| `[[privacy@mega-rabbit.com]]` | 실제 수신 가능한 이메일 |
| `[[support@mega-rabbit.com]]` | 실제 수신 가능한 이메일 |
| `[[NAME]]`, `[[NAME OF PERSON RESPONSIBLE]]` | 개인정보 보호책임자 성함 |

한 번에 확인하는 방법:

```bash
grep -rn '\[\[' *.html
```

`index.html`의 `hello@mega-rabbit.com`도 실제 주소로 바꿔주세요.

---

## 2. GitHub에 올리기

계정 정보 (2026-08 기준):

| 항목 | 값 |
|---|---|
| 사용자명 | `mega-rabbit` |
| 커밋용 noreply 이메일 | `314542640+mega-rabbit@users.noreply.github.com` |

GitHub에서 새 저장소를 하나 만듭니다 (이름은 자유, 예: `mega-rabbit-site`).
**Public**으로 만들어야 무료 플랜에서 Pages가 동작합니다.

```bash
cd /Users/injakun/Documents/MegaRabbit/site

git init
git config user.email "314542640+mega-rabbit@users.noreply.github.com"
git config user.name "mega-rabbit"

git add .
git commit -m "MEGA RABBIT 공식 홈페이지"
git branch -M main
git remote add origin https://github.com/mega-rabbit/mega-rabbit-site.git
git push -u origin main
```

> **`git config` 두 줄을 빼먹지 마세요.** 공개 저장소의 커밋 작성자 이메일은
> `github.com/mega-rabbit/mega-rabbit-site/commit/<해시>.patch` 로 누구나 조회할 수 있습니다.
> 계정 이메일과 커밋 이메일은 별개이며, 커밋에 박히는 건 위 `git config` 값입니다.
>
> push 전 확인:
> ```bash
> git config user.email      # noreply 주소가 나와야 함
> git log --format='%ae'     # 이미 커밋했다면 여기도 확인
> ```
> 개인 이메일로 커밋이 이미 만들어졌다면 `.git` 폴더를 지우고 처음부터 다시 하는 게 가장 깔끔합니다.

---

## 3. GitHub Pages 켜기

저장소 → **Settings** → 좌측 **Pages**

- Source: `Deploy from a branch`
- Branch: `main` / `/ (root)` → **Save**

1~2분 뒤 `https://mega-rabbit.github.io/mega-rabbit-site/` 에서 확인됩니다.

---

## 4. 커스텀 도메인 연결 (mega-rabbit.com)

### 4-1. Spaceship DNS 설정

Spaceship → 도메인 → **Advanced DNS** (또는 DNS Records)에서 아래를 추가합니다.

**A 레코드 4개** — 호스트는 `@`

| Type | Host | Value |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

**CNAME 1개**

| Type | Host | Value |
|---|---|---|
| CNAME | www | `mega-rabbit.github.io` |

> 끝의 마침표(`.`)는 Spaceship이 알아서 처리합니다.
> `mega-rabbit.github.io` 뒤에 저장소 이름을 붙이지 마세요.

IPv6도 지원하려면 AAAA 레코드를 추가로 넣습니다 (선택):

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

### 4-2. GitHub 쪽 설정

Settings → Pages → **Custom domain**에 `mega-rabbit.com` 입력 후 Save.

DNS 검증에 보통 10분 ~ 1시간(길면 24시간) 걸립니다.
검증이 끝나면 **Enforce HTTPS** 체크박스가 활성화됩니다. **반드시 체크하세요** —
구글플레이는 개인정보처리방침 URL이 정상 접근 가능해야 하고, HTTPS가 아니면 문제가 됩니다.

### 4-3. 확인

```bash
dig +short mega-rabbit.com
curl -sI https://mega-rabbit.com | head -1
```

---

## 5. 구글플레이 콘솔에 넣을 URL

| 항목 | URL |
|---|---|
| 개인정보처리방침 | `https://mega-rabbit.com/privacy.html` |
| 계정 삭제 요청 | `https://mega-rabbit.com/privacy.html#deletion` |

계정 로그인 기능이 있는 앱은 **두 번째 항목이 필수**입니다. 누락하면 심사에서 반려됩니다.

---

## 6. 나중에 내용 수정하기

### 게임 정보 추가
`index.html`의 `<!-- ===== GAMES ===== -->` 섹션에서 `.card` 블록을 수정합니다.
카드 구조는 이렇습니다:

```html
<article class="card reveal">
  <span class="tag">Out now</span>
  <h3>게임 이름</h3>
  <p>한 줄 설명.</p>
</article>
```

`.card.ghost`는 "더 준비 중" 자리표시 카드라 게임이 늘어나면 지우면 됩니다.

### 색상 바꾸기
`assets/css/site.css` 최상단 `:root` 블록의 변수만 고치면 전체에 반영됩니다.
`--bone`(#eae5de)이 로고에서 추출한 크림색입니다.

### 로고 교체
`assets/img/`의 `logo.png`(전체 로고), `mark.png`(심볼만)을 같은 이름으로 덮어쓰면 됩니다.
파비콘까지 바꾸려면 `icon-*.png`와 `favicon.ico`도 함께 교체하세요.

---

## 참고

- 폰트(Anton, Inter)는 저장소에 포함되어 self-host 됩니다. Google Fonts CDN을
  쓰지 않으므로 방문자 IP가 제3자로 전송되지 않습니다 — GDPR 관점에서 안전합니다.
- 다크 전용입니다. `prefers-color-scheme`을 따르지 않고 항상 검은 배경으로 표시됩니다.
