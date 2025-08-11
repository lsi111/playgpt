# Asset Goal Tracker (Android – Expo React Native)

개인 재정 목표를 세우고, 투자/기여금과 감정을 함께 기록·분석하는 **오프라인 우선** 모바일 앱. 웹 버전 로직을 바탕으로 **React Native(Expo) + TypeScript**로 구현하며, 최종적으로 **Google Play 스토어** 배포를 목표로 합니다.

---

## 🧭 목표

* 목표 관리(목표 금액·마감일·초기 투자)
* 투자 기록(금액·메모·감정 이모지)
* 진행률 분석(이상적 vs 실제), 필요 수익률(복리/단리 × 연·월·주·일) 계산
* 승률/손익비 기반 매매 시뮬레이션
* 전체 분석(감정별/요일별 패턴, 요약 통계)
* 오프라인 저장(AsyncStorage), 다크/라이트 모드

---

## 🧩 기술 스택

* **Expo(React Native) + TypeScript**
* 상태관리: **Zustand**
* 저장소: **AsyncStorage** (웹의 localStorage 대체)
* 네비게이션: **React Navigation** (Stack + Bottom Tabs)
* 차트: **react-native-svg** (+ 경량 커스텀 Path)
* 애니메이션: **react-native-reanimated** 또는 **moti**
* 빌드/배포: **EAS** (Android AAB)

---

## 📁 디렉터리 구조(초안)

```
agt-mobile/
├─ app.json
├─ package.json
├─ src/
│  ├─ screens/
│  │  ├─ HomeScreen.tsx
│  │  ├─ GoalDetailScreen.tsx   # 탭: 분석 / 투자 일지
│  │  └─ AnalysisScreen.tsx     # 전체 분석
│  ├─ components/
│  │  ├─ MiniLineChart.tsx      # 이상적 vs 실제 꺾은선
│  │  ├─ WeekdayBars.tsx
│  │  ├─ Segmented.tsx
│  │  ├─ StatCard.tsx
│  │  └─ UI.tsx                 # Button/Input/Icon 등
│  ├─ modals/
│  │  ├─ GoalModal.tsx          # 목표 추가/수정
│  │  └─ InvestmentModal.tsx    # 기여금 추가
│  ├─ store/
│  │  ├─ useGoals.ts            # Zustand + AsyncStorage 동기화
│  │  └─ useTheme.ts
│  ├─ lib/
│  │  ├─ storage.ts             # AsyncStorage 래퍼
│  │  ├─ calc.ts                # 필요 수익률/시뮬레이션/보조함수
│  │  └─ format.ts              # 통화/숫자 포맷
│  ├─ types/
│  │  └─ index.ts               # Goal/Investment/Emotion 타입
│  └─ App.tsx
└─ README.md
```

---

## ✅ To-Do (실행 순서)

1. 프로젝트 초기화 & 기본 세팅
2. 데이터 모델 & 스토리지 계층(AsyncStorage + Zustand)
3. 네비게이션 구조 설정(Stack + Tabs)
4. 목표 관리(목록/생성/수정/삭제)
5. 투자 기록(기여금 추가·감정 이모지)
6. 진행률 차트(이상적 vs 실제)
7. 필요 수익률 계산기(복리/단리 × 연/월/주/일)
8. 매매 시뮬레이션(승률·손익비 → 기대값 비교)
9. 전체 분석 대시보드(감정·요일)
10. 테마/접근성(다크/라이트, number-pad, 라벨)
11. 더미 데이터 주입(개발 전용)
12. 앱 메타(app.json) 정리
13. EAS 빌드 & 내부 테스트
14. 스토어 자산(아이콘/스크린샷/설명)
15. 개인정보처리방침 & 데이터 안전 설문
16. Play Console 등록 → 내부 테스트 → 프로덕션 릴리스
17. 버전 업데이트 플로우 정립
18. (옵션) CSV 내보내기, 다국어/통화, 성능/로깅

> 상세 체크리스트는 아래 "Prompt Library" 각 항목 참고

---

## 🔧 로컬 실행

```bash
npm i -g expo-cli
npx create-expo-app agt-mobile --template
# 템플릿: blank (TypeScript)
cd agt-mobile
npm i zustand @react-navigation/native @react-navigation/native-stack @react-navigation/bottom-tabs
npx expo install react-native-screens react-native-safe-area-context
npm i @react-native-async-storage/async-storage victory-native
npx expo install react-native-svg
npm i moti react-native-reanimated
npm run start
```

---

## 📦 빌드(EAS)

