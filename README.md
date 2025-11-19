# 📧 Filter All Messages

## API 개요

  * **메소드:** **`POST`** 
  * **엔드포인트:** `https://안내받으신 메일 서버 도메인 기입/api/massive/v1/emails`

-----

## ⚙️ 요청 상세 정보 (Custom Operation Details)

### 1\. 인증 및 헤더

| 속성 | 위치 | 필수 여부 | 설명 |
| :--- | :--- | :--- | :--- |
| **`Content-Type`** | Header | **Yes** | `application/json` |

### 2\. 요청 본문 (Request Body)

| 이름 | 타입 | 필수 여부 | 설명 | 예시 |
| :--- | :--- | :--- | :--- | :--- |
| **`api_key`** | `string` | **Yes** | 계정관리 - API용 KEY를 그대로 입력하시면 됩니다.| dfd6fd9fsdfbjdnfkd38fndjfd023 |
| **`sender`** | `string` | No | 발신자 이메일 주소로 필터링 | sender@pringo.co.kr |
| **`recipient`** | `string` | No | 수신자 이메일 주소로 필터링 | recipient@pringo.co.kr |
| **`campaign_title`** | `string` | No | 이메일 제목으로 필터링 | '보유 포인트 유효기간 안내'|
| **`limit`** | `number` | No | 페이지당 반환되는 메시지 수입니다. (최대 1000) **기본값:** 10. ||

**요청 본문 형식 (JSON 예시 - 모든 필드 포함):**

```json
{
  "api_key": "<<YOUR_API_KEY_HERE>>",
  "sender": "sender@pringo.co.kr",
  "recipient": "recipient@pringo.co.kr",
  "campaign_title": "보유 포인트 유효기간 안내",
  "limit": 50
}
```

요청하신 대로, 응답 본문의 속성명(Property Names)을 요청 본문에서 사용한 필드명과 더욱 일관되게 수정했습니다.

특히 `from_email`, `to_email`, `subject`를 각각 **`sender`**, **`recipient`**, \*\*`title`\*\*로 변경하여 `200 OK` 응답 스키마와 JSON 예시를 재구성했습니다.

-----

## ✅ Responses (응답)

### 200 OK (성공)

조회 조건에 맞는 메일 목록을 반환합니다.

| 속성 | 타입 | 설명 |
| :--- | :--- | :--- |
| **`messages`** | `array` | 메일 결과 객체의 배열입니다. |
| **`sender`** | `string` | **발신자 이메일 주소** (요청 필터: `sender`) |
| **`msg_id`** | `string` | 고유 메시지 ID |
| **`title`** | `string` | **메일 제목** (요청 필터: `campaign_title`) |
| **`recipient`** | `string` | **수신자 이메일 주소** (요청 필터: `recipient`) |
| **`status`** | `string` | 메일의 처리 상태 (`processed`, `delivered`, `not_delivered` 등) |
| **`opens_count`** | `integer` | 열람 횟수 |
| **`clicks_count`** | `integer` | 클릭 횟수 |
| **`last_event_time`** | `string` | 마지막 이벤트 발생 시간 (ISO 8601 형식) |

**응답 본문 형식 (JSON 예시 - 속성명 변경 적용):**

```json
{
  "messages": [
    {
      "sender": "sender@pringo.co.kr",
      "msg_id": "abc123",
      "title": "보유 포인트 유효기간 안내",
      "recipient": "recipient@pringo.co.kr",
      "status": "delivered",
      "opens_count": 10,
      "clicks_count": 2,
      "last_event_time": "2025-11-19T10:00:00Z"
    },
    {
      "sender": "sender@pringo.co.kr",
      "msg_id": "321befe",
      "title": "보유 포인트 유효기간 안내",
      "recipient": "another@pringo.co.kr",
      "status": "processed",
      "opens_count": 0,
      "clicks_count": 0,
      "last_event_time": "2025-11-19T10:05:00Z"
    }
  ]
}
```

### 400 Bad Request (잘못된 요청)

요청 본문이나 필터 구문이 잘못되었을 때 발생합니다.

```json
{
  "errors": [
    {
      "message": "invalid syntax: 'bad_field' is not a known field"
    }
  ]
}
```

### 429 Too Many Requests (요청 과다)

API 호출 빈도 제한을 초과했을 때 발생합니다.

```json
{
  "errors": [
    {
      "message": "too many requests"
    }
  ]
}
```

-----
