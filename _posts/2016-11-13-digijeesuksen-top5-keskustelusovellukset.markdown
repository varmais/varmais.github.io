---
layout: post
author: 'Teemu Tiilikainen'
title: Digijeesuksen TOP-5 keskustelusovellukset
date: 2016-11-13 12:35:16.000000000 +03:00
---

Pohdittiin eilen porukalla kysymystä “onko Telegram vai WhatsApp turvallisempi viestintäväline 
yksityisyyden näkökulmasta?”, joten päätin iskeä faktat tiskiin niiden ja muutaman muunkin keskustelusovelluksen 
osalta. Samalla murretaan myytti erään sovelluksen turvallisuudesta ja lisäksi annan 
suosituksen sovelluksesta, jota kannattaa käyttää mikäli haluaa pitää keskustelun luottamuksellisena.

### Surkein: [Telegram](https://telegram.org)
- On huomattu [vuotavan metadataa](http://motherboard.vice.com/read/encrypted-messaging-app-telegram-leaks-usage-data), ja myös tallentaa sitä palvelimilleen.
- Viestien salaus ei ole oletuksen päällä, vaikka siihen ei ole mitään syytä.
- Ainoa tästä joukosta joka käyttää itse tehtyä salausta, vaikka parempia ja 
yleisesti toimivaksi todettuja vaihtoehtoja on olemassa.

Telegram on jostain syystä turvallisen sovelluksen maineessa, vaikka jo pelkästään [kotikutoisen 
salauksen käyttäminen huolestuttaa](https://eprint.iacr.org/2015/1177.pdf). En suosittele käyttöä.

### Kyseenalaiset:

#### [Google Allo](https://allo.google.com)
- Salaus ei päällä oletuksena.
- Lukee läpi kaikki viestit, jotta voi tarjota palveluita viestittelyn lomaan.
- Tukee “yksityistilaa”, jonka käytön aikana kaikki ominaisuudet ei ole käytössä.
- Tallentaa metadataa kerätäkseen tietoa käyttäjästä.

#### [FB Messenger](https://www.messenger.com)
- Salaus ei päällä oletuksena, sama kuin chatti FB-sivuilla.
- Tallentaa metadataa kerätäkseen tietoa käyttäjästä.
- Tukee “yksityistilaa”, joka pitää avata erikseen jokaista keskustelua varten.

Googlen ja Facebookin tarjoamat chatti-sovellukset ovat kyseenalaisia, koska oletusasetuksilla 
molemmat keräävät käyttäjästä tietoa, jotta voivat käyttää sitä mainoksien myymiseen. Ne 
kuitenkin sisältävät “yksityistilan” keskusteluille, jossa käytetään hyväksi todettua 
Signal-protokollaa, joka on merkittävä parannus Telegramin omaan salaustekniikkaan. Yksityistilastakin 
kuitenkin todennäköisesti kerätään metadataa, kuten kenelle on lähetetty viestejä ja milloin, 
mutta viestien sisältö ei ole ulkopuolisten luettavissa.

### Ihan ok: [WhatsApp](https://www.whatsapp.com)
- Käyttää salausta oletuksena, mikäli kaikki keskustelussa mukana olevat tukevat sitä.
- Kerää metadataa siitä kenelle on lähetetty viestejä ja milloin. Viestien sisältö ei siis 
kuitenkaan ole ulkopuolisten luettavissa.
- Jos käyttää online-backup -ominaisuutta, siirtyvät viestit pilveen ilman salausta. Puhelimessa 
olevien viestien turvallisuus riippuu puhelimen salauksesta.
- Lähettää käyttäjän kontaktit palvelimelle, mikäli käyttäjä antaa luvan.

Myös WhatsApp käyttää Signal-protokollaa keskusteluiden salaamiseen, mutta salaus on päällä 
oletuksena. Palvelun käyttöehdot selkeästi sallivat ja kertovat metadatan keräämisestä, muuten 
tietojen jakaminen eteenpäin on käyttäjän vastuulla.

### Ainoa oikea: [Signal](https://whispersystems.org)
- Salaus oletuksena päällä.
- Ei kerää metadataa, pl. päivän tarkkuudella milloin käyttäjä on ollut viimeksi online.
- Lähettää kontaktit palvelimelle tiivisteenä (sama tekniikka kuin [digitaalisessa allekirjoituksessa](https://fi.wikipedia.org/wiki/Digitaalinen_allekirjoitus))
- Sovellus on avointa lähdekoodia, ansainta perustuu lahjoituksiin.

Signal on saman Open Whisperer Systemsin tuottama sovellus kuin on jo aikaisemmin mainittu Signal-protokolla. 
Sanomattakin on selvää, että myös Signal-sovellus luottaa kyseiseen protokollaan keskusteluiden 
salaamiseksi. Signal on käytännössä ainoa sovellus tai teknologia, jota voi oikeasti suositella sellaiseen 
digitaaliseen kanssakäymiseen, jossa yksityisyys ja tietoturva on tärkeää.
