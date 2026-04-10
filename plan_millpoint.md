# MillPoint – Produktplan

## Vision

MillPoint er en komplet platform til værktøjshåndtering og apps til maskinværkstedet. Den skal kunne udvides med moduler over tid.

## Kernekrav

- Modulær arkitektur: nye moduler/apps kan tilføjes løbende.
- Data for: maskiner, værktøjer, fixturer, nulpunkter, værktøjsoffsets.
- Login-beskyttet adgang med en forsidelanding der beskriver tilgængelige moduler og apps.
- Multi-tenant: hvert login er knyttet til et **firma**, så data deles inden for firmaet (ikke på tværs).

## Tek-stack (forslag)

| Lag | Valg |
|-----|------|
| Frontend | Next.js (React + TypeScript) |
| Backend / API | Next.js (API routes / server actions); evt. senere NestJS ved større team |
| Database | **SQLite** (MVP) med vej til migration til PostgreSQL senere |
| ORM | Prisma |
| Auth | NextAuth, eller hosted (Clerk / Auth0) |
| UI | Tailwind CSS + shadcn/ui |
| Hosting (MVP) | Vercel eller VPS; SQLite som fil med backup |
| Filer (manualer, billeder) | S3-kompatibelt lager (fx Cloudflare R2) når behov opstår |

### SQLite-noter

- Godt til MVP og få samtidige skrivere.
- Brug WAL mode for bedre concurrency (`PRAGMA journal_mode=WAL`).
- Daglig backup af `.db`-filen.
- Design med Prisma fra start så skift til Postgres bliver ligetil.

## Datamodel (MVP – overordnet)

Alle forretnings-tabeller scopes med `company_id`.

- `companies`
- `users` + `memberships` (user ↔ company, rolle: admin / member)
- `machines`
- `tools`
- `fixtures`
- `datums` (nulpunkter)
- `tool_offsets`
- `activity_logs` (audit: hvem ændrede hvad)

Indeksering: `company_id`, kombinationer som `(company_id, machine_id)`, `(company_id, tool_number)`.

## MVP (fase 1)

1. **Auth & firma:** login, invitation, bruger tilhører ét firma i MVP; roller admin/member.
2. **Dashboard:** overblik (antal maskiner/værktøjer/fixturer, seneste ændringer), links til moduler.
3. **Maskiner:** CRUD (navn, fabrikat, model, styring, lokation, noter).
4. **Værktøjer:** CRUD (nummer, type, mål, holder, status, noter); søgning/filter.
5. **Fixturer:** CRUD (id/navn, maskine, placering, noter).
6. **Nulpunkter & offsets:** registrering pr. maskine/fixtur og værktøjsoffsets; simpel historik/audit.
7. **Sikkerhed:** al data filtreret på `company_id`; ingen cross-company adgang.

## Senere moduler (efter MVP)

- Rapportering og eksport, integrationer (CAM/ERP), avancerede roller, flere lokationer under samme firma, mobilapp, osv.

## Relateret repo-arbejde

- Fusion post (`fanuc-test.cps`): værktøjs-/offset-format i NC kan tilpasses uafhængigt af webappen; hold post-filer og app-kode i samme repo eller som submodule efter behov.

---

*Sidst opdateret: 2026-04 (plan + tech-stack + MVP sammenfattet for implementering.)*
