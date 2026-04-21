# MillPoint - Projektplan (Clean)

## Formaal
Bygge et operatoervenligt system til CNC-vaerkstedet, som erstatter Excel-ark med en modulopbygget loesning for maskiner, vaerktoejer og fixturer.

## MVP-scope (Fase 1)
- **Maskiner:** basisregister (navn, model, placering, status).
- **Vaerktoejer:** relation til maskine + placering (`skabNr`, `posNr`) + status.
- **Fixturer:** relation til maskine + `palleplads` + noter.
- **Tool detail popup:** klik paa vaerktoej viser detaljer, kommentarer og billeder.

## Arkitekturprincipper
- **Modulaer opbygning:** moduler kan aktiveres/deaktiveres uden stor ombygning.
- **Krydsmodul-data:** data kan hentes paa tvaers af moduler via faelles datamodel.
- **Fremtidssikring:** designet skal kunne udvides med nye moduler senere (fx kalibrering, rapportering).
- **Praktisk drift:** loesningen skal fungere stabilt i vaerkstedets hverdag.

## Foreslaaet teknisk retning
- Next.js (App Router) + TypeScript
- Prisma ORM
- SQLite til tidlig MVP, mulighed for PostgreSQL senere
- Tailwind + komponentbibliotek til enkelt, tydeligt UI
- Filupload til billeder i vaerktoejsdetaljer (faseopdelt)

## Datamodel (minimum)
1. `Machine` (1) -> (N) `Tool`
2. `Machine` (1) -> (N) `Fixture`
3. `Tool` indeholder `cabinetNumber`, `positionNumber`, `comments`, `images`
4. `Fixture` indeholder `palletLocation`

## Implementeringsfaser
1. **Grunddata og CRUD**
   - Opret moduler: maskiner, vaerktoejer, fixturer
   - CRUD-sider med simple tabeller og soegning
2. **Tool popup**
   - Klikbar vaerktoejsrække
   - Modal med felter, kommentarer og billedeliste
3. **Modulregister**
   - Central registry for enable/disable af moduler
   - Faelles API-moenster for moduler
4. **Stabilisering**
   - Test af relationer, dataflow og brugerrejse i vaerkstedet
   - Fejlretning og performance-tuning

## Done-kriterier
- Excel-ark for kerneflow kan erstattes af systemets 3 MVP-moduler.
- Vaerktoejs-popup virker med detaljer, kommentarer og billedevisning.
- Relationer mellem maskiner, vaerktoejer og fixturer fungerer korrekt.
- Modularitet er etableret, saa nye moduler kan tilfoejes uden redesign.

## KPI'er (foerste maaling)
- Reduceret opstillingstid
- Faerre opstillingsfejl
- Hojejere brugeradoption hos operatoerer
- Mindre manuel administration
