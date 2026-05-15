# 5. API 명세서 (API Specification)

본 문서는 앱과 연구 서버 간의 인터페이스를 정의합니다.

## 공통 설정
- **Base URL:** `https://kpnc.chemainet.com/vest2025`
- **Content-Type:** `application/json`
- **인증 방식:** HTTP Bearer Token (Authorization Header)

## 1. 로그인 (Login)
연구 대상자 인증을 수행하고 세션 토큰을 발급받습니다.

- **Endpoint:** `/auth/login.do`
- **Method:** `POST`
- **Request Body (JSON Example):**
```json
{
  "name": "홍길동",
  "number": "01012345678"
}
```

- **Response Body (200 OK):**
```json
{
  "accessToken": "eyJhbGciOiJIUzI1...",
  "ptcseq": 123
}
```
- **Error Codes:**
  - `404 Not Found`: 등록되지 않은 사용자 정보.

## 2. 건강 데이터 동기화 (Health Data Sync)
수집된 건강 데이터를 기기 모델 정보 및 동기화 시간과 함께 서버로 업로드합니다. 본 API는 여러 날짜(`DailyRecord`)와 각 날짜별 상세 항목(`HealthRecord`)을 포함하는 계층적 구조를 가집니다.

- **Endpoint:** `/health/sync.do`
- **Method:** `POST`
- **Request Body (JSON Example):**
```json
{
  "ptcseq": 123,
  "deviceModel": "iPhone 15 Pro",
  "syncTime": 1715655600,
  "dailyRecords": [
    {
      "date": "2026-05-14",
      "healthRecords": [
        {
          "type": "steps",
          "value": 5420.0,
          "unit": "count",
          "startTime": 1715612400,
          "endTime": 1715655600
        },
        {
          "type": "heartRate",
          "minValue": 62.0,
          "maxValue": 128.0,
          "unit": "bpm",
          "startTime": 1715612400,
          "endTime": 1715655600
        }
      ]
    }
  ]
}
```

- **Response Body (200 OK):**
```json
{
  "success": true,
  "message": "Sync successful",
  "syncId": 456
}
```
