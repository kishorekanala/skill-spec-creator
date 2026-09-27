# Feature catalog — prompts for suggestions

Use this to fill gaps in the feature map, not as a checklist to march through. The idea
itself should drive the core features; these are the areas people routinely forget.
Only suggest what plausibly fits the product.

Most of these areas are **Advanced** for a typical product — they make it better, not
possible. Tier by the removal test from SKILL.md, not by the area's name: sign-in is Core
when each user's data is private, payments are Core for a marketplace that takes money,
an audit log is Core only where a regulation gates launch. The non-functional prompts at
the end belong in the advanced round, except hard constraints (platform, launch-gating
regulation) that the core round asks about once.

## Identity and access
- Sign up (email / phone / SSO / social), email verification
- Sign in, sign out, "remember me", session timeout
- Password reset, password rules, MFA
- Roles and permissions, role assignment, invitations
- Account deactivation / deletion, account recovery

## Core data lifecycle (for each main entity)
- Create, view, edit, delete (soft vs hard delete)
- List, search, filter, sort, paginate
- Bulk actions, import (CSV/API), export
- Versioning / history, undo, archive / restore
- Ownership, sharing, visibility

## Collaboration
- Comments, mentions, reactions
- Assignment, approvals / review workflows
- Real-time presence, concurrent-edit handling

## Notifications
- Channels: in-app, email, push, SMS, webhook
- Triggers and templates, digests / batching
- User preferences, quiet hours, unsubscribe

## Admin and configuration
- Org / tenant settings, feature flags
- User administration, usage limits / quotas
- Branding, custom fields

## Payments and billing (only if monetised)
- Plans, trials, checkout, invoices, refunds, tax
- Payment failure handling, dunning

## Integrations
- Inbound / outbound APIs, webhooks, API keys
- Third-party connectors, SSO providers

## Content and media
- Upload (size/type limits), preview, storage, retention

## Reporting and analytics
- Dashboards, reports, scheduled reports, export

## Offline and sync (mobile / edge)
- Local storage, sync, conflict resolution

## Onboarding and help
- First-run experience, empty states, sample data, in-app help

## Trust and compliance
- Audit log, consent, data export on request, data deletion, retention policies
- Regional rules as relevant (e.g. GDPR, India's DPDP Act, HIPAA, PCI-DSS)

## Error handling and resilience
- Validation messages, retry, rate limiting, graceful degradation

## Non-functional prompts
- Performance: response times, throughput, data volumes
- Availability: uptime target, backup / restore, disaster recovery
- Security: encryption at rest / in transit, secrets, vulnerability handling
- Accessibility: WCAG level
- Localisation: languages, time zones, currencies
- Platforms: web browsers, iOS / Android versions, desktop, APIs
- Observability: logging, metrics, alerting
