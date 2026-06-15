# DXCMS v8.1.0

> PHP 5.6 ~ 8.x / IIS · Apache · Nginx · 저가형 웹호스팅 완전 호환  
> Windows · Linux / MySQL 5.6+ · MariaDB 10.1+

---

## 목차

1. [특징](#특징)
2. [폴더 구조](#폴더-구조)
3. [설치](#설치)
4. [업그레이드](#업그레이드)
5. [웹서버별 설정](#웹서버별-설정)
6. [핵심 클래스](#핵심-클래스)
7. [게시판 시스템](#게시판-시스템)
8. [카테고리 시스템](#카테고리-시스템)
9. [회원 시스템](#회원-시스템)
10. [소셜 로그인](#소셜-로그인)
11. [테마 시스템](#테마-시스템)
12. [플러그인 시스템](#플러그인-시스템)
13. [결제 플러그인](#결제-플러그인)
14. [에디터](#에디터)
15. [캡차](#캡차)
16. [SMS](#sms)
17. [메일](#메일)
18. [SEO 최적화](#seo-최적화)
19. [캐시 · 성능](#캐시--성능)
20. [보안](#보안)
21. [멀티사이트](#멀티사이트)
22. [URL 라우팅](#url-라우팅)
23. [훅 시스템](#훅-시스템)
24. [extend/ — 코드 삽입](#extend----코드-삽입)
25. [게시판 여분 필드](#게시판-여분-필드)
26. [포인트 · 경험치 · 레벨](#포인트--경험치--레벨)
27. [알림 시스템](#알림-시스템)
28. [쪽지 · 친구 · 스크랩](#쪽지--친구--스크랩)
29. [팝업](#팝업)
30. [포인트샵](#포인트샵)
31. [전체 공지](#전체-공지)
32. [썸네일](#썸네일)
33. [마켓](#마켓)
34. [API 키](#api-키)
35. [호환성](#호환성)
36. [라이선스](#라이선스)

---

## 다운로드

https://designonex.com/dxcms-download

---

## 특징

| 항목 | 내용 |
|---|---|
| **언어** | PHP 5.6 ~ 8.x (단일 코드베이스) |
| **DB** | MySQL 5.6+ / MariaDB 10.1+ (PDO) |
| **웹서버** | IIS · Apache · Nginx · 저가형 공유호스팅 |
| **OS** | Windows · Linux |
| **에디터** | CKEditor 4 로컬 내장 (CDN 의존 없음) |
| **캐시** | Redis → APCu → 파일 캐시 자동 선택 |
| **멀티도메인** | 같은 DB에 도메인별 독립 사이트 운영 |
| **결제** | 국내외 PG 11종 플러그인 내장 |
| **보안** | CSRF · XSS · Path Traversal · Rate Limit · WAF |
| **실시간** | WebSocket 소켓 플러그인 (dx-socket) |

---

## 폴더 구조

```
dxcms/
│
├── index.php                        ← 단일 진입점 (모든 요청)
├── dx_load.php                      ← 독립 부트스트랩 (플러그인 개발용)
│
├── core/                            ← CMS 엔진 (수정 비권장)
│   ├── Secure.php                   ← 보안 전담 (CSRF · XSS · 세션 · 업로드)
│   ├── functions.php                ← 공통 함수
│   ├── DxCache.php                  ← Redis · APCu · 파일 캐시
│   ├── DxRouter.php                 ← 라라벨 스타일 라우터
│   ├── DxSite.php                   ← 멀티사이트
│   ├── DxTheme.php                  ← 테마 엔진
│   ├── DxSeo.php                    ← SEO (OG · Twitter Card · JSON-LD)
│   ├── DxSanitizer.php              ← 서버사이드 HTML 정제
│   ├── DxCategory.php               ← 카테고리 렌더링
│   ├── DxPoint.php                  ← 포인트 · 경험치 · 레벨
│   ├── DxNotification.php           ← 실시간 알림
│   ├── DxFriend.php                 ← 스크랩 · 친구 · 차단
│   ├── DxShop.php                   ← 포인트샵
│   ├── DxPopup.php                  ← 팝업 엔진
│   ├── DxMailer.php                 ← 메일 발송
│   ├── DxSms.php                    ← SMS 발송
│   ├── DxCaptcha.php                ← 캡차 (빌트인 · reCAPTCHA · hCaptcha · Turnstile)
│   ├── DxSocialAuth.php             ← 소셜 로그인
│   ├── DxThumb.php                  ← 썸네일 생성 (GD)
│   ├── DxBoardSkin.php              ← 게시판 스킨 폴백 체인
│   ├── DxMarket.php                 ← 마켓 클라이언트
│   ├── DxMigration.php              ← 마이그레이션 감지
│   ├── DxMemberMonitor.php          ← 회원 접속 모니터링
│   ├── DxContainer.php              ← 경량 서비스 컨테이너
│   ├── DxCss.php                    ← dxb-css 서버사이드 엔진
│   ├── DxExtend.php                 ← extend/ 폴더 자동 실행
│   ├── BoardFields.php              ← 게시판 여분 필드
│   ├── PluginRegistry.php           ← 플러그인 등록 · 활성화
│   ├── hook/HookManager.php         ← 훅 시스템
│   ├── db/Database.php              ← PDO 래퍼 (싱글턴)
│   ├── db/QueryBuilder.php          ← 쿼리 빌더
│   ├── router/Router.php            ← URL 파싱 · 라우트 결정
│   ├── router/Dispatcher.php        ← 라우트 → 핸들러 실행
│   ├── auth/Auth.php                ← 세션 기반 인증
│   ├── auth/mypage/                 ← 마이페이지 탭별 렌더러
│   ├── captcha/                     ← 캡차 드라이버 (빌트인·reCAPTCHA V2/V3·hCaptcha·Turnstile)
│   ├── mailer/                      ← 메일 드라이버 (SMTP · Sendmail · PHP mail())
│   ├── sms/                         ← SMS 드라이버 (CoolSMS · KT비즈메카 · NCP · Twilio)
│   ├── search/handler.php           ← 통합 검색
│   └── api/                         ← API 핸들러
│
├── admin/                           ← 관리자
│   ├── index.php                    ← 관리자 레이아웃
│   ├── dashboard/                   ← 대시보드
│   ├── boards/                      ← 게시판 관리
│   ├── board_groups/                ← 게시판 그룹
│   ├── categories/                  ← 카테고리
│   ├── members/                     ← 회원 관리
│   ├── menus/                       ← 메뉴 관리
│   ├── pages/                       ← 페이지 관리
│   ├── theme/                       ← 테마 관리
│   ├── plugins/                     ← 플러그인 관리
│   ├── popup/                       ← 팝업 관리
│   ├── global_notices/              ← 전체 공지
│   ├── sites/                       ← 멀티사이트 관리
│   ├── statistics/                  ← 방문자 통계 · 인기검색어
│   ├── ranking/                     ← 회원 랭킹
│   ├── levels/                      ← 레벨 설정
│   ├── points/                      ← 포인트 관리
│   ├── shop/                        ← 포인트샵 관리
│   ├── market/                      ← 마켓 관리
│   ├── social/                      ← 소셜 로그인 설정
│   ├── sendmail/                    ← 메일 발송
│   ├── sendsms/                     ← SMS 발송
│   ├── socket/                      ← 소켓 모니터
│   ├── downloads/                   ← 다운로드 로그
│   ├── popular/                     ← 인기 게시물
│   └── settings/                    ← 사이트 설정
│
├── boards/                          ← 게시판
│   ├── handler.php                  ← 목록/보기/쓰기/수정/삭제/댓글
│   ├── skins/gallery/               ← 갤러리 스킨
│   └── category/skins/              ← 카테고리 탭 스킨 (default · pill)
│
├── themes/                          ← 프론트엔드 테마
│   └── default/                     ← 기본 테마
│       ├── theme.json               ← 테마 메타정보
│       ├── layout/main.php          ← 전체 레이아웃
│       ├── board/basic/             ← 게시판 스킨
│       ├── board_latest/            ← 최신글 위젯 스킨
│       ├── auth/mypage/             ← 마이페이지 테마 오버라이드
│       ├── page/                    ← 403 · 404 페이지
│       ├── parts/pagination.php     ← 페이지네이션
│       └── search/list.php          ← 검색 결과
│
├── plugins/                         ← 플러그인
│   ├── ckeditor4-editor/            ← CKEditor 4
│   ├── dx-socket/                   ← 실시간 WebSocket
│   ├── tosspay-payment/             ← 토스페이먼츠
│   ├── kakaopay-payment/            ← 카카오페이
│   ├── naverpay-payment/            ← 네이버페이
│   ├── nicepay-payment/             ← 나이스페이먼츠
│   ├── kg-inicis-payment/           ← KG이니시스
│   ├── kcp-payment/                 ← NHN KCP
│   ├── danal-payment/               ← 다날 DPAY
│   ├── payletter-payment/           ← 페이레터
│   ├── paypal-payment/              ← PayPal
│   ├── stripe-payment/              ← Stripe
│   ├── example-plugin/              ← 플러그인 개발 예제
│   └── custom-payment-template/     ← 결제 모듈 개발 템플릿
│
├── assets/
│   ├── ckeditor4/                   ← CKEditor 4 로컬 파일
│   └── js/
│       ├── dxb-css.js               ← dxb-css 런타임 (Tailwind 호환)
│       └── dx.js                    ← 공통 JS
│
├── extend/                          ← 훅 없이 코드 삽입
│   ├── top/                         ← 초기화 직후 실행
│   ├── middle/                      ← 라우팅 후 실행
│   └── bottom/                      ← 렌더링 완료 후 실행
│
├── routes/                          ← DxRouter 라우트 정의 파일
│
├── controllers/                     ← 컨트롤러 (DxRouter용)
│
├── pages/                           ← 커스텀 페이지
│   ├── home.php                     ← 홈 페이지
│   └── shop.php                     ← 쇼핑 페이지
│
├── data/                            ← 설정 · 업로드 · 캐시 (웹 접근 차단)
│   ├── config.php                   ← DB 연결 + 설정 (설치 시 자동 생성)
│   ├── cache/                       ← 파일 캐시
│   └── uploads/                     ← 업로드 파일
│
├── install/                         ← 설치 마법사 (설치 후 삭제 권장)
│   ├── index.php                    ← 설치 UI
│   ├── migrate.php                  ← 마이그레이션 실행기
│   └── schema.sql                   ← DB 스키마
│
├── docs/
│   ├── CAPTCHA_MANUAL.md            ← 캡차 설정 가이드
│   └── MARKET_API_SPEC.md           ← 마켓 API 명세
│
├── .htaccess                        ← Apache 설정
├── web.config                       ← IIS 설정 (URL Rewrite 있는 환경)
├── web.config.no-rewrite            ← IIS 설정 (URL Rewrite 없는 환경)
└── nginx.conf.example               ← Nginx 설정 예시
```

---

## 설치

### 1. 파일 업로드
웹 루트 또는 서브디렉토리에 전체 파일을 업로드합니다.

### 2. 설치 마법사 접속
```
https://yourdomain.com/install/
```
DB 정보를 입력하면 `data/config.php`가 자동 생성됩니다.

### 3. 설치 후 정리
```
install/ 폴더 삭제 (보안)
```

### 권장 서버 설정
```ini
; php.ini
opcache.enable              = 1
opcache.memory_consumption  = 256
opcache.max_accelerated_files = 10000

; PHP-FPM
pm.max_children = 200
```

---

## 업그레이드

버전 업그레이드 시 `install/migrate.php`를 브라우저에서 실행합니다.

```
https://yourdomain.com/install/migrate.php
```

실행 완료 후 `install/` 폴더를 삭제하세요.

---

## 웹서버별 설정

| 환경 | 사용 파일 |
|---|---|
| Apache (mod_rewrite 있음) | `.htaccess` |
| Apache (mod_rewrite 없음) | `.htaccess` — `?_url=` 방식 자동 전환 |
| IIS (URL Rewrite 있음) | `web.config` |
| IIS (URL Rewrite 없음) | `web.config.no-rewrite` → `web.config`로 교체 |
| Nginx | `nginx.conf.example` 참고 |
| 저가형 공유호스팅 | 별도 설정 불필요 |

---

## 핵심 클래스

| 클래스 | 역할 |
|---|---|
| `Secure` | 보안 전담 — CSRF · 세션 · WAF · Rate Limit · 업로드 검증 |
| `Database` | PDO 래퍼 싱글턴 |
| `QueryBuilder` | 체이닝 쿼리 빌더 |
| `Router` | URL 파싱 · 라우트 결정 |
| `Dispatcher` | 라우트 → 핸들러 실행 |
| `DxRouter` | 라라벨 스타일 라우터 (routes/ 파일 기반) |
| `DxContainer` | 경량 서비스 컨테이너 |
| `DxSite` | 멀티사이트 도메인별 설정 분리 |
| `DxCache` | Redis → APCu → 파일 캐시 자동 선택 |
| `DxTheme` | 테마 · 스킨 해석 및 폴백 체인 |
| `DxSeo` | OG · Twitter Card · JSON-LD · 사이트맵 |
| `DxSanitizer` | 서버사이드 XSS 차단 (DOMDocument 기반) |
| `DxCategory` | 카테고리 렌더링 |
| `DxPoint` | 포인트 · 경험치 · 레벨 엔진 |
| `DxNotification` | 실시간 알림 (댓글 · 친구 · 스크랩 · 쪽지) |
| `DxFriend` | 스크랩 · 친구 · 차단 |
| `DxShop` | 포인트샵 |
| `DxPopup` | 팝업 엔진 |
| `DxMailer` | 메일 발송 (SMTP · Sendmail · mail()) |
| `DxSms` | SMS 발송 (CoolSMS · KT비즈메카 · NCP · Twilio) |
| `DxCaptcha` | 캡차 (빌트인 · reCAPTCHA V2/V3 · hCaptcha · Turnstile) |
| `DxSocialAuth` | 소셜 로그인 (카카오 · 네이버 · 구글 · GitHub) |
| `DxThumb` | GD 기반 썸네일 생성 |
| `DxMarket` | 플러그인 마켓 클라이언트 |
| `DxMigration` | 미실행 마이그레이션 감지 |
| `DxMemberMonitor` | 회원 접속 모니터링 |
| `DxBoardSkin` | 게시판 스킨 폴백 체인 |
| `BoardFields` | 게시판 여분 필드 |
| `HookManager` | 훅 등록 · 실행 |
| `PluginRegistry` | 플러그인 등록 · 활성화 관리 |
| `DxExtend` | extend/ 폴더 자동 실행 |
| `DxCss` | dxb-css 서버사이드 CSS 생성 |

---

## 게시판 시스템

### 지원 기능
- 목록 · 보기 · 쓰기 · 수정 · 삭제
- 공지사항 · 비밀글 · 카테고리 · 태그
- 댓글 · 대댓글
- 파일 첨부 (드래그&드롭 · 멀티 업로드)
- 좋아요 · 스크랩
- 검색 (제목 · 내용 · 작성자 · 분류)
- 게시판 그룹
- 게시판별 여분 필드

### 게시판 스킨 우선순위
```
themes/[테마]/board/[스킨]/    ← 테마별 커스텀 (최우선)
boards/skins/[스킨]/           ← 전역 스킨
boards/skins/gallery/          ← 기본 갤러리 스킨 (폴백)
```

### 파일 업로드 보안
- MIME 타입 + 확장자 이중 검증
- 실제 이미지 검증 (`getimagesize()`)
- 이중 확장자 공격 차단
- 업로드 디렉토리 PHP 실행 차단 `.htaccess` 자동 생성

---

## 카테고리 시스템

- DB 기반 무한 깊이 계층 구조
- 게시판별 독립 카테고리
- 표시 위치: 리스트 탭 / 뷰 배지 독립 on/off
- 배지 색상 선택

### 카테고리 스킨 우선순위
```
themes/[테마]/category/        ← 테마별 커스텀 (최우선)
boards/category/skins/[스킨]/  ← 지정 스킨
boards/category/skins/default/ ← 기본 스킨 (폴백)
```

---

## 회원 시스템

- 회원가입 · 로그인 · 로그아웃
- 이메일 인증
- 비밀번호 bcrypt 해시 (PHP 5.6 폴백 포함)
- 프로필 이미지 (GD 리사이즈)
- 마이페이지: 프로필 · 포인트 · 경험치 · 알림 · 쪽지 · 친구 · 스크랩 · 소셜 · 구매내역 · 차단

---

## 소셜 로그인

| 서비스 | 구현 |
|---|---|
| 카카오 | REST API |
| 네이버 | OAuth2 |
| 구글 | OAuth2 |
| GitHub | OAuth2 |

관리자 > 소셜 설정에서 각 서비스의 API 키를 입력합니다.

---

## 테마 시스템

### 테마 구조
```
themes/[테마명]/
├── theme.json          ← 이름 · 버전 · 작성자
├── layout/main.php     ← 전체 레이아웃 (헤더+본문+푸터)
├── board/[스킨]/       ← 게시판 스킨
├── board_latest/       ← 최신글 위젯
├── auth/mypage/        ← 마이페이지 오버라이드
├── page/               ← 403 · 404
├── parts/              ← 공통 파트 (페이지네이션 등)
└── search/             ← 검색 결과
```

파일이 없으면 `themes/default/`로 자동 폴백됩니다.

### 최신글 위젯 스킨
`themes/[테마]/board_latest/` 아래에 `list.php`, `card.php`, `simple.php`, `gallery/grid.php`, `gallery/slide.php` 배치.

---

## 플러그인 시스템

### 플러그인 등록
```php
// plugins/my-plugin/plugin.php
dx_register_plugin(array(
    'id'      => 'my-plugin',
    'name'    => 'My Plugin',
    'version' => '1.0.0',
    'type'    => 'editor',   // editor | payment | socket | captcha | 커스텀
));
```

### manifest.php (마켓 공유용)
```php
// plugins/my-plugin/manifest.php
return array(
    'name'        => 'My Plugin',
    'version'     => '1.0.0',
    'description' => '설명',
    'author'      => 'DesignOneX',
    'type'        => 'editor',
    'requires'    => '5.6.0',
);
```

### 서비스 컨테이너 활용
```php
// 바인딩
dx_app()->bind('mailer', function() {
    return new MyMailer(dx_config('smtp_host'));
});

// 사용
$mailer = dx_app()->make('mailer');
```

---

## 결제 플러그인

| 플러그인 | PG사 | 지원 수단 |
|---|---|---|
| `tosspay-payment` | 토스페이먼츠 | 카드 · 계좌이체 · 가상계좌 · 휴대폰 |
| `kakaopay-payment` | 카카오페이 | 단건 · 정기결제 |
| `naverpay-payment` | 네이버페이 | 주문형 간편결제 |
| `nicepay-payment` | 나이스페이먼츠 | 카드 · 가상계좌 · 계좌이체 · 휴대폰 · 간편결제 |
| `kg-inicis-payment` | KG이니시스 | 카드 · 계좌이체 · 가상계좌 · 휴대폰 · 상품권 |
| `kcp-payment` | NHN KCP | 카드 · 계좌이체 · 가상계좌 · 휴대폰 · 간편결제 |
| `danal-payment` | 다날 DPAY | 카드 · 계좌이체 · 휴대폰 |
| `payletter-payment` | 페이레터 | 카드 · 가상계좌 · 계좌이체 · 휴대폰 |
| `paypal-payment` | PayPal | PayPal JS SDK v2 |
| `stripe-payment` | Stripe | Stripe.js v3 + Payment Intents API |
| `custom-payment-template` | — | 신규 결제 모듈 개발 템플릿 |

---

## 에디터

| 플러그인 | 비고 |
|---|---|
| `ckeditor4-editor` | 기본 활성, 로컬 파일 (CDN 의존 없음) |

관리자 > 게시판 관리에서 게시판마다 에디터를 선택할 수 있습니다.

---

## 캡차

| 드라이버 | 설명 |
|---|---|
| `DxBuiltinDriver` | 서버 내장 이미지 캡차 (외부 의존 없음) |
| `DxRecaptchaV2Driver` | Google reCAPTCHA v2 |
| `DxRecaptchaV3Driver` | Google reCAPTCHA v3 |
| `DxHcaptchaDriver` | hCaptcha |
| `DxTurnstileDriver` | Cloudflare Turnstile |

커스텀 드라이버를 `DxCaptchaDriverInterface`를 구현해서 등록할 수 있습니다.  
자세한 내용은 `docs/CAPTCHA_MANUAL.md`를 참고하세요.

---

## SMS

| 드라이버 | 서비스 |
|---|---|
| `DxCoolSmsDriver` | CoolSMS |
| `DxKtBizmekaDriver` | KT 비즈메카 |
| `DxNcpSmsDriver` | Naver Cloud Platform SMS |
| `DxTwilioDriver` | Twilio |

```php
// 단건 발송
DxSms::send('01012345678', '인증번호: 123456');
```

---

## 메일

| 드라이버 | 설명 |
|---|---|
| `DxSmtpDriver` | SMTP (TLS/SSL) |
| `DxSendmailDriver` | Sendmail |
| `DxMailFuncDriver` | PHP `mail()` 함수 |

```php
DxMailer::send('to@example.com', '제목', '<p>내용</p>');
```

---

## SEO 최적화

### 자동 생성 태그
```html
<title>게시글 제목 — 게시판명 | 사이트명</title>
<meta name="description" content="본문 앞 150자 자동 추출">
<meta name="robots" content="index,follow">
<link rel="canonical" href="https://...">
<meta property="og:type" content="article">
<meta property="og:image" content="본문 첫 이미지 자동 추출">
<meta name="twitter:card" content="summary_large_image">
<script type="application/ld+json">{"@type":"Article",...}</script>
```

### 자동 noindex
- 비밀글
- 검색 결과 페이지 (`?s=` 파라미터)

### 사이트맵
```
/sitemap.xml           ← 사이트맵 인덱스
/sitemap-pages.xml     ← 홈 + 정적 페이지
/sitemap-boards.xml    ← 게시판 목록
/sitemap-posts-1.xml   ← 게시글 (1000개씩 자동 분할)
```

멀티도메인 환경에서 각 도메인이 독립 사이트맵을 생성합니다.

---

## 캐시 · 성능

### DxCache 드라이버 우선순위
```
1. Redis    — REDIS_SESSION_URL 상수 + Redis 익스텐션
2. APCu     — apc.enabled + apcu_fetch 존재
3. 파일     — data/cache/ 쓰기 가능 (저가형 호스팅 기본)
4. none     — 위 모두 실패 (캐시 없이 동작)
```

### 캐시 항목
| 항목 | TTL |
|---|---|
| 전체 설정 (dx_settings) | 5분 |
| 게시판 목록 | 1분 |
| 사이트맵 XML | 10분 |

### 세션 최적화
비로그인 GET 요청은 세션을 시작하지 않아 파일 lock이 제거됩니다.

---

## 보안

보안 코드는 **`core/Secure.php` 한 파일**에 집중되어 있습니다.

### 담당 기능
- 세션 보안 (HttpOnly · Secure · SameSite=Lax)
- CSRF 토큰 발급 / 검증 (TTL 3시간, 글쓰기 중 자동 갱신)
- WAF (SQL Injection · XSS · Path Traversal 패턴 차단)
- Rate Limit
- 보안 헤더 (X-Frame-Options · X-Content-Type-Options · Referrer-Policy)
- 업로드 파일 검증 (MIME · 이중 확장자 차단)
- 비밀번호 bcrypt 해시
- 안전 난수 생성 (PHP 5.6 ~ 8.x)
- Redis 세션 저장소 지원

---

## 멀티사이트

`dx_sites` 테이블에 도메인을 등록하면 도메인별 독립 설정이 적용됩니다.

```
domain              사이트명    테마      메뉴그룹
example.com         메인사이트  default   main
shop.example.com    쇼핑몰      default   shop
blog.example.com    블로그      default   blog
```

### 도메인별 독립 적용 항목
- 사이트명 · 설명 · URL · 테마 · 언어 · 시간대 · 메뉴 그룹
- SEO 사이트맵 (각 도메인 URL 기준으로 독립 생성)

---

## URL 라우팅

| URL 패턴 | 처리 |
|---|---|
| `/` | 홈 |
| `/free` | 게시판 목록 (board_key=free) |
| `/free/view/123` | 게시글 보기 |
| `/free/write` | 게시글 작성 |
| `/free/edit/123` | 게시글 수정 |
| `/about` | 커스텀 페이지 (slug=about) |
| `/auth/login` | 로그인 |
| `/auth/register` | 회원가입 |
| `/admin` | 관리자 대시보드 |
| `/api/upload` | 파일 업로드 API |
| `/sitemap.xml` | 동적 사이트맵 인덱스 |
| `/robots.txt` | 동적 robots.txt |

**URL Rewrite 없는 환경**: `index.php?_url=/free/view/123`

### DxRouter (라라벨 스타일)
`routes/web.php` 등 파일을 생성하고 라우트를 정의합니다.

```php
DxRouter::get('/mypage/dashboard', 'MemberController@dashboard')
        ->middleware('auth');

DxRouter::post('/api/update', 'MemberController@update')
        ->middleware(array('auth', 'csrf'));

DxRouter::group(array('prefix' => '/shop', 'middleware' => 'auth'), function() {
    DxRouter::get('/cart', 'ShopController@cart');
});
```

컨트롤러는 `controllers/` 폴더에 저장합니다.

### 기본 미들웨어
| 미들웨어 | 동작 |
|---|---|
| `auth` | 로그인 필수 |
| `admin` | 관리자 필수 |
| `guest` | 비로그인만 접근 |
| `csrf` | CSRF 토큰 검증 |
| `json` | JSON 응답 헤더 자동 설정 |

---

## 훅 시스템

### 훅 등록
```php
dx_add_hook('dx_head', function() {
    echo '<link rel="stylesheet" href="/my.css">';
}, 10); // 우선순위 (낮을수록 먼저 실행)
```

### 주요 훅 포인트
| 훅 | 위치 |
|---|---|
| `dx_head` | `<head>` 내부 |
| `dx_body_bottom` | `</body>` 직전 |
| `dx_board_list_context` | 게시판 목록 컨텍스트 생성 후 |
| `dx_board_view_context` | 게시글 보기 컨텍스트 생성 후 |
| `dx_board_after_save` | 게시글 저장 후 |
| `dx_admin_top` | 관리자 본문 상단 |
| `dx_editor_render` | 에디터 렌더링 |

---

## extend/ — 코드 삽입

훅 등록 없이 파일만 폴더에 넣으면 파일명 오름차순으로 자동 실행됩니다.

```
extend/top/01_darkmode_early.php      ← 다크모드 초기 설정
extend/top/05_activity_monitor.php    ← 회원 활동 모니터링
extend/top/50_board_fields.php        ← 게시판 여분 필드 활성화
extend/top/90_market_guard.php        ← 마켓 라이선스 검증
extend/middle/01_visit_tracker.php    ← 방문자 추적
extend/bottom/01_session_rescue_gc.php ← 세션 복구 GC
extend/bottom/02_darkmode_engine.php  ← 다크모드 엔진
```

에러가 발생해도 다른 파일에 영향을 주지 않습니다.

---

## 게시판 여분 필드

게시판마다 관리자가 정의한 추가 입력 필드를 붙일 수 있습니다.  
`extend/top/50_board_fields.php`로 활성화됩니다.

```php
// 글쓰기 폼에 출력
echo dx_board_fields()->renderWriteForm($board['id'], $savedMeta);

// 뷰 페이지에 출력
echo dx_board_fields()->renderView($board['id'], $post['id']);

// 단일 값 읽기
$value = dx_board_fields()->getValue($post['id'], 'field_key');
```

---

## 포인트 · 경험치 · 레벨

- 로그인 · 글쓰기 · 댓글 · 좋아요 등 활동별 포인트 자동 지급
- 경험치 누적 → 레벨업 자동 처리
- 레벨 설정은 관리자 > 레벨 관리에서 커스터마이징
- `dx_point_log`, `dx_exp_log`, `dx_level_config` 테이블 사용

---

## 알림 시스템

| 알림 타입 | 발생 조건 |
|---|---|
| `comment` | 내 게시글에 댓글 |
| `comment_reply` | 내 댓글에 답글 |
| `friend` | 친구 추가 요청 |
| `scrap` | 내 게시글 스크랩 |
| `memo` | 쪽지 수신 |

DB `dx_notifications` 저장, 소켓 플러그인 연동 시 실시간 푸시.

---

## 쪽지 · 친구 · 스크랩

- **쪽지**: 회원 간 1:1 메시지 (`dx_memos`, `dx_dm_messages`)
- **친구**: 친구 추가 · 수락 · 차단 (`dx_friends`)
- **스크랩**: 게시글 북마크 (`dx_scraps`)

---

## 팝업

- 관리자 > 팝업 관리에서 등록
- 노출 기간 · 기기 조건 · 도메인 조건 설정
- "오늘 하루 보지 않기" 쿠키 처리 자동 포함

---

## 포인트샵

- 관리자 > 포인트샵에서 상품 등록
- 회원이 포인트로 상품 구매
- `dx_shop_items`, `dx_shop_purchases` 테이블 사용

---

## 전체 공지

- 관리자 > 전체 공지에서 사이트 전체에 띠 공지 표시
- `dx_global_notices` 테이블 사용

---

## 썸네일

GD 라이브러리 기반 썸네일 자동 생성.

| 유형 | 설명 |
|---|---|
| `fit` | 비율 유지, 지정 사이즈 안에 맞춤 |
| `crop` | 지정 사이즈로 중앙 크롭 |

GD가 없는 환경에서는 원본 이미지를 그대로 사용합니다.

---

## 마켓

플러그인 · 테마를 중앙 마켓 서버(`designonex.com`)에서 검색 · 설치할 수 있습니다.  
자세한 API 명세는 `docs/MARKET_API_SPEC.md`를 참고하세요.

---

## API 키

외부 연동을 위한 REST API 키를 발급하고 요청 로그를 관리합니다.  
`dx_api_keys`, `dx_api_logs` 테이블 사용.

---

## 호환성

| 항목 | 범위 |
|---|---|
| PHP | 5.6 · 7.0 · 7.1 · 7.2 · 7.3 · 7.4 · 8.0 · 8.1 · 8.2 · 8.3 · 8.4 |
| MySQL | 5.6 이상 |
| MariaDB | 10.1 이상 |
| 웹서버 | IIS 7+ · Apache 2.2+ · Nginx · 저가형 공유호스팅 |
| OS | Windows · Linux |
| 브라우저 | Chrome · Firefox · Safari · Edge |

---

## 라이선스

Copyright (C) 2026 designonex [디자인원엑스]  
Licensed under the GNU Lesser General Public License v3.0 (LGPL-3.0)  
https://designonex.com/dxcms-manual/view/1776622530771477

전체 라이선스 본문은 `LICENSE` 파일을 참고하세요.

---

*DesignOneX CMS — 최고의 PHP CMS를 목표로 합니다.*
