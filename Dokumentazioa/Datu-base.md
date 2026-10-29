---
layout: page
title: Datu-baseak
---

## 1. E/R sortu:
Streaming plataforma ezagun batek bere datu-basea berritu nahi du. Honako hau da lana egiteko jaso dugun informazioa:

Erabiltzaileei buruz honako datu hauek gorde nahi dira: ID-Erabiltzailea, izena, abizena, NAN, telefonoa eta e-maila.

Plataformak hainbat plan desberdin eskaintzen ditu, eta bakoitzak bere ID-Plana eta mota (Oinarrizkoa iragarkiekin, oinarrizkoa eta premiuma) dute. Erabiltzaile bakoitzak gutxienez plan bat kontratatu behar du zerbitzua erabiltzeko eta, noski, plan bakoitza erabiltzaile askok kontratatuko dute.

Plan bakoitzak harpidetza bakarra izaten du lotuta, eta plan bakoitzari gutxienez harpidetza bat dagokio. Harpidetza bakoitzak bere ID-Harpidetza, hasiera-data eta amaiera-data izango ditu. Harpidetza bakoitza plan bakar bat edo gehiagori egongo da beti lotuta.

Erabiltzaile bakoitzak bere kontuaren barruan Rol bat baina gehiago kudeatu ditzake. Rolei buruz informazio hau dugu: ID-Rola eta mota (Pertsonala, Familiarra eta Infantila). Rol bakoitza erabiltzaile bakar bati dagokio.

Plataforman mota askotako edukiak daude eskuragarri. Eduki bakoitzari buruz bere ID-Edukia eta izenburua jakin nahi dira. Edukiak hiru motatakoak izan daitezke: Serieak, Pelikulak edo Dokumentalak.

Rol bakoitzak edukiak bistaratu ditzake. Rol batek gutxienez eduki bat ikusiko du, eta eduki bakoitza gutxienez rol batek baino gehiago ikusiko du. Rol batek eduki bat ikusten duen bakoitzean, bistaratze hori erregistratu nahi da, ikustaldi horren informazioa gordez.

Azkenik, serieek egitura berezia dute. Serie bakoitzak denboraldi asko izango ditu. Denboraldi bakoitza serie bakar batekoa da, eta ID-Denboraldia kodeaz gain, bere zenbakia gordetzen da. Era berean, denboraldi bakoitzak gutxienez kapitulu asko izango ditu, eta kapitulu bakoitza denboraldi zehatz bati dagokio.

![Diagrama DatuBasea](Argazkiak_Datu-base/diagrama.png)

## 2. Eredu erlazionala
![Erlazionala](Argazkiak_Datu-base/erlazionala.png)

## 3. Datu-basearen normalizazioa

## 1. HARPIDETZA

**Eskema:** `HARPIDETZA (ID-Harpidetza, Hasiera_data, Amaiera_data)`

- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Bai. Dependentzia partzialik ez dago.
- **3FN:** Bai. Dependentzia funtzional transitiborik ez dago.

## 2. PLANA

**Hasierako eskema:** `PLANA (ID-Plana, ID-Harpidetza, Mota)`

- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Ez. Dependentzia partziala dago.

**Deskonposizioa:**

- `ID-Plana → Mota`
- `PLANA (ID-Plana, ID-Harpidetza)`

**3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 3. ERABILTZAILEA

**Hasierako eskema:** `ERABILTZAILEA (ID-Erabiltzailea, ID-Plana, Izena, Abizena, NAN, Mugikor zenbakia, E-maila)`

- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Ez. Dependentzia partziala dago.

**Deskonposizioa:**

- `ID-Erabiltzailea → Izena, Abizena, NAN, Mugikor zenbakia, E-maila`
- `ERABILTZAILEA (ID-Erabiltzailea, ID-Plana)`

**3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 4. ROLA

**Hasierako eskema:** `ROLA (ID-Rola, ID-Erabiltzailea, Izena)`

- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Ez. Dependentzia partziala dago.

**Deskonposizioa:**

- `ID-Rola → Izena`
- `ROLA (ID-Rola, ID-Erabiltzailea)`

**3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 5. IKUSI

**Hasierako eskema:** `IKUSI (ID-Izena, ID-Edukia, Bistaratzea)`

- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Ez. Dependentzia partziala dago.

**Deskonposizioa:** `IKUSI (ID-Izena, ID-Edukia, Bistaratzea)`

- **3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 6. EDUKIA

**Eskema:** `EDUKIA (ID-Edukia, Izenburua, Mota)`

- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Bai. Dependentzia partzialik ez dago.
- **3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 7. PELIKULAK

**Eskema:** `PELIKULAK (Zuzendaria, Iraupena_min)`

- `id`: `EDUKIKO` taularako kanpo-gakoa (`id`).
- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Bai. Dependentzia partzialik ez dago.
- **3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 8. DOKU

**Eskema:** `DOKU (Gaia, Iraupena_min)`

- `id`: `EDUKIKO` taularako kanpo-gakoa (`id`).
- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Bai. Dependentzia partzialik ez dago.
- **3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 9. SERIEAK

**Eskema:** `SERIEAK`

- `id`: `EDUKIKO` taularako kanpo-gakoa (`id`).
- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Bai. Dependentzia partzialik ez dago.
- **3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 10. DENBORALDIAK

**Hasierako eskema:** `DENBORALDIAK (ID-Denboraldia, ID-Edukia, Zenbakia)`

- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Ez. Dependentzia partziala dago.

**Deskonposizioa:**

- `ID-Denboraldia → Zenbakia`
- `DENBORALDIAK (ID-Denboraldia, ID-Edukia)`

**3FN:** Bai. Ez dago dependentzia funtzional transitiborik.

## 11. KAPITULUAK

**Hasierako eskema:** `KAPITULUAK (ID-Kapitulua, ID-Denboraldiak, Izena)`

- **1FN:** Bai. Ez dago errepikapenik; datuak atomikoak dira.
- **2FN:** Ez. Dependentzia partziala dago.

**Deskonposizioa:**

- `ID-Kapitulua → Izena`
- `KAPITULUAK (ID-Kapitulua, ID-Denboraldiak)`

**3FN:** Bai. Ez dago dependentzia funtzional transitiborik.
## 4. Inplementazioa Access-en