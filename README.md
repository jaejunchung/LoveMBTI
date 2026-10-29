# 애인의 MBTI 이상형

MBTI가 "나는 어떤 사람인가"를 묻는다면, 이 테스트는 "나는 어떤 사람을 견딜 수 있는가"를 묻습니다.
스무 개의 연애 밸런스 게임에서 **더 화나는 쪽**을 고르면, 내가 바라는 애인의 네 글자가 나옵니다.

**데모:** https://jaejunchung.github.io/LoveMBTI/

---

## 구성

```
LoveMBTI/
├── index.html                  단일 파일 웹 앱 (HTML + CSS + JS 전부 포함)
├── assets/og-image.png         카카오톡·트위터 공유 미리보기 이미지 (1200×630)
├── docs/questions.md           20문항 원문과 채점표
├── .github/workflows/deploy.yml  GitHub Actions 자동 배포
├── .nojekyll                   Jekyll 빌드 비활성화
├── LICENSE
└── README.md
```

빌드 도구나 패키지 의존성이 없습니다. 외부에서 불러오는 것은 Google Fonts(Gowun Batang, IBM Plex Sans KR) 하나뿐이고, 나머지 CSS·JS·데이터는 `index.html` 안에 전부 들어 있습니다. 파일을 더블클릭해도 그대로 동작합니다.

## 배포

### 방법 1 — 브랜치에서 바로 배포 (가장 간단)

1. 저장소 **Settings → Pages**
2. **Source**를 `Deploy from a branch`로 두고, 브랜치 `main` / 폴더 `/ (root)` 선택
3. 저장하면 1~2분 뒤 `https://jaejunchung.github.io/LoveMBTI/` 가 열립니다

이 방식을 쓸 경우 `.github/workflows/deploy.yml`은 필요 없으니 삭제해도 됩니다.

### 방법 2 — GitHub Actions로 배포

1. 저장소 **Settings → Pages → Source**를 `GitHub Actions`로 변경
2. `main`에 푸시할 때마다 `deploy.yml`이 자동으로 배포합니다

### 방법 3 — Netlify / Vercel

저장소를 연결하거나 폴더를 드래그 앤 드롭합니다. 빌드 명령은 비우고, 배포 디렉터리는 루트(`.`)로 지정하면 됩니다.

### 업로드 명령

```bash
git clone https://github.com/jaejunchung/LoveMBTI.git
cd LoveMBTI
# 받은 파일들을 이 폴더에 복사한 뒤
git add .
git commit -m "애인의 MBTI 이상형 테스트 페이지 추가"
git push origin main
```

## 채점 방식

핵심은 **선택의 방향**입니다. "더 화나는 쪽"을 고른다는 것은 그 유형을 싫어한다는 뜻이므로, 고른 선택지의 **반대 성향**에 1점이 갑니다. A가 N형 애인을 묘사한다면, A를 고를 때 점수는 S로 갑니다.

네 축에 정확히 5문항씩 배분해서 동점이 구조적으로 나올 수 없습니다.

| 축 | 문항 번호 | 주제 |
|---|---|---|
| E / I | 3, 7, 10, 16, 19 | 연락, SNS, 친구, 취미, 가족 소개 |
| N / S | 1, 2, 12, 14, 17 | 선물, 편지, 관찰, 미래, 식사 |
| T / F | 8, 9, 11, 13, 15 | 과거, 비용, 질투, 간병, 사과 |
| J / P | 4, 5, 6, 18, 20 | 기념일, 데이트, 다툼, 여행, 사생활 |

결과 화면에는 네 글자, 유형 설명, 축별 점수 막대, 조심할 점, 네 글자를 모두 뒤집어 계산한 "가장 못 견디는 애인", 그리고 내가 고른 20개의 기록이 표시됩니다.

## 문항·문구 수정하기

`index.html` 안 `<script>` 블록 상단의 `Q` 배열이 설문 전체입니다.

```js
{tag:"선물", axis:"NS", title:"애인이 100일 기념으로 준 선물",
 a:{t:"밤새 종이로 접은 목걸이를 ...", s:"S"},
 b:{t:"당근에서 산 걸 숨기고 ...",   s:"N"}}
```

- `tag` — 문항 위에 뜨는 주제 꼬리표
- `axis` — 이 문항이 속한 축
- `t` — 선택지 문구
- `s` — **그 선택지를 골랐을 때 올라가는 점수**(= 반대 성향)

문항을 추가하거나 뺄 때는 축마다 홀수 개를 유지해야 동점이 나오지 않습니다.
16개 유형의 설명은 같은 스크립트의 `TYPES` 객체에 있습니다.

색과 글꼴은 `<style>` 최상단 `:root`의 토큰만 바꾸면 전체에 반영됩니다. 다크 모드 값은 `@media (prefers-color-scheme: dark)`와 `:root[data-theme="dark"]` 두 블록에 같이 들어 있으니 함께 수정하세요.

## 참고

재미로 만든 설문입니다. 네 글자는 상대를 평가하는 기준이 아니라, 내가 무엇을 못 견디는지 알아보는 참고용입니다.
