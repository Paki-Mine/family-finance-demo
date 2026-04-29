# Family Finance App — Demo

Web app per la gestione delle finanze familiari condivise. Dashboard, transazioni, statistiche per categoria, budget mensili, transazioni ricorrenti, gestione multi-membro.

> **Demo pubblica** con dati fittizi — clicca, esplora, modifica liberamente.
> 🔗 **Live demo:** _da inserire dopo il deploy Vercel_

---

## Stack

- **Frontend:** React 18 (single-file, no build step), JSX trasformato in-browser via Babel standalone
- **Backend:** Supabase (Postgres 17) — schema minimale (1 tabella `appdata` con blob JSON)
- **Persistenza:** REST API Supabase con `fetch` + debounce save (1.5s)
- **UI:** CSS-in-JS, dark mode, mobile-first, Italian

## Funzionalità

- 📊 **Dashboard** con saldo mensile, entrate/uscite, sparkline, top categorie
- 💸 **Transazioni** filtrabili per mese / anno / membro, ricerca testuale
- 📈 **Statistiche** con pie chart per categoria e confronto periodi
- 🎯 **Budget** mensili per categoria con barre di avanzamento
- 🔁 **Ricorrenti** automatiche (affitto, bollette, abbonamenti)
- 👨‍👩‍👧‍👦 **Multi-membro** — ogni transazione associabile a un membro famiglia
- 🌙 **Dark mode** nativa
- 📱 **PWA-ready** con manifest

## Architettura

L'app è un single-page deliberatamente minimale: tutto lo stato (transazioni utente, membri, ricorrenze, budget) vive in **un singolo blob JSON** nella tabella `appdata`. Lettura al mount, scrittura debounced ad ogni cambio. Nessun ORM, nessuno strato di data fetching dedicato.

```sql
CREATE TABLE appdata (
  family_id text PRIMARY KEY,
  data      jsonb,
  updated_at timestamptz DEFAULT now()
);
```

Approccio scelto per:
- **Sviluppo rapido** — nessun migration management, lo schema applicativo evolve nel JSON
- **Ottimismo concorrenza** — pensata per uso famiglia, non per multi-tenant ad alta scala
- **Single-file deploy** — un solo HTML statico servibile da qualsiasi CDN

## Demo vs versione completa

Questa è la **versione demo** pensata per essere navigabile pubblicamente:
- Dati seed fittizi (3 membri Marco/Giulia/Famiglia, ~28 transazioni feb-apr 2026)
- **Nessuna autenticazione** — accesso diretto
- RLS aperta sulla riga di demo (innocua, dati finti)
- Le modifiche persistono ma non sono dati personali

La versione privata di produzione include PIN locale e usa un Supabase separato.

## Deploy

```bash
# Vercel
vercel deploy

# o servire come statico
npx serve .
```

Nessun build step richiesto.

## Licenza

MIT
