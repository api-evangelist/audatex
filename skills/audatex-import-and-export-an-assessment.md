---
generated: '2026-09-17'
method: generated
name: Import and export an assessment in AxFormat
description: Find or create an AudaConnect assessment from an AxFormat document, read it back, export it, and render a PDF report.
api: openapi/audatex-audaconnect-api-openapi.yml
operations: [Assessment_Search, AssessmentImport_ImportAssessmentV2, Assessment_GetSummary, Assessment_GetAssessment, AssessmentExport_ExportAssessment, AssessmentReport_GetReportDefinitions, AssessmentReport_Get]
source: >-
  Grounded in openapi/audatex-audaconnect-api-openapi.yml (Swagger 2.0, verbatim from
  https://audaconnect.ax-aee.co.uk/AudaAPI.WebAPI/swagger/docs/v1); auth per
  authentication/audatex-authentication.yml, errors per errors/audatex-problem-types.yml,
  reversibility per conventions/audatex-conventions.yml.
---

# Import and export an assessment in AxFormat

AxFormat is Audatex's proprietary assessment interchange document (JSON or XML). This flow moves an assessment into AudaConnect, reads it, and takes it back out.

## Auth
- OAuth 2.0 bearer token (see `authentication/audatex-authentication.yml`). Scopes: `Assessment.MainIndex` to search, `Assessment.EditAssessmentAdminData` to import, `Assessment.Detail+ImageAccess` to read/export/report.
- Base URL: `https://audaconnect.ax-aee.co.uk/AudaAPI.WebAPI`.

## Steps
1. **Search before you create** — `Assessment_Search` (`POST /api/assessments/search`, or `GET /api/assessments/vrn/{registration}` by vehicle registration). There is no idempotency key and no delete: an import that is retried creates a second assessment.
2. **Import** — `AssessmentImport_ImportAssessmentV2` (`POST /v2/api/assessments/importAxFormat`, body: the AxFormat string). Prefer V2 over `AssessmentImport_ImportAssessment`: V2 uses the assessment number carried in the document. `400` = "The input data is invalid or not understood". Note that importing does NOT raise `ASSESSMENT.STATUS.CREATED`.
3. **Read it back** — `Assessment_GetSummary` (`GET /api/assessments/{id}/summary`) for the header, or `Assessment_GetAssessment` (`GET /api/assessments/{id}`) for the full document. `{id}` is the uuid or the composite originator + assessment number. Reading via `Assessment_GetAssessment` does not create a lock.
4. **Export** — `AssessmentExport_ExportAssessment` (`GET /api/assessments/{id}/exportAxFormat`). Exporting here does NOT raise `ASSESSMENT.EXPORT.CREATED`.
5. **Render a report** — `AssessmentReport_GetReportDefinitions` (`GET /api/reports`) to pick a definition, then `AssessmentReport_Get` (`GET /api/assessments/{id}/report`) for the PDF. The report call does not create a lock either.

## Rules
- Every operation accepts and returns JSON or XML; send an explicit `Accept` header.
- Reversibility: none of import, image add or parts upload can be undone (`conventions/audatex-conventions.yml` reversibility block); `Assessment_Close` and `Assessment_CompleteAssessment` move state forward only.
- 84 of 93 WebAPI operations declare only a `200`; treat any non-2xx as opaque and quote the `Correlation-Id` header to the service desk.
