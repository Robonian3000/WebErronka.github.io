# Datu Baseak:
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



## 2. Eredu erlazionala

## 3. Datu-basearen normalizazioa

## 4. Inplementazioa Access-en