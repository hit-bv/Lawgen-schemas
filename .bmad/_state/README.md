# BMAD State Directory

Deze directory bevat de runtime state voor BMAD-gebaseerde AI-agents.

## Doel

De `_state/` directory slaat operationele data op die agents gebruiken om context te behouden tussen sessies:

- **Sessie checkpoints** - Vastgelegde context bij sessie-afsluiting
- **Agent memories** - Persistente kennis en leerpunten
- **Taak tracking** - Status van lopende werkzaamheden
- **Context snapshots** - Relevante projectinformatie

## Structuur

Bestanden worden automatisch aangemaakt door agents tijdens gebruik:

```
_state/
├── README.md           # Dit bestand
├── checkpoints/        # Sessie snapshots (optioneel)
└── *.yaml             # Agent-specifieke state bestanden
```

## Belangrijk

- State bestanden zijn **repo-specifiek** en niet gedeeld tussen repos
- Oude state bestanden kunnen veilig worden opgeruimd
- Deze directory wordt **wel** gecommit naar git voor team-zichtbaarheid
