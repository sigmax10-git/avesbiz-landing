# AVES 랜딩페이지 (avesbiz.co.kr)

복합기·프린터 임대, 원격 모니터링, 기업 IT 운영 소개 페이지.
빌드 도구 없는 정적 HTML 한 장이라 `index.html`만 고치면 된다.

AI 플랫폼(AIRS, airental.co.kr)은 별도 저장소 `airental-landing`에 있다.
두 사이트는 서로 바깥 링크로만 연결된다.

## 파일

| 파일 | 설명 |
|---|---|
| `index.html` | 페이지 전체 (HTML + CSS + JS 한 파일) |
| `aves-logo.svg` | 네이비 워드마크 — 헤더용 |
| `aves-logo-white.svg` | 흰색 워드마크 — 푸터용 |
| `mfp-1..9.webp` / `.png` | 복합기 외장 디자인 9종 (WebP 우선, PNG 폴백) |

> **로고는 임시본이다.** 실제 AVES 로고가 나오면 위 SVG 두 개를 같은 이름으로
> 교체하면 된다. `index.html`에서 `width="211" height="55"`로 비율을 지정해
> 두었으니, 교체 로고의 viewBox 비율이 다르면 이 두 값도 같이 고쳐야 한다.

## GitHub Pages 배포

배포 완료. `main` 에 푸시하면 1~2분 뒤 반영된다.

- 저장소: https://github.com/sigmax10-git/avesbiz-landing
- 미리보기: https://sigmax10-git.github.io/avesbiz-landing/

## 교차 링크 (도메인 연결 전까지)

두 사이트는 서로 바깥 링크로 연결된다. 그런데 가비아 DNS 를 아직 붙이지
않았으므로, 지금 `airental.co.kr` / `avesbiz.co.kr` 로 링크를 걸면 방문자가
오류 페이지로 떨어진다. 그래서 **클릭되는 링크는 github.io 미리보기 주소**로
두었고, `canonical` / `og:url` / JSON-LD 는 최종 도메인을 유지한다.

전환은 `C:\RandingPage\.swap-crosslinks.py` 로 한다 (두 저장소를 같이 고친다).

```bash
python C:/RandingPage/.swap-crosslinks.py domain    # 도메인 연결 후
python C:/RandingPage/.swap-crosslinks.py preview    # 되돌리기
```

바뀐 뒤 양쪽 저장소를 각각 커밋·푸시해야 반영된다.

## 도메인 연결 (avesbiz.co.kr)

**미리보기가 끝난 뒤에** 한다. 커스텀 도메인을 걸면 github.io 주소는
커스텀 도메인으로 리다이렉트되므로, DNS가 아직 안 붙은 상태에서 먼저 걸면
사이트에 접근할 수 없게 된다.

1. 가비아 DNS 관리에서 레코드를 추가한다.

   | 타입 | 호스트 | 값 |
   |---|---|---|
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |
   | CNAME | www | sigmax10-git.github.io. |

2. DNS가 퍼진 뒤(보통 10분~수 시간) 저장소 **Settings → Pages →
   Custom domain**에 `avesbiz.co.kr`을 입력하고 저장한다.
   저장하면 저장소에 `CNAME` 파일이 자동으로 생긴다.
3. **Enforce HTTPS**를 켠다. 인증서 발급에 시간이 조금 걸린다.
4. 연결이 끝나면 `.swap-crosslinks.py domain` 을 돌려 교차 링크를 실제
   도메인으로 바꾸고, 두 저장소를 커밋·푸시한다.
5. `airental-landing` 저장소의 `rental.html` 안 `AUTO_REDIRECT`를 `true`로
   바꾼다. 그래야 옛 주소로 들어온 방문자가 자동으로 이 사이트로 넘어간다.

## 아직 연결 안 된 것

- **견적 문의 폼** — 접수 서버가 없어 제출하면 메일 작성 화면이 열린다.
  거짓 접수 완료 메시지를 띄우지 않으려고 일부러 이렇게 두었다.
  실제 접수 API가 생기면 `index.html`의 submit 핸들러에서 `fetch`로 보내고
  `formDone` 안내 문구를 바꾸면 된다.
- **`contact@avesbiz.co.kr`** — 가비아에서 메일 설정이 끝나고 실제로
  수신되는지 확인해야 한다. 페이지 안에 이 주소가 4곳 들어 있다.
- **방문자 카운터** — `VISIT_API`가 비어 있어 표시되지 않는다.
  주소를 넣으면 켜진다. 엔드포인트는 GET 한 번에 방문을 1 올리고
  `{"today": n, "total": n}`을 돌려주며 CORS를 허용해야 한다.
