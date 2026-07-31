---
name: Request and track a MEDIGO second opinion
description: Create a second-opinion request, attach medical files, exchange messages, and retrieve the report when ready.
api: openapi/medigo-openapi-original.yml
operations:
- createSecondOpinion
- getSecondOpinion
- uploadFile
- postSecondOpinionMessage
- listSecondOpinionMessages
- listSecondOpinionAttachments
- getSecondOpinionAttachmentContent
---

# Request and track a MEDIGO second opinion

Use the MEDIGO API V2 to obtain a professional review of a diagnosis or treatment.

## Auth
Send `Authorization: Bearer <partner-api-token>` on every request. Requests and
responses are `application/json`. Every response carries a `Trace-Id` header —
keep it for support. Errors come back as `{ code, message }`.

## Steps
1. **Create the request.** Call `createSecondOpinion` with the patient/medical
   details. Capture the returned second-opinion `id`.
2. **Attach supporting files (optional).** Upload each medical document with
   `uploadFile`, then reference them on the second opinion. Confirm they landed
   with `listSecondOpinionAttachments`.
3. **Communicate.** Post questions or clarifications with
   `postSecondOpinionMessage` and read replies with `listSecondOpinionMessages`.
4. **Poll or listen for readiness.** Check status with `getSecondOpinion`, or
   subscribe to the `second_opinion:report_ready` webhook (see
   asyncapi/medigo-webhooks.yml) so you are notified instead of polling.
5. **Retrieve the report.** Once ready, download the report attachment with
   `getSecondOpinionAttachmentContent`.

## Notes
- No idempotency-key contract is documented — avoid blind retries of
  `createSecondOpinion`; on a network failure, reconcile with `getSecondOpinion`.
- `second_opinion:update` and `second_opinion:new_message` webhooks also fire for
  this flow.
