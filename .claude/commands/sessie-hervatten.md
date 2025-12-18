# Sessie Hervatten Protocol

Een vorige sessie is afgesloten met `/sessie-afsluiten`. Laad de context om verder te gaan waar die sessie stopte.

## Stap 1: Git Sync

Voer uit:
```bash
git fetch origin
git rev-list HEAD..origin/main --count
```

Als achterstand > 0:
- Toon: "Er zijn {n} nieuwe commits op remote"
- Voer uit: `git pull origin main`

## Stap 2: Instance Identificatie

Lees `LOCAL_INSTANCE.md` in de repository root.

Als bestand bestaat:
- Parse `instance_id` (bijv. "SCHEMAS-WITTETOREN")
- Toon: "Instance: {instance_id}"

Als bestand NIET bestaat:
- Toon: "LOCAL_INSTANCE.md niet gevonden - gebruik 'SCHEMAS-UNKNOWN'"
- Gebruik `instance_id = "SCHEMAS-UNKNOWN"`

## Stap 3: Registry Check

Lees `docs/instances/registry.yaml`.

Check voor conflicten:
- Als andere instance status="active" op zelfde branch:
  - Toon: "CONFLICT: {other_instance} is actief op dezelfde branch!"
  - Vraag bevestiging om door te gaan

Toon actieve instances:
- "Actieve instances: {lijst}" of "Geen andere instances actief"

Check stale active (> 24 uur):
- Toon waarschuwing indien gevonden

## Stap 4: Checkpoint Load

Zoek meest recente checkpoint: `docs/checkpoints/CHECKPOINT-*.md`

Toon:
- Checkpoint naam
- Machine waar gemaakt
- Datum
- Korte samenvatting
- Open items

## Stap 5: Vraag Werkbeschrijving (BLOKKERENDE INTERACTIE)

### â›” STOP - WACHT OP GEBRUIKER

**Dit is een BLOKKERENDE stap. Je MOET hier stoppen en wachten.**

1. **TOON** exact deze vraag aan de gebruiker:

   ```
   Wat ga je deze sessie doen? (korte beschrijving)
   ```

2. **STOP** en wacht op antwoord van de gebruiker
3. **GEBRUIK** het antwoord als `current_work` in Stap 6

### âŒ NIET TOEGESTAAN:
- Zelf een werkbeschrijving verzinnen
- Aannemen wat de gebruiker gaat doen
- Doorgaan zonder expliciet antwoord

### âœ… CORRECT:
- Vraag stellen en WACHTEN
- Pas na antwoord doorgaan naar Stap 6

---

## Stap 6: Registry Update (NA ANTWOORD STAP 5)

**BELANGRIJK:** Bepaal eerst de huidige branch:
```bash
git branch --show-current
```

Update `docs/instances/registry.yaml` voor jouw instance:
```yaml
{instance_id}:
  status: active
  current_branch: {resultaat van git branch --show-current}
  current_work: "{antwoord van gebruiker uit Stap 5}"
  last_session_start: {ISO timestamp, bijv. 2025-12-17T15:00:00Z}
```

**Race condition mitigatie:** Commit en push met rebase:
```bash
git add docs/instances/registry.yaml
git commit -m "[{instance_id}] chore: Session started"
git pull --rebase origin main   # VERPLICHT: voorkomt merge conflicts
git push
```

Als push faalt door remote changes, herhaal `git pull --rebase` en `git push`.

## Output Format

```
SESSIE HERVATTEN - {instance_id}

Git Sync: {n} commits pulled / Up to date
Instance: {instance_id}
Branch: {current_branch}
Werk: {current_work}
Andere actief: {lijst of "geen"}

Checkpoint: {naam}
   Machine: {machine}
   Datum: {datum}
   Open items: {aantal}

Ready to work!
```
