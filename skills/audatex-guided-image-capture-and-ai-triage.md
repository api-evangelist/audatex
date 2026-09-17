---
generated: '2026-09-17'
method: generated
name: Guided image capture and AI triage
description: Send a policyholder a guided image-capture request, receive completion via webhook, run AI Triage on the captured images, and fetch the damage-detection report.
api: openapi/audatex-api-gateway-openapi.yml
operations: []
paths: ['POST /api/v1/Webhooks', 'POST /api/v1/ImageCapture/WithLink', 'GET /api/v1/ImageCapture/{imageCaptureId}', 'POST /api/v2/AiTriage', 'GET /api/v2/AiTriage/{imageCaptureId}/{calculationId}', 'GET /api/v1/Reports/{format}/IntelligentDamageDetection/{imageCaptureId}/{commandId}/{language}', 'DELETE /api/v1/ImageCapture/{imageCaptureId}']
source: >-
  Grounded in openapi/audatex-api-gateway-openapi.yml (OpenAPI 3.0.1, verbatim from
  https://services-pat.auda-target.com/APIGateway/swagger/V1/swagger.json). The spec declares NO
  operationIds, so every step is bound by method + path; events per asyncapi/audatex-webhooks.yml.
---

# Guided image capture and AI triage

## Auth
- `Authorization: Bearer <JWT>` (securityScheme `Bearer`). Tokens are issued to onboarded partners; the published host is the pre-production acceptance-test (PAT) environment `https://services-pat.auda-target.com/APIGateway`.

## Steps
1. **Register a webhook once** — `POST /api/v1/Webhooks` with `WebhookReqViewModel {callbackUri, authentication, webhookEvents: [ImageRequest_Completed, ImageRequest_Expired, ImageCollection_Ai_Triage_Completed]}`. `201` returns `SelfViewModel {id, self}`. Check `GET /api/v1/Webhooks` first — duplicates are not prevented.
2. **Send the capture request** — `POST /api/v1/ImageCapture/WithLink` (returns the link in the response) or `POST /api/v1/ImageCapture` (SMS only) or `/WithQR`. **This sends an SMS to the customer on every call**: there is no idempotency key, so on a timeout read `GET /api/v1/ImageCapture/{imageCaptureId}` (or `/Query/{imageCaptureId}` for the light view) before re-sending.
3. **Wait for `ImageRequest_Completed`** on your callback (or poll step 2's GET). Delivery status arrives as `ImageRequest_Sent / Delivered / Undelivered / Opened`; `ImageRequest_Reminder` fires if a reminder was scheduled (`POST /api/v1/Reminder`).
4. **Run AI Triage** — `POST /api/v2/AiTriage` for the capture; `202` returns `CalculationSelfViewModel` with a `calculationId`. Poll `GET /api/v2/AiTriage/{imageCaptureId}/{calculationId}` or wait for `ImageCollection_Ai_Triage_Completed`. Sibling flows: `POST /api/v1/ViDamagedParts`, `/api/v1/ViFullCalculation`, `/api/v2/ViNoCalculation`.
5. **Fetch the report** — `GET /api/v1/Reports/{format}/IntelligentDamageDetection/{imageCaptureId}/{commandId}/{language}`.
6. **Clean up (optional)** — `DELETE /api/v1/ImageCapture/{imageCaptureId}` emits `ImageRequest_Deletion_Completed`; it does not unsend the SMS.

## Rules
- `400` bodies are ASP.NET Core `ProblemDetails` / `ValidationProblemDetails` or a field-to-message dictionary, served as `application/json` (not `problem+json`) — see `errors/audatex-problem-types.yml`.
- `429` is declared only on `POST /api/v1/PingEvent`; no limit or `Retry-After` is documented (`rate-limits/audatex-rate-limits.yml`).
- The webhook callback payload schema is undocumented; use `GET /api/v1/Webhooks/debug` to inspect what was sent for an `imageCaptureId`.
