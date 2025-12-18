# SCHEMAS - Claude Code CLI Instructions

---

## ðŸ–¥ï¸ MULTI-INSTANCE AWARENESS

Dit repository ondersteunt werken met Claude Code agents op meerdere machines.

### Machine Identity

**Lees bij sessie start:** `LOCAL_INSTANCE.md` (repository root)
- Dit bestand identificeert op welke machine je draait
- Gebruik de `instance_id` in logs en checkpoints
- Als het bestand ontbreekt: toon waarschuwing, gebruik "SCHEMAS-UNKNOWN"

**Instance Registry:** `docs/instances/registry.yaml`
- Toont status van alle bekende instances
- Update bij /sessie-hervatten (status â†’ active)
- Update bij /sessie-afsluiten (status â†’ idle)

### Branch Convention

**Format:** `{agent}/{machine}-{beschrijving}`

**Voorbeelden:**
- `schemas/wittetoren-feature-x`
- `schemas/ieniemienie-research`
- `schemas/mac-planning`

### Session Commands

- `/sessie-hervatten` - Start sessie (git sync + registry + checkpoint load)
- `/sessie-afsluiten` - Einde sessie (checkpoint + registry + PR)
- `/sessie-vastleggen` - Tussentijds checkpoint (voor compacting recovery)

### â›” VERPLICHTE INTERACTIE BIJ SESSIE START

**Bij /sessie-hervatten:**

1. **EERST** toon context uit laatste checkpoint:
   - Wat er vorige sessie gedaan is
   - Open items die nog moeten gebeuren

2. **DAN** stel de vraag:
   ```
   Wat ga je deze sessie doen? (korte beschrijving)
   ```

3. **WACHT** op antwoord voordat je registry update

**Regels:**
- NOOIT zelf een werkbeschrijving verzinnen
- NOOIT aannemen dat gebruiker checkpoint-items wil doen
- Gebruik het EXACTE antwoord van de gebruiker als `current_work`

### Checkpoint Naming

**Format:** `docs/checkpoints/CHECKPOINT-{YYYY-MM-DD}-{HHMM}-{beschrijving}.md`

**Voorbeeld:** `CHECKPOINT-2025-12-14-1930-multi-instance-implementatie.md`

---
