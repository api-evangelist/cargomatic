---
name: Accept a shipment and dispatch a driver with Cargomatic
description: As a Cargomatic carrier, accept a shipment, create and assign a driver, and progress the stops to completion.
api: openapi/cargomatic-openapi-original.yml
operations: [auth, acceptShipment, updateAppointment, createDriver, assignDriver, addStop, arriveAtStop, completeStop]
---

# Carrier dispatch and stop management

Use this skill to operate the carrier side of a Cargomatic shipment: accept the work, assign a driver, and drive the stops to completion.

## Prerequisites
- Carrier sandbox credentials from `apisupport@cargomatic.com`.
- Authenticate first: `POST /auth` (`operationId: auth`), then send `Authorization: Bearer <token>`.

## Steps
1. **Accept** — `POST /shipments/accept` (`operationId: acceptShipment`) to take a shipment as a carrier.
2. **Appointment** — `POST /shipments/appointment` (`operationId: updateAppointment`) to set/adjust the pickup or delivery appointment.
3. **Create driver** — `POST /carrier/{carrierid}/driver` (`operationId: createDriver`) to register a driver under your carrier id.
4. **Assign driver** — `POST /shipment/assign` (`operationId: assignDriver`) to attach the driver to the shipment.
5. **Manage stops**:
   - `POST /stops/add` (`operationId: addStop`) to add a stop.
   - `POST /stops/transition` (`operationId: arriveAtStop`) when the driver arrives.
   - `POST /stops/complete` (`operationId: completeStop`) when the stop is done.

## Rules
- Every operation except `auth` requires the bearer token; a missing/expired token yields a 400. See `authentication/cargomatic-authentication.yml`.
- Errors are `application/json` `{ "message": string }`. See `errors/cargomatic-problem-types.yml`.
- Entity relationships (Carrier → Driver, Shipment → Stop) are in `data-model/cargomatic-data-model.yml`.
