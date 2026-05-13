# \# Бие даалт 14 — DummyJSON API Testing

# 

# API testing using Postman + Newman with CI/CD via GitHub Actions.

# 

# \## Tech stack

# \- Postman (Collection v2.1)

# \- Newman CLI

# \- GitHub Actions

# \- DummyJSON public API

# 

# \## Хэрхэн ажиллуулах

# 

# \### Шаардлага

# \- Node.js 18+

# \- Newman: `npm install -g newman newman-reporter-htmlextra`

# 

# \### Локалд ажиллуулах

# 

# &#x20;   newman run postman/collection.json -e postman/env.dev.json --reporters "cli,htmlextra" --reporter-htmlextra-export reports/api.html

# 

# \## Бүтэц

# 

# &#x20;   bie-daalt-14/

# &#x20;   ├── .github/workflows/api-tests.yml  # CI/CD

# &#x20;   ├── partA/                           # Setup + screenshot

# &#x20;   ├── postman/                         # Collection + environments

# &#x20;   ├── reports/                         # Newman HTML тайлан

# &#x20;   ├── README.md

# &#x20;   └── REFLECTION.md

# 

# \## API

# \*\*DummyJSON\*\* — https://dummyjson.com

# 

# \- 8 request: Auth (Login, Get me), Products (CRUD), Errors (404)

# \- 21 test assertion

# \- JWT chain demonstrated (Login → token → Get me)

# \- 1 pre-request script (random title generator)

# 

# \## CI

# GitHub Actions runs on every push. Тайланг \*\*Actions\*\* tab → `api-test-report` artifact-аас татаж авах боломжтой.

# 

# \## Note

# `env.dev.json` ба `env.ci.json`-н `token` нь placeholder (`REPLACE\_THIS`). Бодит token-ыг Login request явснаар автоматаар chain-ээр шинэчлэгдэнэ.

# 

# \## Test results summary

# 

# Local Newman run: \*\*21/21 tests passed\*\*, 0 failed.  

# CI: GitHub Actions green ✅





