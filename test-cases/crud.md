# CRUD Test Cases

## GET /music/{id}

| ID     | Scenario           | Steps                     | Expected                         | Actual             | Status |
| ------ | ------------------ | ------------------------- | -------------------------------- | ------------------ | ------ |
| TC-001 | Existing ID        | GET `/music/1`            | 200 + requested resource         | 200                | PASS   |
| TC-002 | Non-existing ID    | GET `/music/999999`       | 404 Not Found                    | Socket hung up     | FAIL   |
| TC-003 | ID = 0             | GET `/music/0`            | Error response                   | Socket hung up     | FAIL   |
| TC-004 | Negative ID        | GET `/music/-1`           | Error response                   | Socket hung up     | FAIL   |
| TC-005 | String ID          | GET `/music/abc`          | 400 Bad Request                  | 500                | FAIL   |
| TC-006 | Very large ID      | GET `/music/999999999999` | Error response                   | 500                | FAIL   |
| TC-007 | Response structure | GET `/music/1`            | Response matches Swagger schema  | Matches schema     | PASS   |
| TC-008 | Content type       | GET `/music/1`            | `Content-Type: application/json` | `application/json` | PASS   |

---

## POST /music

| ID     | Scenario               | Steps                                   | Expected                             | Actual         | Status |
| ------ | ---------------------- | --------------------------------------- | ------------------------------------ | -------------- | ------ |
| TC-009 | Valid data             | POST valid music object                 | 201 + created resource               | 201            | PASS   |
| TC-010 | Missing required field | POST without required field             | 400 Bad Request                      | 400            | PASS   |
| TC-011 | Empty required field   | POST with empty value                   | Validation error                     | 400            | PASS   |
| TC-012 | Null value             | POST with `null` in required field      | Validation error                     | 400            | PASS   |
| TC-013 | Invalid data type      | POST invalid field type                 | 400 Bad Request                      | 500            | FAIL   |
| TC-014 | Very long string       | POST field with very long value         | Validation error or defined handling | 400            | PASS   |
| TC-015 | Special characters     | POST data containing special characters | Request handled correctly            | 201            | PASS   |
| TC-016 | Unicode characters     | POST data containing Unicode            | Request handled correctly            | 400            | PASS   |
| TC-017 | Duplicate resource     | POST same valid data twice              | Defined duplicate handling           | 422            | PASS   |
| TC-018 | Invalid JSON           | POST malformed JSON                     | 400 Bad Request                      | 500            | FAIL   |
| TC-019 | Empty body             | POST with empty body                    | 400 Bad Request                      | 500            | FAIL   |
| TC-020 | Response structure     | POST valid data                         | Response matches Swagger schema      | Matches schema | PASS   |

---

## PUT /music/{id}

| ID     | Scenario                     | Steps                          | Expected                             | Actual                | Status  |
| ------ | ---------------------------- | ------------------------------ | ------------------------------------ | --------------------- | ------- |
| TC-021 | Update existing resource     | PUT `/music/1` with valid data | Successful update                    | 202                   | PASS    |
| TC-022 | Update non-existing resource | PUT `/music/999999`            | 404 Not Found                        | 400                   | FAIL    |
| TC-023 | ID = 0                       | PUT `/music/0`                 | Error response                       | 400                   | PASS    |
| TC-024 | Negative ID                  | PUT `/music/-1`                | Error response                       | 400                   | PASS    |
| TC-025 | Missing required field       | PUT with missing field         | 400 Bad Request*                     | 202                   | *Review |
| TC-026 | Null value                   | PUT with `null`                | Validation error                     |                       | FAIL    |
| TC-027 | Invalid data type            | PUT with invalid field type    | 400 Bad Request                      | 500                   | FAIL    |
| TC-028 | Empty value                  | PUT with empty field           | Validation error                     |                       | FAIL    |
| TC-029 | Very long value              | PUT with oversized field       | Validation error or defined handling | 400                   | PASS    |
| TC-030 | Verify update                | PUT followed by GET            | GET returns updated data             | Updated data returned | PASS    |

> **TC-025:** Verify the Swagger specification before marking this as PASS/FAIL. If the omitted field is not required for `PUT`, then `202` may be correct.

---

## DELETE /music/{id}

| ID     | Scenario                     | Steps                        | Expected                     | Actual         | Status |
| ------ | ---------------------------- | ---------------------------- | ---------------------------- | -------------- | ------ |
| TC-031 | Delete existing resource     | DELETE `/music/1`            | Successful deletion          | 204            | PASS   |
| TC-032 | Delete non-existing resource | DELETE `/music/999999`       | Defined error response       | 400            | FAIL   |
| TC-033 | ID = 0                       | DELETE `/music/0`            | Error response               | 400            | PASS   |
| TC-034 | Negative ID                  | DELETE `/music/-1`           | Error response               | 400            | PASS   |
| TC-035 | String ID                    | DELETE `/music/abc`          | 400 Bad Request              | 400            | PASS   |
| TC-036 | Repeat deletion              | DELETE same ID twice         | Defined error response       | 400            | FAIL   |
| TC-037 | Verify deletion              | DELETE followed by GET       | Resource no longer exists    | Error response | PASS   |
| TC-038 | Database verification        | DELETE followed by SQL query | Record removed from database | Record removed | PASS   |