```bash
npm i -g eas-cli
EAS_NO_VCS=1 eas login
EAS_NO_VCS=1 eas init
EAS_NO_VCS=1 eas build:configure
# preview(profile) → 디버그/내부 테스트용
EAS_NO_VCS=1 eas build -p android --profile preview
# production(profile) → 스토어 업로드용 AAB
EAS_NO_VCS=1 eas build -p android --profile production
```

> Windows/개인 PC에서 Git 세팅 전이라면 `EAS_NO_VCS=1` 환경변수로 VCS 체크를 우회할 수 있습니다.

---

## 🗂️ .gitignore (권장)

```
# dependencies
/node_modules

# expo
.expo
.expo-shared

# build outputs
/dist
/build
*.aab
*.apk

# env
*.env
.env*
```

---

## 🧾 라이선스 (예시: MIT)

```
MIT License

Copyright (c) 2025 …

Permission is hereby granted, free of charge, to any person obtaining a copy…
```

(루트에 `LICENSE` 파일로 추가 권장)

---

## 🧰 Prompt Library (복붙용)

### 1) 프로젝트 초기화 & 기본 세팅

> “Expo(React Native + TypeScript)로 `agt-mobile` 프로젝트 스캐폴딩 코드를 만들어줘. Zustand, React Navigation(Stack+BottomTabs), AsyncStorage 래퍼(JSON 직렬화), 다크/라이트 토글 훅, 폴더 구조(src/screens, src/components, src/store, src/lib, src/types)와 ESLint/Prettier 설정, 예제 HomeScreen 포함.”

### 2) 데이터 모델 & 스토리지 계층

