---
generated: '2026-09-17'
method: generated
name: Look up and value a vehicle
description: Resolve a UK vehicle from its registration or VIN, fetch detailed specification and paint code, retrieve its history, and obtain a CAP valuation.
api: openapi/audatex-audaconnect-api-openapi.yml
operations: [Vehicle_GetVIwithVRN, Vehicle_GetViWithVinPlus, Vehicle_GetDetailedVIwithVinPlus, Vehicle_GetPaintCodewithVin, Vehicle_GetValuation, History_GetHistory]
source: >-
  Grounded in openapi/audatex-audaconnect-api-openapi.yml (Swagger 2.0, verbatim from
  https://audaconnect.ax-aee.co.uk/AudaAPI.WebAPI/swagger/docs/v1); scopes per
  scopes/audatex-scopes.yml.
---

# Look up and value a vehicle

Read-only flow — safe to retry.

## Auth
- OAuth 2.0 bearer token (`authentication/audatex-authentication.yml`). Scopes: `Vehicle.Lookup`, `Vehicle.LookupDetailed`, `Vehicle.Valuation` (CAP valuations).
- Base URL: `https://audaconnect.ax-aee.co.uk/AudaAPI.WebAPI`.

## Steps
1. **By registration** — `Vehicle_GetVIwithVRN` (`GET /api/vehicle/vrn/{vehicleRegistration}`). The registration must be 3 or more characters (spec note).
2. **By VIN (VIN Plus, UK)** — `Vehicle_GetViWithVinPlus` (`GET /api/vehicle/vin/{vinumber}`); for the full specification use `Vehicle_GetDetailedVIwithVinPlus` (`GET /api/vehicle/vin/{vinumber}/detailed`); for the paint code `Vehicle_GetPaintCodewithVin` (`GET /api/vehicle/vin/{vinumber}/paintcode`).
3. **History** — `History_GetHistory` (`GET /api/history`) returns the assessment history for a vehicle registration.
4. **Value it** — `Vehicle_GetValuation` (`POST /api/vehicle/valuation`) returns the CAP valuation for the registration and mileage you post. Although it is a POST it is a query, not a write.

## Rules
- All four vehicle lookups declare only a `200`; an unknown registration/VIN is not distinguishable from the spec alone — check the body.
- Reference lists for the enumerations you will see (`VehicleType`, `ModelType`, `DamageArea` ...) come from the `ReferenceData_*` operations (scope `ReferenceData.System`).
