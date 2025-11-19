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

-----

## 💻 코드 예시 (Custom POST Body 요청)

추가된 필터링 조건을 포함한 **POST** 요청 예시입니다.

### cURL (POST 요청 본문 포함)

curl --location 'https://안내받으신 메일 서버 도메인 기입/api/massive/v1/emails' \
--header 'Content-Type: application/json' \
--data '{
    "api_key": "dfd6fd9fsdfbjdnfkd38fndjfd023",
    "sender": "sender@pringo.co.kr",
    "recipient": "recipient@pringo.co.kr",
    "campaign_title": "보유 포인트 유효기간 안내",
    "limit": 50
}'