> “다음 타입을 기준으로 AsyncStorage CRUD 유틸과 Zustand 스토어를 작성해줘. 키는 `agt_goals_v1`, `agt_investments_v1`, `agt_theme_v1`. 초기 로딩, 변경 시 저장, 에러 핸들링 포함.
>
> ````ts
> type Emotion = '🚀'|'🤔'|'😬'|'😴'|'🎉'|'🧘'|'😢'|'😎';
> type Goal = { id:string; name:string; targetAmount:number; targetDate:string; startDate:string; initialInvestment:number; };
> type Investment = { id:string; goalId:string; amount:number; date:string; note?:string; emotion?:Emotion; };
> ```”
> ````

### 3) 네비게이션 구조

> “React Navigation으로 BottomTabs(Home/Analysis) + Stack(GoalDetail, Modals)을 구성하는 코드 템플릿을 만들어줘. 타입 안전한 param list, 헤더 타이틀, Android 백버튼 처리 포함.”

### 4) 목표 관리(목록/생성/수정/삭제)

> “Home에서 카드 리스트(진행률 바 포함), ‘목표 추가’ FAB → 모달(이름, 목표금액, 시작일, 목표일, 초기투자). 수정/삭제 액션과 폼 검증(빈값·음수 방지) 포함한 Goal CRUD UI를 구현해줘.”

### 5) 투자 기록(기여금 추가)

> “GoalDetail의 ‘투자 일지’ 탭을 구현해줘. 최신순 리스트(금액·날짜·메모·감정). ‘기여금 추가’ 버튼 → 모달에서 금액(number-pad), 메모, 감정(🚀 🤔 😬 😴 🎉 🧘 😢 😎) 선택 후 저장. 저장 시 goalId로 Investment 생성.”

### 6) 진행률 차트(이상적 vs 실제)

> “react-native-svg로 `MiniLineChart` 컴포넌트를 만들어줘. 입력: startDate, endDate, idealStart, idealEnd, series(\[{date, value}]). 이상적 직선 + 실제 꺾은선, 좌우 날짜 라벨, 점선 그리드 포함. 성능 위해 경량 Path 사용.”

### 7) 필요 수익률 계산기

> “현재금액(current)에서 목표금액(target)까지 목표일(end) 기준 남은 기간을 연/월/주/일로 환산하고, **복리/단리** 각각의 ‘기간당 필요 수익률’을 계산하는 훅과 UI를 만들어줘. 수식: 복리 `(target/current)^(1/n)-1`, 단리 `(target-current)/(current*n)`; 엣지케이스 방어.”

### 8) 매매 시뮬레이션

> “승률 `p`(0.2~~0.8), 손익비 `R`(0.5~~4.0), 기간(주/월) 입력 시, 목표기간 성장률 `neededPerPeriod`와 기대값 `E = p*R - (1-p)` 비교하는 컴포넌트를 작성해줘. 부족 시 `R_needed = (g + 1 - p)/p`, `p_needed = (g + 1)/(R + 1)` 계산을 안내.”

### 9) 전체 분석 대시보드

> “Investments 배열로 ‘총 횟수/총액/평균액’ 요약, 감정별(8종) 집계(건수/총액/평균액), 요일별(일\~토) 건수 히스토그램 계산 헬퍼와 UI를 만들어줘. 막대그래프는 react-native-svg 직사각형.”

### 10) 테마 & 접근성

> “`useColorScheme` + 앱 내 토글로 다크/라이트를 제어하는 훅과 컨텍스트를 만들어줘. 터치 영역 44dp 이상, 스크린리더 라벨, number-pad 키보드 지정 등 접근성 반영.”

### 11) 더미 데이터 주입(개발용)

> “dev 환경에서만 작동하는 seed 함수로 샘플 목표 2개와 랜덤 투자 10\~20건을 생성해 AsyncStorage에 기록하는 코드를 작성해줘. `__DEV__` 가드 추가.”

### 12) 앱 메타 설정(app.json)

> “`app.json`을 작성해줘. 이름 ‘자산 목표 트래커’, slug `agt-mobile`, android.package `com.<닉네임>.agt`, icon/splash 경로, versionCode 1, 권한 없음.”

### 13) EAS 빌드 & 내부 테스트

> “EAS로 Android 빌드를 준비하는 단계별 가이드를 출력해줘. eas init → build\:configure → keystore 생성 → preview(profile)로 APK 빌드, production(profile)로 AAB 빌드 커맨드와 주의점을 정리.”

### 14) 스토어 자산(문구/아이콘/스크린샷)

> “Google Play 스토어용 문구를 한국어로 작성해줘: ① 80자 이내 짧은 설명, ② 400~~4,000자 긴 설명, ③ 핵심 기능 5~~8개 불릿, ④ ‘디바이스에만 저장·제3자 공유 없음’ 문구 포함.”

### 15) 개인정보처리방침 & 데이터 안전

> “로컬 저장(AsyncStorage)만 사용하고 서버로 전송하지 않는 개인 금융 앱의 **개인정보처리방침** 초안을 한국어로 작성해줘. 항목: 수집 항목, 저장 위치, 보유·삭제, 제3자 제공 없음, 문의 이메일 템플릿.”

### 16) Play Console 등록 & 릴리스

> “Google Play Console에서 신규 앱 등록 → 내부 테스트 트랙 배포 → 프로덕션 승격 체크리스트를 알려줘. 필수 스크린샷 해상도, 정책 체크, 심사 반려 빈출 사유 포함.”

### 17) 버전 업데이트 플로우

> “마이너 업데이트 절차: android.versionCode 증가, 체인지로그 포맷(한국어 예시), EAS production 빌드, 내부 테스트 → 프로덕션 승격.”

### 18) 성능/안정화(옵션)

> “리스트/차트 중심 RN 앱의 성능 최적화 체크리스트를 작성해줘. FlatList 가상화, 메모이제이션, SVG 리렌더 최소화, Reanimated 레이아웃, 배치 업데이트, Sentry 로깅 등.”

### 19) CSV 내보내기(옵션)

> “Investments 배열을 CSV로 직렬화하고 Android 공유 시트를 통해 `agt_export_YYYYMMDD.csv`로 내보내는 유틸과 UI 코드를 작성해줘.”

### 20) 다국어/통화(옵션)

> “KRW 기본에서 USD/EUR 옵션을 추가하는 통화 포맷터와 설정 화면, i18next 기반 ko/en 전환 예시 코드를 작성해줘.”

---

## 📝 커밋 가이드(예시)

* feat: 목표 CRUD 화면 추가
* feat(chart): 이상적 vs 실제 라인차트 구현
* feat(sim): 승률·손익비 시뮬레이터 추가
* chore: app.json 메타 정리
* build: EAS production 프로파일 구성

---

## 📨 배포 체크리스트(요약)

* [ ] app.json 메타/아이콘/스플래시
* [ ] 개인정보처리방침 공개 URL
* [ ] 데이터 안전 설문(디바이스 저장/공유 없음)
* [ ] 내부 테스트 트랙 빌드/배포
* [ ] 프로덕션 릴리스 노트 작성

---

## 🙌 기여

PR/Issue 환영합니다. 브랜치 규칙·코드 스타일은 ESLint/Prettier 설정을 따릅니다.

---

## 📄 라이선스

MIT (또는 원하는 라이선스를 `LICENSE` 파일로 추가)
