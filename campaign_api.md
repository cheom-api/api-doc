
## 📧 개별 캠페인 발송 상세 정보 조회 API

-----

## ⚙️ 요청 상세 정보 (Custom Operation Details)

### 1\. 인증 및 헤더

| 속성 | 위치 | 필수 여부 | 설명 |
| :--- | :--- | :--- | :--- |
| **`Content-Type`** | Header | **Yes** | `application/json` |

### 2\. 요청 본문 (Request Body)

`(User Variables)`가 `variables` 객체 형태로 추가되었습니다.

| 이름 | 타입 | 필수 여부 | 설명 | 예시 |
| :--- | :--- | :--- | :--- | :--- |
| **`api_key`** | `string` | **Yes** | 계정관리 - API용 KEY를 그대로 입력하시면 됩니다. | dfd6fd9fsdfbjdnfkd38fndjfd023 |
| **`template_id`** | `integer` | **Yes** | 조회할 캠페인(템플릿)의 고유 ID | 1024 |
| **`campaign_type`** | `array` | No | 조회할 메일 상태 필터 (`success`, `fail`, `open`, `click` 등) | `["fail", "open"]` |
| **`(User Variables)`** | `object` | No | **(User Variables)** 특정 사용자 변수 값으로 필터링할 때 사용합니다. | `{"name": "김철수"}` |
| **`limit`** | `number` | No | 페이지당 반환되는 메시지 수 (최대 1000) **기본값:** 10 | 50 |
| **`page`** | `number` | No | 조회할 페이지 번호. **기본값:** 1 | 2 |

**요청 본문 형식 (JSON 예시):**

```json
{
  "api_key": "dfd6fd9fsdfbjdnfkd38fndjfd023",
  "template_id": 1024,
  "campaign_type": ["fail", "open"], 
// (User Variables)
  "limit": 50,
  "page": 1
}
```

-----

## ✅ Responses (응답)

### 200 OK (성공)

해당 캠페인으로 발송된 개별 메일들의 상태와 **사용자가 입력한 변수 값**을 반환합니다.

| 속성 | 타입 | 설명 |
| :--- | :--- | :--- |
| **`mails`** | `array` | 개별 메일 상세 정보 객체의 배열 |
| **`no`** | `integer` | 리스트 내 순번 (Row Number) |
| **`status`** | `string` | 현재 상태 (`success`, `fail`, `open`, `click` 등) |
| **`(User Variables)`** | `string` | **사용자가 입력한 주소록 변수들** (예: `name`, `point`, `coupon_code` 등) |
| **`current_page`** | `integer` | 현재 조회된 페이지 번호 |
| **`total_pages`** | `integer` | 총 페이지 수 |
| **`total_items`** | `integer` | 필터 조건에 맞는 전체 메일 수 |

**응답 본문 형식 (JSON 예시):**

```json
{
  "mails": [
    {
      "no": 1,
      "status": "open",
   // (User Variables)
      
    },
    {
      "no": 2,
      "status": "fail",
   // (User Variables)

    }
    // ... (limit 개수만큼 데이터가 포함됩니다)
  ],
  "current_page": 1,
  "total_pages": 10, 
  "total_items": 500
}
```

### 400 Bad Request (잘못된 요청)

```json
{
  "errors": [
    {
      "message": "Missing required field: template_id"
    }
  ]
}
```
