# HAKUTO BIC — B案 ｜ 웹서버 배포용

B案(Light Cinematic) 정적 사이트입니다. 자산 경로는 **루트 절대경로**(`/assets/...`, `/cmn_logo01.svg`)로 되어 있어, **이 폴더의 내용을 웹 루트(도메인 최상위)에 그대로 올리면** 됩니다.

```
hakuto-bic/
├─ index.html            ← /  (웹 루트)
├─ cmn_logo01.svg        ← /cmn_logo01.svg
├─ download-assets.sh    ← CDN 자산 다운로드 스크립트
├─ vercel.json           ← (Vercel용, 일반 서버는 무시)
└─ assets/               ← /assets/  (스크립트 실행 후 18개 파일)
```

## 1) CDN 자산 다운로드 (최초 1회)
```bash
cd hakuto-bic
bash download-assets.sh
```
→ `assets/` 에 이미지·동영상 18개가 받아집니다.

## 2) 웹서버에 업로드
`index.html`, `cmn_logo01.svg`, `assets/` 폴더(통째로)를 **웹 루트**에 업로드합니다.
- 예) 정적 호스팅 / Nginx·Apache `root` 디렉터리 / S3 버킷 루트 등
- 절대경로(`/assets/`)이므로 **반드시 도메인 루트에서 서빙**되어야 합니다.
  - 서브경로(`example.com/site/`)에 올리려면 경로를 `/site/assets/` 식으로 바꿔야 하니, 그 경우 알려주세요.

### 로컬 확인
```bash
npx serve .         # http://localhost:3000 — 루트 서빙이라 /assets/ 정상 동작
# 또는: python3 -m http.server 8000
```

### (선택) Vercel
```bash
npx vercel --prod
```
`vercel.json` 로 `/assets/*` 에 장기 캐시가 적용됩니다.

## 참고
- 파일명은 원본 생성 파일명을 그대로 사용합니다.
- 자산 교체 시 `assets/` 내 파일을 같은 이름으로 교체하세요.
