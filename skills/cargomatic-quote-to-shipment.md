---
name: Quote, order, and create a shipment with Cargomatic
description: Authenticate, request a freight quote, place an order, and create a trackable shipment as a Cargomatic shipper.
api: openapi/cargomatic-openapi-original.yml
operations: [auth, createQuote, createOrder, createShipment, shipmentStatus]
---

# Quote to shipment (shipper)

Use this skill to move from a price quote to a live, trackable shipment on the Cargomatic Public API.

## Prerequisites
- Sandbox credentials from `apisupport@cargomatic.com` (username + password).
- Base URL: `https://api-acceptance.cargomatic.com` (sandbox) or `https://api.cargomatic.com` (production).

## Steps
1. **Authenticate** — `POST /auth` (`operationId: auth`) with `{ "username", "password" }`. Read `token` from the response. Send `Authorization: Bearer <token>` on every subsequent call.
2. **Quote** — `POST /quotes` (`operationId: createQuote`) with the load details (stops, weight, equipment). The response `quote` includes `total_cost`, `shipment_type`, `weight`, and `distance`.
3. **Order** — `POST /orders` (`operationId: createOrder`). The response `order` includes an `order_id` (e.g. `ORD-8246`) and `reference_numbers`.
4. **Shipment** — `POST /shipments` (`operationId: createShipment`) to turn the order into a shipment.
5. **Track** — `GET /shipments/status` (`operationId: shipmentStatus`) or `GET /shipments` (`operationId: listShipments`) to poll `status` and per-`stop` progress.

## Rules
- Errors are `application/json` `{ "message": string }` (not RFC 9457): 400 = fix the request body; 500 = retry with backoff, then contact support. See `errors/cargomatic-problem-types.yml`.
- No idempotency-key contract is published — do not blindly retry writes; check status first. See `conventions/cargomatic-conventions.yml`.
