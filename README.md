# Giga — Template presentazione

Template ufficiale per le presentazioni branded **Giga / UNICEF Digital Impact**.

- **File:** `PPT giga.pptx`
- **Uso:** design di riferimento per generare presentazioni (solo i testi cambiano). Vedi il pacchetto [giga-deck](https://github.com/...) per lo script che usa questo template.

## Pubblicare la repo su GitHub

1. Vai su [github.com/new](https://github.com/new).
2. Nome repo: es. **giga-template-repo** (o **giga-template**).
3. Scegli **Public**, non inizializzare con README (è già in locale).
4. Crea la repo, poi in locale:

```bash
cd /Users/mariodelprete/Desktop/giga-template-repo
git remote add origin https://github.com/TUO_UTENTE/giga-template-repo.git
git push -u origin main
```

Sostituisci `TUO_UTENTE` con il tuo username GitHub.

## Link diretto al template (dopo il push)

Per usare il file via URL (es. con lo script `giga_from_template.py`):

```
https://github.com/TUO_UTENTE/giga-template-repo/raw/main/PPT%20giga.pptx
```
