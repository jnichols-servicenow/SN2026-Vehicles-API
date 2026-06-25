# Build Agent prompt — Vehicles API

Build a Scripted REST API using the ServiceNow Fluent SDK's `RestApi` function (imported from `@servicenow/sdk/core`).

## Context — assume this already exists
- A scoped application already exists with scope `x_snc_vehicles_api`.
- It contains a table named `x_snc_vehicles_api_vehicle` with the fields: `make`, `model`, `year`, `vin`, `country`, `city` (plus standard system fields).
- The table is already populated with demo records.
- Do NOT create the application, the table, or any demo data. Only create the REST API and its resources.

## What to build
One Scripted REST web service with these properties:
- name: `Vehicles API`
- serviceId: `vehicles_api`
- consumes: `application/json`

It must have exactly six resources (routes). For each route, you MUST set the `path` property explicitly — do not omit it. The item routes share the path `/vehicle/{vin}` and are distinguished by HTTP method; the collection routes use the path `/vehicles` and are likewise distinguished by HTTP method.

| Resource name | method | path |
|---|---|---|
| Get Vehicle | GET | `/vehicle/{vin}` |
| Get Vehicles | GET | `/vehicles` |
| Create Vehicles | POST | `/vehicles` |
| Update Vehicle (PATCH) | PATCH | `/vehicle/{vin}` |
| Update Vehicle (PUT) | PUT | `/vehicle/{vin}` |
| Delete Vehicle | DELETE | `/vehicle/{vin}` |

## Implementation rules — follow these exactly
1. **Every route must include a `path` property.** Do not rely on method-only routing. The `{vin}` segment is a path parameter.
2. For routes that read the VIN from the path, get it with `request.pathParams.vin`. For routes that read filter values from the query string, get them with `request.queryParams.<name>`.
3. For routes that read a request body, get the already-parsed body with `request.body.data` (NOT `request.body.dataString` and NOT `global.JSON().decode()`).
4. Query and modify the table with `GlideRecord` against `x_snc_vehicles_api_vehicle`.
5. Use inline scripts in each route's `script` property (do not split handlers into separate module files).
6. Use `Now.ID[...]` for every `$id`.
7. Multiple routes intentionally share a path (`/vehicle/{vin}` is used by GET, PATCH, PUT, and DELETE; `/vehicles` is used by GET and POST). This is correct — they are separate routes distinguished by HTTP method. Do not collapse or deduplicate them.

## Behaviour of each resource

**Get Vehicle (GET `/vehicle/{vin}`)**
- Look up the vehicle by `vin`.
- If found, return an object with `make`, `model`, `vin`, `year`.
- If not found, set status 404 and return `{ error: 'Vehicle not found' }`.

**Get Vehicles (GET `/vehicles`)**
- Declare these three query parameters on the route (all optional):

  | $id | name | exampleValue | required | shortDescription |
  |---|---|---|---|---|
  | country | country | Sweden | false | The country the vehicle is located. |
  | make | make | Volvo | false | The make of the vehicle. |
  | year | year | 2026 | false | The year the vehicle was manufactured. |

- Read these filter values from the query string via `request.queryParams`.
- Build a GlideRecord query, adding a condition for each filter that was supplied (skip any that are absent).
- Loop the results and return `{ vehicles: [ { make, model, vin, year, country }, ... ] }`.
- Read `request.body.data`, which contains a `vehicles` array.
- Loop the array; for each entry, insert a new GlideRecord setting `make`, `model`, `vin`, `year`, `country`, `city`.
- Capture each inserted record's sys_id.
- Set status 201 and return `{ created: [ { vin, sys_id }, ... ] }`.

**Update Vehicle — PATCH (`/vehicle/{vin}`)**
- Look up the vehicle by `vin`; if not found, status 404 and `{ error: 'Vehicle not found' }`.
- Update only the fields present in the body (partial update): check each of `make`, `model`, `year`, `country`, `city` for `!== undefined` before setting it.
- Return `{ sys_id, vin }`.

**Update Vehicle — PUT (`/vehicle/{vin}`)**
- Use the same script/logic as PATCH (partial-update behaviour is acceptable for both in this demo).

**Delete Vehicle (DELETE `/vehicle/{vin}`)**
- Look up the vehicle by `vin`. If found, delete it. If not found, do nothing (idempotent — do NOT return 404).
- Set status 204 and return no body.

## Reference
Use the ServiceNow SDK `RestApi` documentation: https://servicenow.github.io/sdk/api/restapi-api
Note that the documentation's CRUD example omits `path` and parses the body with `request.body.dataString` — do not follow those two patterns here. Set `path` on every route and use `request.body.data` as specified above.
