# 퍼스트커피랩 인스타그램 자동 게시 (GitHub Actions / 클라우드)

내 PC가 꺼져 있어도 **GitHub 서버**가 정해진 시각에 자동으로 인스타그램에 게시합니다.
PC·인터넷·클로드 아무것도 켜둘 필요가 없습니다.

## 동작 방식
- 매일 **오전 11:00 / 오후 17:00 (한국시간)** 에 GitHub Actions가 자동 실행
- `content_queue.json` 에서 그 날짜·슬롯의 미게시 콘텐츠를 하나 꺼내 게시
- 게시 후 `posted: true` 로 표시하고 저장소에 자동 커밋 (중복 게시 방지)
- 게시할 콘텐츠가 없으면 조용히 건너뜀

## 폴더 구조
```
.
├─ .github/workflows/autopost.yml   ← 자동 실행 스케줄(cron)
├─ instagram_post.py                ← 게시 스크립트
├─ content_queue.json               ← 게시할 콘텐츠 목록
├─ images/                          ← 게시할 사진들 (여기에 업로드)
└─ videos/                          ← 릴스용 영상 (선택)
```

## 최초 1회 세팅 (사람이 직접)
1. GitHub에 로그인 → **New repository** 로 저장소 생성 (Private 권장)
2. 이 폴더의 파일들을 저장소에 업로드 (이미지 포함)
3. 저장소 **Settings → Secrets and variables → Actions → New repository secret**
   - Name: `IG_ACCESS_TOKEN` / Value: (인스타 액세스 토큰) ← **본인만 입력, 코드엔 안 넣음**
   - (선택) Name: `IG_USER_ID` / Value: `17841445460419105`
4. **Actions** 탭 → 워크플로우 활성화

## 테스트 (즉시 게시)
- 저장소 **Actions → "Instagram 자동 게시" → Run workflow** → slot(am/pm) 선택 → 실행
- 예약 시간을 기다리지 않고 바로 1건 게시해볼 수 있습니다.

## 콘텐츠 추가·수정
- 사진은 `images/` 에 넣고, `content_queue.json` 에 항목(날짜/슬롯/이미지경로/캡션)을 추가
- 인스타 API는 **JPEG 이미지**만 허용 (png는 jpg로 변환 필요)
- 영상은 `media_type: "REELS"`, `image_path: "videos/파일.mp4"` 로 지정

## 참고
- 토큰 수명: 약 60일. 만료 전 재발급해서 Secret 값만 교체하면 됩니다.
- GitHub Actions 예약 실행은 트래픽에 따라 몇 분 늦을 수 있습니다(정상).
