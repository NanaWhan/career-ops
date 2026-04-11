# Modo: pipeline — Inbox de URLs (Second Brain)

Procesa URLs de ofertas acumuladas en `data/pipeline.md`. El usuario agrega URLs cuando quiera y luego ejecuta `/career-ops pipeline` para procesarlas todas.

## Workflow

1. **Read** `data/pipeline.md` → find `- [ ]` items in the Pending section
2. **For each pending URL**:
   a. Calculate next sequential `REPORT_NUM` (read `reports/`, take highest number + 1)
   b. **Fetch the JD** using Playwright (browser_navigate + browser_snapshot) → WebFetch → WebSearch
   c. If URL not accessible → mark as `- [!]` with note and continue

   ### STEP 0 — Global Remote Pre-Screen (MANDATORY, runs before full eval)
   Read `modes/_profile.md` → "Global Remote Eligibility" section.
   Scan the JD for location/eligibility signals:
   - **FAIL (→ SKIP immediately):** "Remote — US only", "US residents/citizens only", requires work authorization in US/UK/EU, lists specific US states, requires relocation
   - **PASS (→ proceed to full eval):** "Remote (global)", "Worldwide", "Open to international candidates", EMEA/APAC listed, no country restriction stated
   - **AMBIGUOUS (→ evaluate but flag):** Just says "Remote" with no country specified — run full eval but add note: ⚠️ *Confirm global eligibility with recruiter before applying*

   If SKIP: mark as `- [x] #--- | URL | Company | Role | SKIP — not globally open | PDF ❌` and move on. No full evaluation.

   d. **Run full auto-pipeline**: A–F evaluation → Report .md → PDF (if score >= 3.0) → Tracker
   e. **Move from Pending to Processed**: `- [x] #NNN | URL | Company | Role | Score/5 | PDF ✅/❌`

3. **If 3+ pending URLs**, launch agents in parallel (Agent tool with `run_in_background`) for speed.
4. **When done**, show summary table:

```
| # | Empresa | Rol | Score | PDF | Acción recomendada |
```

## Formato de pipeline.md

```markdown
## Pendientes
- [ ] https://jobs.example.com/posting/123
- [ ] https://boards.greenhouse.io/company/jobs/456 | Company Inc | Senior PM
- [!] https://private.url/job — Error: login required

## Procesadas
- [x] #143 | https://jobs.example.com/posting/789 | Acme Corp | AI PM | 4.2/5 | PDF ✅
- [x] #144 | https://boards.greenhouse.io/xyz/jobs/012 | BigCo | SA | 2.1/5 | PDF ❌
```

## Detección inteligente de JD desde URL

1. **Playwright (preferido):** `browser_navigate` + `browser_snapshot`. Funciona con todas las SPAs.
2. **WebFetch (fallback):** Para páginas estáticas o cuando Playwright no está disponible.
3. **WebSearch (último recurso):** Buscar en portales secundarios que indexan el JD.

**Casos especiales:**
- **LinkedIn**: Puede requerir login → marcar `[!]` y pedir al usuario que pegue el texto
- **PDF**: Si la URL apunta a un PDF, leerlo directamente con Read tool
- **`local:` prefix**: Leer el archivo local. Ejemplo: `local:jds/linkedin-pm-ai.md` → leer `jds/linkedin-pm-ai.md`

## Numeración automática

1. Listar todos los archivos en `reports/`
2. Extraer el número del prefijo (e.g., `142-medispend...` → 142)
3. Nuevo número = máximo encontrado + 1

## Sincronización de fuentes

Antes de procesar cualquier URL, verificar sync:
```bash
node cv-sync-check.mjs
```
Si hay desincronización, advertir al usuario antes de continuar.
