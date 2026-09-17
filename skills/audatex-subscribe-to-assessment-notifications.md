---
generated: '2026-09-17'
method: generated
name: Subscribe to assessment notifications
description: Discover AudaConnect notification topics, create a poll or push subscription to a hierarchical ASSESSMENT.* topic, drain notifications safely, and unsubscribe.
api: openapi/audatex-audaconnect-api-openapi.yml
operations: [Notification_GetTopics, Notification_GetSubscriptions, Notification_Subscribe, Notification_PushSubscribe, Notification_GetNotifications, Notification_Unsubscribe]
source: >-
  Grounded in openapi/audatex-audaconnect-api-openapi.yml (Swagger 2.0) and the topic catalog at
  https://audaconnect.ax-aee.co.uk/AudaAPI.WebAPI/Help/Topic/NotificationTopics; auth per
  authentication/audatex-authentication.yml, events per asyncapi/audatex-webhooks.yml, errors per
  errors/audatex-problem-types.yml.
---

# Subscribe to assessment notifications

Receive AudaConnect events (assessment created / updated / sent / completed, images added, mail received) without polling every assessment.

## Auth
- OAuth 2.0 bearer token from `https://audaconnect.ax-aee.co.uk/AudaAPI.Portal/OAuth20` (authorization-code flow recommended; tokens last 1800 s, refresh with `grant_type=refresh_token`). See `authentication/audatex-authentication.yml`.
- Scope `System.Notifications` for poll subscriptions, `System.PushNotifications` for push subscriptions.
- Base URL: `https://audaconnect.ax-aee.co.uk/AudaAPI.WebAPI` (demo: `https://audaconnect-demo.ax-aee.co.uk/AudaAPI.WebAPI`).

## Steps
1. **List topics** — `Notification_GetTopics` (`GET /api/notifications/topics`). Topics are hierarchical: subscribing to `ASSESSMENT.STATUS` receives every `ASSESSMENT.STATUS.*` notification. The 12 documented topics are listed in `asyncapi/audatex-webhooks.yml`.
2. **Check existing subscriptions first** — `Notification_GetSubscriptions` (`GET /api/notifications/subscriptions`). Subscribing twice to the same topic answers `409 Already subscribed`; reuse the existing `SubscriptionId` instead of retrying a create.
3. **Subscribe** — either
   - poll style: `Notification_Subscribe` (`POST /api/notifications/subscriptions`, body `SubscribeData {TopicCode}`), or
   - push style: `Notification_PushSubscribe` (`POST /api/notifications/pushsubscriptions`, body `SubscribeDataWithPush {TopicCode, Push {URL, Format: JSON|XML, AuthEnabled, AuthScheme, AuthParameter}}`).
   Both return `SubscriptionResponse {SubscriptionId}`. `400` = topic format invalid, `404` = non-existing topic.
4. **Drain notifications** — `Notification_GetNotifications` (`GET /api/notifications/{subscriptionId}`), roughly every 5 minutes (the BMS twin of this operation says so in its summary). A push subscription can also be polled to recover after downtime on your receiver. Each `NotificationMessage` carries `EventType`, `Topic`, `Parameters[] {Key, Value}` (e.g. `assessmentId`) and `Resources[] {ResourceType, ResourceId}`.
5. **Act on the resource** — fetch the assessment named in `Parameters.assessmentId` with `Assessment_GetSummary` or `Assessment_GetAssessment` (scope `Assessment.Detail+ImageAccess`).
6. **Unsubscribe when done** — `Notification_Unsubscribe` (`DELETE /api/notifications/subscriptions/{id}`). This is the only reversal on this surface; there is no documented retention window for undelivered notifications.

## Rules
- No idempotency key exists (`conventions/audatex-conventions.yml`): never blind-retry step 3 — re-list (step 2) and reuse.
- `ASSESSMENT.STATUS.UPDATED` is NOT raised for image add/delete, mail, reassignment, authorisation or completion — subscribe to those topics explicitly.
- `ASSESSMENT.STATUS.CREATED` is not raised by `importAxFormat`, and `ASSESSMENT.EXPORT.CREATED` is not raised by `exportAxFormat`.
- Quote the `Correlation-Id` response header to servicedesk@audatex.co.uk on any `500`.
