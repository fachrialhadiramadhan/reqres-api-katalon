# Reqres API Katalon 🧪

> Automated API testing of the [reqres.in](https://reqres.in) REST API using **Katalon Studio** — 33 test cases, 11 API objects, 8 test suites, one-click regression.

![Katalon Studio](https://img.shields.io/badge/Tool-Katalon%20Studio-blue)
![Language](https://img.shields.io/badge/Language-Groovy-orange)
![API](https://img.shields.io/badge/API-REST%20reqres.in-teal)
![Test Cases](https://img.shields.io/badge/Test%20Cases-33-green)
![Bugs Found](https://img.shields.io/badge/Contract%20Bugs-11-red)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

---

## ✨ 3 Highlights

### 1️⃣ Full API Coverage — Auth to Delete
**33 test cases covering the complete request lifecycle** — from authentication (Login / Register), Users CRUD (GET / POST / PUT / PATCH / DELETE), Resources, to delayed-response scenarios — organized into 8 test suites and a single-click **Test Suite Collection** for full regression runs.

### 2️⃣ 11 API Contract Bugs Identified & Documented
Not just happy-path testing. Edge cases were aggressively probed — empty payloads, missing fields, invalid ID formats, non-existent pages — and every deviation from expected HTTP semantics was **flagged as a `(BUG)` test case and documented** in the table below. All actual behaviors were verified against the live API.

### 3️⃣ Parameterized & Environment-Driven
**11 reusable API objects** with variable bindings (`baseUrl`, `user_name`, `user_job`, `user_id`, `page`, `delay`) driven by `GlobalVariable.baseUrl` — switch environments and swap test data without touching the test scripts.

---

## 📡 Endpoint Coverage

| # | API Object | Endpoint | Method | Tested By |
|---|------------|----------|--------|-----------|
| 1 | Auth/Login | `/api/login` | POST | TC-001 → TC-007 |
| 2 | Auth/Register | `/api/register` | POST | TC-008 → TC-011 |
| 3 | Resources/List Resource | `/api/unknown` | GET | TC-012 |
| 4 | Resources/Single Resource | `/api/unknown/{id}` | GET | TC-013, TC-014 |
| 5 | Users/Create User | `/api/users` | POST | TC-015 → TC-018 |
| 6 | Users/Single User | `/api/users/{id}` | GET | TC-019, TC-020 |
| 7 | Users/List Users | `/api/users?page={page}` | GET | TC-021, TC-022 |
| 8 | Users/Update User | `/api/users/{id}` | PUT | TC-023 → TC-027 |
| 9 | Users/Patch (Update) User | `/api/users/{id}` | PATCH | TC-028, TC-029 |
| 10 | Users/Delete User | `/api/users/{id}` | DELETE | TC-030 → TC-032 |
| 11 | Users/Delay Response | `/api/users?delay={delay}` | GET | TC-033 |

## 📂 Test Suites

| Test Suite | Contains | Focus |
|------------|----------|-------|
| Login Test Suite | TC-001 → TC-007 | Auth happy path + negatives |
| Register Test Suites | TC-008 → TC-011 | Registration edge cases |
| Create User Test Suite | TC-015 → TC-018 | POST validation |
| Get User Test Suites | TC-019 → TC-022 | GET by id / list / paging |
| Update User Test Suite | TC-023 → TC-027 | PUT validation |
| Delete User Test Suite | TC-030 → TC-032 | DELETE + invalid input |
| Resources Test Suite | TC-012 → TC-014 | Unknown resources |
| Delay Test Suite | TC-033 | Response latency |
| **Test Suite Collection** | **All 8** | One-click full regression |

---

## 🐛 Contract Bugs Found (Expected vs Actual)

> Expected values follow standard HTTP semantics (server should validate). Actual values verified against the live API.

| TC | Scenario | Expected | Actual | Note |
|----|----------|:--------:|:------:|------|
| TC-016 | Create user without `job` | 400 | **201** | Server accepts partial payload |
| TC-017 | Create user without `name` | 400 | **201** | Server accepts partial payload |
| TC-018 | Create user — blank form `{}` | 400 | **201** | Empty name & job are persisted |
| TC-022 | GET list users, page 23 (out of range) | 404 | **200** + `data: []` | Empty array returned instead of 404 |
| TC-024 | Update non-existent user | 404 | **200** | Mock always succeeds |
| TC-025 | Update user without `job` | 400 | **200** | Server accepts partial payload |
| TC-026 | Update user without `name` | 400 | **200** | Server accepts partial payload |
| TC-027 | Update user — blank form `{}` | 400 | **200** | Empty payload accepted |
| TC-029 | PATCH with empty body | 400 | **200** | No payload validation |
| TC-031 | DELETE non-existent user (99999999) | 404 | **204** | Always succeeds |
| TC-032 | DELETE invalid id format (`abc`) | 400 | **204** | No format validation |

**Takeaway:** reqres.in is a mock API without payload validation — a production service with the same contract would need stricter request validation. Documenting this gap is exactly what these tests are for.

---

## 🗂 Project Structure

```
reqres-api-katalon/
├── Object Repository/        # 11 parameterized API objects
│   ├── Auth/                 #   Login, Register
│   ├── Resources/            #   List & Single resource
│   └── Users/                #   Create, Get, Update, Patch, Delete, Delay
├── Scripts/                  # 33 test cases (Groovy)
│   ├── Auth/                 #   TC-001 → TC-011
│   ├── Resources/            #   TC-012 → TC-014
│   └── Users/                #   TC-015 → TC-033 (incl. 11 (BUG) cases)
├── Test Suites/              # 8 suites + Test Suite Collection
├── Profiles/                 # Environment profile (GlobalVariable.baseUrl)
├── build.gradle
└── console.properties
```

---

## 🚀 Getting Started

### Prerequisites
- [Katalon Studio](https://katalon.com/download-studio-free) (latest)
- Internet access to `https://reqres.in`

### Clone
```bash
git clone https://github.com/fachrialhadiramadhan/reqres-api-katalon.git
```

### Run
1. Open the cloned project in **Katalon Studio**.
2. Open **Test Suites → Test Suite Collection**.
3. Click **Execute** — all 8 suites run sequentially.

> Set `GlobalVariable.baseUrl` in **Profiles/default** to switch environments (default: `https://reqres.in`).

---

## 🛠 Tech Stack

| Tool | Role |
|------|------|
| Katalon Studio | Test automation framework |
| Groovy | Test scripting |
| REST API | reqres.in |
| WS Keywords | `sendRequest`, `verifyResponseStatusCode`, `verifyElementPropertyValue`, `containsString` |

---

## 👤 Author

**Fachri Alhadi Ramadhan** — QA Engineer

- GitHub: [@fachrialhadiramadhan](https://github.com/fachrialhadiramadhan)
- Project Link: [reqres-api-katalon](https://github.com/fachrialhadiramadhan/reqres-api-katalon)

---

## 📄 License

Distributed for portfolio and learning purposes.
