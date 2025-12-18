# Sessie Afsluiten Protocol

Sluit sessie af met checkpoint creatie, registry update, en PR.

## Stap 1: Instance Check

Lees `LOCAL_INSTANCE.md` voor `instance_id` en `machine`.
Fallback naar "SCHEMAS-UNKNOWN" indien niet gevonden.

## Stap 2: Checkpoint Creatie

Vraag gebruiker om korte beschrijving van de sessie (als niet al bekend).

Maak bestand: `docs/checkpoints/CHECKPOINT-{YYYY-MM-DD}-{HHMM}-{beschrijving}.md`

Template:
```markdown
# Checkpoint {YYYY-MM-DD}-{HHMM} - {beschrijving}

**Machine:** {machine}
**Instance:** {instance_id}
**Datum:** {YYYY-MM-DD HH:MM} NL
**Branch:** {branch}

## Sessie Samenvatting

{Beschrijving van wat er gedaan is}

## Bereikte Resultaten

- {Resultaat 1}
- {Resultaat 2}

## Bestanden Gewijzigd

- {bestand 1}
- {bestand 2}

## Open Items voor Volgende Sessie

- [ ] {Item 1}
- [ ] {Item 2}

## Context voor Volgende Agent

{Belangrijke context voor voortzetting, ongeacht machine}

---

*Checkpoint door {instance_id} - {timestamp}*
```

## Stap 3: Registry Update

Update `docs/instances/registry.yaml` voor jouw instance:
```yaml
{instance_id}:
  status: idle
  current_branch: null
  current_work: null
  last_session_end: {ISO timestamp, bijv. 2025-12-17T16:00:00Z}
  last_checkpoint: {checkpoint bestandsnaam}
```

## Stap 4: Git Operations

**Race condition mitigatie:** Commit en push met rebase:
```bash
git add -A
git commit -m "[{instance_id}] docs: Sessie checkpoint - {beschrijving}"
git pull --rebase origin main   # VERPLICHT: voorkomt merge conflicts
git push
```

Als push faalt door remote changes, herhaal `git pull --rebase` en `git push`.

## Stap 5: PR Check

Als op feature branch (niet main):
- Check of PR bestaat: `gh pr view`
- Zo niet: `gh pr create --title "..." --body "..."`
- Toon PR URL

## Output Format

```
SESSIE AFGESLOTEN - {instance_id}

Checkpoint: {bestandsnaam}
Commit: {short SHA}
PR: {URL of "N/A - op main"}

Tot de volgende sessie!
```
