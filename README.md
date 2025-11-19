## 📧 Filter All Campagins
대량메일 결과 조회 api
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
| **`limit`** | `number` | No | 페이지당 반환되는 메시지 수입니다. (최대 1000) **기본값:** 10. | 50 |
| **`page`** | `number` | No | **조회할 페이지 번호입니다. 기본값은 1입니다.** | 2 |

**요청 본문 형식 (JSON 예시 - 모든 필드 포함):**

```json
{
  "api_key": "<<YOUR_API_KEY_HERE>>",
  "sender": "sender@pringo.co.kr",
  "recipient": "recipient@pringo.co.kr",
  "campaign_title": "보유 포인트 유효기간 안내",
  "limit": 50,
  "page": 2
}
```

-----

## ✅ Responses (응답)

### 200 OK (성공)

조회 조건에 맞는 메일 목록을 반환합니다.

| 속성 | 타입 | 설명 |
| :--- | :--- | :--- |
| **`messages`** | `array` | 메일 결과 객체의 배열입니다. |
| **`campaign_id`** | `string` | 고유 캠페인 ID |
| **`campaign_title`** | `string` | 메일 캠페인 제목 (요청 필터: `campaign_title`) |
| **`sender`** | `string` | 발신자 이메일 주소 (요청 필터: `sender`) |
| **`recipient`** | `string` | 수신자 이메일 주소 (요청 필터: `recipient`) |
| **`status`** | `string` | 메일의 처리 상태 (`processed`, `delivered`, `not_delivered` 등) |
| **`send_time`** | `string` | 메일 발송 완료 시간 (ISO 8601 형식) |
| **`total_count`** | `integer` | 해당 캠페인 전체 발송 시도 수 |
| **`success_count`** | `integer` | 해당 캠페인 발송 성공 수 |
| **`fail_count`** | `integer` | 해당 캠페인 발송 실패 수 |
| **`opens_count`** | `integer` | 열람 횟수 |
| **`clicks_count`** | `integer` | 클릭 횟수 |
| **`last_event_time`** | `string` | 마지막 이벤트 발생 시간 (ISO 8601 형식) |
| **`current_page`** | `integer` | **현재 조회된 페이지 번호** |
| **`total_pages`** | `integer` | **총 페이지 수** |
| **`total_items`** | `integer` | **필터 조건에 맞는 전체 항목 수** |

**응답 본문 형식 (JSON 예시 - 페이지네이션 적용):**

```json
{
  "messages": [
    {
      "campaign_id": "abc123",
      "campaign_title": "보유 포인트 유효기간 안내",
      "sender": "sender@pringo.co.kr",
      "recipient": "recipient@pringo.co.kr",
      "status": "delivered",
      "send_time": "2025-11-19T09:30:00Z",
      "total_count": 50000,
      "success_count": 49950,
      "fail_count": 50,
      "opens_count": 10,
      "clicks_count": 2,
      "last_event_time": "2025-11-19T10:00:00Z"
    },
    {
      "campaign_id": "321befe",
      "campaign_title": "보유 포인트 유효기간 안내",
      "sender": "sender@pringo.co.kr",
      "recipient": "another@pringo.co.kr",
      "status": "processed",
      "send_time": "2025-11-19T09:35:00Z",
      "total_count": 50000,
      "success_count": 49950,
      "fail_count": 50,
      "opens_count": 0,
      "clicks_count": 0,
      "last_event_time": "2025-11-19T10:05:00Z"
    }
    // ... (limit 개수만큼 데이터가 포함됩니다)
  ],
  "current_page": 2,
  "total_pages": 10000, // 예시: 50만 건 / 50개 limit = 10,000 페이지
  "total_items": 500000 
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
