# NDSC Digital Campus — Architecture and Phased Plan

## Status and assumptions

This plan follows `docs/REPOSITORY_AUDIT.md`. The audited repository is empty apart from its README, so no incumbent framework or server architecture can be retained. Confirm that this is the intended repository before implementation. The stack below is a proposed default, not a claim about an existing system.

## Proposed architecture

Use a **TypeScript modular monolith** initially: one deployable web application with clear public, student, administrative, and server-integration boundaries. This avoids premature microservices while keeping CMS, learning modules, and private student services independently testable.

- **Web:** Next.js App Router + React + TypeScript, with server components for crawlable public content and client components only for interactions.
- **Styling:** CSS variables/tokens and a maintained utility/component approach selected during Phase 1; Plus Jakarta Sans/Inter and Hind Siliguri with locally optimized font delivery.
- **Data:** managed PostgreSQL, Prisma migrations, and transactional server services. Do not create schema until result-source and retention decisions are approved.
- **Authentication:** established server-side session library, hashed opaque session cookies, role-based authorization, CSRF defenses, secure expiry, and rate limiting. Public, `student`, `editor`, and `administrator` capabilities are checked on the server.
- **Files:** private originals in S3-compatible object storage where available; validated MIME/type/size, generated safe names, image transformation, malware scanning policy, and authorized delivery for non-public files.
- **AI:** server-only provider adapter, strict input/output validation, quotas, timeouts, approved-content retrieval, citations/links, and no student/private CMS content in the knowledge index.
- **Interactive learning:** modular experiment registry with SVG/Canvas simulations, typed parameters, learning objectives, and deterministic reset. Load laboratory and panorama viewers only on their routes.
- **Operations:** structured logs without student payloads, audit events, backups/restore drills, error monitoring, health checks, and environment-based secrets.

Suggested boundaries: `public`, `portal`, `admin`, `api`, `components`, `features/{cms,results,assistant,lab,campus,tour}`, `lib/{auth,db,validation,storage}`, and database migrations. Exact paths follow the scaffold's conventions; parallel implementations of the same feature are not permitted.

## Content and data model

Start with versioned/publishable content types for pages, navigation, announcements, notices, news/events, gallery media, achievements, staff/leadership, academic programs, admissions, resources, and site settings. Include draft/published/archived states, authorship, publication timestamps, alt text, attachments, and audit history.

Results and student profiles remain separate from public CMS content. Result access must use server-side entitlement checks and an approved identity workflow; roll numbers or predictable IDs alone are not authorization. Private routes and APIs emit `noindex` and restrictive cache headers. Campus markers and tour scenes require provenance and verification status.

## Delivery phases and gates

### Phase 1 — Foundation

Confirm repository/hosting; scaffold TypeScript application; add lint/typecheck/test/CI; implement tokens, typography, responsive shell, accessible dropdown/mobile navigation, search contract, footer, metadata helpers, error states, and reduced-motion primitives.  
**Gate:** production build and component/accessibility tests pass at 360, 390, 768, 1024, and 1440px; no horizontal overflow.

### Phase 2 — Public website and CMS read path

Build the homepage and complete About, Academics, Admissions, Contact, Notices, News/Events, Gallery, and detail routes. Add filtering, pagination, lightbox keyboard controls, structured data, sitemap, robots, canonical/OG metadata, and optimized media. Use approved content or honest empty states only.  
**Gate:** every navigation target resolves; CRUD-backed publishing is not represented as working until Phase 5; crawl, accessibility, and route tests pass.

### Phase 3 — Institutional services

Implement approved result lookup, student resources, authentication, student profile, sessions, authorization, and recovery/expiry states. Add audit logging and anti-enumeration behavior. Do not import real student data into development.  
**Gate:** authorization tests prove cross-student and public access are denied; security review and synthetic-data integration tests pass.


### Phase 4 — Digital Campus

Deliver the grounded bilingual-capable AI assistant with current approved notices and unavailable-service behavior; launch an initial accurate lab set across Physics, Chemistry, and Biology through the experiment registry; implement a verified campus map and panorama viewer with explicit unavailable/demonstration states.  
**Gate:** each control changes real simulation state; reset works; mobile/keyboard behavior and AI injection/retrieval tests pass; unverified locations or imagery are never published as authentic.

### Phase 5 — Administration

Build the protected dashboard and complete create/edit/publish/unpublish/archive/delete workflows, uploads, gallery ordering, academic/admissions/staff management, result administration, roles, settings, and audit views. Editors receive only required permissions.  
**Gate:** unauthenticated users cannot discover usable upload/admin actions; server authorization, CSRF, rate-limit, file-validation, and audit tests pass.

### Phase 6 — Polish

Apply restrained reveal/counter/navigation/gallery motion, image loading refinement, responsive editorial tuning, WCAG 2.2 AA review, SEO validation, bundle analysis, caching, and performance budgets.  
**Gate:** keyboard/screen-reader and reduced-motion checks pass; agreed Core Web Vitals/lighthouse budgets are measured on representative pages and devices.

### Phase 7 — Validation and release

Run full regression, API, authorization, upload, AI outage, lab, map/tour, browser, accessibility, SEO, and production-build checks. Configure migrations, backups and restoration, monitoring, runbooks, content-editor guide, deployment/rollback documentation, and launch checklist.  
**Gate:** no critical broken routes/actions or high-severity security findings; release approval covers both software and published institutional content.

## Test strategy

Use fast unit tests for validation/services/simulations, component tests for forms/navigation/dialogs, database integration tests for publication and entitlements, and Playwright end-to-end tests for public browsing, portal, and CMS workflows. Include malicious upload, CSRF, session expiry, rate-limit, direct-object-reference, prompt-injection, network-failure, and empty-content cases. Run accessibility checks automatically and manually verify keyboard and screen-reader-critical flows.

## Decisions required before Phase 1

1. Confirm this empty repository is correct and identify any existing deployment or source.
2. Approve the proposed stack and hosting/database/storage constraints.
3. Appoint institutional content approvers and provide verified identity/contact/academic/admissions content and licensed media.
4. Define result data ownership, student identity verification, retention, and administrator roles.
5. Approve AI provider/privacy/budget requirements and map/tour asset availability.

No production feature is marked complete until its phase gate and actual functionality tests pass.
