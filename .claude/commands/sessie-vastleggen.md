# Sessie Vastleggen Protocol

Leg sessie context vast zonder af te sluiten (voor compacting/clear recovery).

Maak tussentijds checkpoint zonder sessie af te sluiten.
Nuttig voor lange sessies of voor verwachte context compacting.

## Stap 1: Instance Check

Lees `LOCAL_INSTANCE.md` voor `instance_id` en `machine`.
Fallback naar "SCHEMAS-UNKNOWN" indien niet gevonden.

## Stap 2: Checkpoint Creatie

Vraag gebruiker om korte beschrijving.

Maak checkpoint bestand (zelfde format als /sessie-afsluiten):
`docs/checkpoints/CHECKPOINT-{YYYY-MM-DD}-{HHMM}-{beschrijving}.md`

## Stap 3: Git Operations

**Race condition mitigatie:** Commit en push met rebase:
```bash
git add -A
git commit -m "[{instance_id}] docs: Tussentijds checkpoint - {beschrijving}"
git pull --rebase origin main   # VERPLICHT: voorkomt merge conflicts
git push
```

Als push faalt door remote changes, herhaal `git pull --rebase` en `git push`.

**Let op:** Registry wordt NIET geÃ¼pdatet (sessie blijft active).

## Output Format

```
CHECKPOINT VASTGELEGD - {instance_id}

Checkpoint: {bestandsnaam}
Commit: {short SHA}

Sessie blijft actief. Gebruik /sessie-afsluiten om af te sluiten.
```
