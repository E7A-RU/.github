# E7A Gateway — independent network-access product

**Status: EXISTING PRIVATE REPOSITORY / DESIGN IN DRAFT PR / EXECUTABLE IMPLEMENTATION UNVERIFIED.** Source of evidence: [E7A-RU/gateway](https://github.com/E7A-RU/gateway), `main` at `906cb871cae355f04bce76a66cbcf01077e2e738`, [Gateway PR #1](https://github.com/E7A-RU/gateway/pull/1) head `6a13aae01dec8cb272766eac9f0e39125d042558` (reviewed 2026-10-30). These are read-only repository observations, **not verification of live Butlerov network state**.

## Product charter

**Gateway** is a standalone E7A product for a personal/family VPN access control plane, initially targeting the existing WireGuard topology on **Butlerov**. It is **not** an enterprise-wide HTTP API gateway, not the E7A Platform connector broker, not a generic Infra Control Plane and not a custom cryptography/VPN protocol.

Initial user need: provision/revoke family members' devices using official free WireGuard clients and one named connection endpoint `gateway.e7a.ru`, without WGDashboard/S-UI as the family-facing management interface. The single-hostname target is a proposal, not a proven universal public connection capability.

## Ownership and component boundaries

| Component | Owns | Must not own |
| --- | --- | --- |
| Gateway | User-facing family access, device/peer inventory, provisioning requests, product-specific auth policy, desired peer config revision, change operations, sanitized status and Gateway audit | Raw VPN traffic, WireGuard cryptography, production-wide DNS/network operations, generic cross-product IAM or Infrastructure |
| WireGuard/kernel | VPN dataplane / peer protocol and runtime traffic | E7A business identity, web UI, cross-product governance |
| Infra | Host networking, secrets, host privileged boundary, routing/firewall policy, service topology, system backup/recovery, approval and execution safeguards | Product family membership business semantics |
| Identity (optional initial dependency) | Shared identity for registered persons and accounts when integrated | LAN routing, VPN peer ownership, product-specific device ACL |
| Central Audit (progressive) | Standard protected security audit events | Full VPN traffic logs or unrestricted personal-data retention |
| Core | Versioned UI/SDK/typed interfaces | Privileged WireGuard apply helper or host root credentials |
| Platform (optional) | Notifications/Jobs/Connectors where a concrete cross-product capability is needed | Mandatory forwarding of all VPN packets or VPN control-plane state |
| Website | Catalog/positioning and links to approved public Gateway page | Public exposure of privileged management interface |

Gateway may initially keep locally owned identities/access mappings and migrate securely to E7A Identity later; this is a **bounded compatibility bridge**, not authorization to duplicate canonical Person records as a new organization-wide authority.

## Existing topology described in Gateway draft — operationally unverified here

Gateway architecture notes claim Butlerov Debian 13; `wg-home` UDP/443 with MikroTik in `10.90.0.1/30`; `wg-exit` to existing VDS and policy routing table 100; special Mac test policy route via S-UI; nginx TCP/443 exposing pre-existing `/control/` S-UI. **Treat these as Gateway PR's operator-read diagnostic claims, not confirmed current production evidence in this governance review.**

Critical invariant: **No changes** to existing `wg-home`, `wg-exit`, MikroTik PBR, VDS, s-ui routing, nginx or production firewall without a separate owner-approved rollout and tested rollback. UDP/443 and TCP/443 are different transports, but an available TCP endpoint is not evidence of mobile UDP reachability. Do not allocate arbitrary `10.91.0.0/24` peer space without interface address/route/NAT/return-path proof.

## Implementation and safety requirements

1. Dedicated product repository and release lifecycle. TypeScript-first API and optional Next.js/React UI with pinned Core UI version after compatible package checks. Local storage (e.g. SQLite) only after durability/backup/concurrency review.
2. A **narrow root-owned helper** receives validated declarative changes, strict allowlisted resources and versioned expected state; UI/API workers run unprivileged, never get arbitrary shell/root/docker socket. Define trust boundary, authentication and command signatures before implementing host modifications.
3. `preview → approval → apply → verify → rollback` with durable desired state and atomic/recoverable host operations where possible. Use lock/revision checks, unique peer public keys and tunnel IPs, configuration backup and demonstrated failure recovery. Never directly rewrite `wg-quick` config with naive concurrent string manipulation.
4. Client-side private-key generation preferred. If server-side export/QR is ever used, require one-time secret delivery, restricted encrypted short retention, expiry and scrubbing; never log/export private keys in Git or test fixtures.
5. Family/device peers default-deny home LAN, metadata/management UI and other sensitive networks; explicitly allow only required DNS/exit. Protect against IPv6 and DNS leaks, routing bypass, tenant cross-access and split/full-tunnel misconfiguration.
6. Management UI is private/authenticated and isolated from existing S-UI `/control/`; MFA, CSRF/session protection, administrative rate limits, explicit break-glass and service-health monitoring. No exposure of control plane by setting up public VPN DNS alone.
7. Preserve existing MikroTik/VDS/S-UI paths until measured migration with owner permission. Read-only inventory precedes any provisioning. Test a real mobile client over a relevant external network, validating handshake/DNS/external IP and teardown without impacting current users.
8. Governance status transitions: DESIGN → IMPLEMENTED → TESTED → STAGING-VERIFIED → OWNER-APPROVED PRODUCTION → RUNNING-VERIFIED. A draft topology document does not establish test/production health.
9. No GitHub-hosted Actions CI, package publication, production deployment or privileged system modification without explicit owner permission. Relevant exact-SHA checks must be local/self-hosted.

## Open design / owner decisions

- Home-user scope only vs future external commercial VPN service (not implied by this private family MVP).
- Distinct `Account / Person / Family Member / Device / Peer` IDs and permission model, bootstrap admin, integration timeline with E7A Identity.
- Client address plan and migration routing rules, endpoint exposure and S-UI coexistence; verify Butlerov inventory immediately before applying.
- Key custody and recovery/rotations for lost devices; encrypted one-time QR/config UX.
- How the privileged helper enforces authorization without trusting UI claims; malicious/compromised controller behavior and audit on failure.
- Time-bounded owner production approval and safeguards for risk R3/R4 network changes; tested rollback may require a separate reachable management path.

Related: [ecosystem charter](ecosystem-charter.md), [component ownership](ownership-and-data.md), [Infra](infra-operations.md), [Identity](identity-and-trust.md), [final baseline](final-architecture-baseline-2026-10-27.md).
