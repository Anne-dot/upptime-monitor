# Upptime Monitor

Veebilehtede seire GitHub Actions abil.

## Monitooritavad saidid

| Sait | Tüüp | Kontaktid |
|------|------|-----------|
| nirgu.ee | PHP | info@nirgu.ee |
| birdbusters.eu | PHP | info@birdbusters.eu |
| sharpy.ee | PHP | info@sharpy.ee |
| linnupeletaja.ee | PHP | info@nirgu.ee |
| lasteaiapildid.ee | Staatiline | info@lasteaiapildid.ee |
| athliit.ee | PHP | anne.ruusmann@athliit.ee, info@athliit.ee |

## Kontrollid

Iga saidi puhul kontrollitakse:

1. **Kättesaadavus** - kas leht vastab 30 sekundi jooksul
2. **HTTP staatus** - kas tagastab 200 OK
3. **PHP vead** - kas lehel on Fatal/Parse error või Warning (ainult PHP saitidel)
4. **Sisu** - kas vastus pole tühi (<500 bytes)
5. **SSL sertifikaat** - kas aegub rohkem kui 7 päeva pärast

## Workflows

### v1 (`uptime-check.yml`)
- **Sagedus:** iga 5 minutit
- **Struktuur:** 11 paralleelset jobi (maatriks)
- **Kasutus:** ~11 minutit per run
- **Sobib:** PUBLIC repole (piiramatu Actions)

### v2 (`uptime-check-v2.yml`)
- **Sagedus:** iga 30 minutit (või käsitsi)
- **Struktuur:** 1 job, järjestikused kontrollid
- **Kasutus:** ~1 minut per run
- **Sobib:** PRIVATE repole (säästab minuteid)

## Seadistamine

### Secrets (Settings → Secrets → Actions)
- `MAIL_USERNAME` - Gmail aadress
- `MAIL_PASSWORD` - Gmail App Password

### Keskkonnamuutujad (workflow failis)
```yaml
env:
  TEST_MODE: "false"  # true = ainult TECH_EMAIL saab meile
  TECH_EMAIL: "ruusmann@gmail.com"
```

## Kasutus

### Public repo (praegu)
- v1 aktiivne, iga 5 min
- Minuteid pole vaja jälgida

### Private repo (tulevikus)
1. Lülita v1 schedule välja
2. Lülita v2 schedule sisse
3. Muuda v2 `TEST_MODE: "false"`

## Minutite kasutus (private repo)

GitHub Free annab 2000 min/kuu privaatsetele repodele.

| Workflow | Min/run | Runs/päev | Min/päev | Kestab |
|----------|---------|-----------|----------|--------|
| v1 (5 min) | 11 | 288 | 3168 | ~15h ❌ |
| v2 (30 min) | 1 | 48 | 48 | ~41 päeva ✅ |

---

*Loodud Claude Code abil*
