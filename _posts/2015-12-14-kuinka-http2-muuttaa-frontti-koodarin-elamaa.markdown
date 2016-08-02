---
layout: post
author: 'Teemu Tiilikainen'
title: Kuinka HTTP/2 muuttaa frontti-koodarin elämää
date: 2015-12-14 19:50:35.000000000 +02:00
---
HTTP/2:n kehitteillä olevan [spesifikaation](https://httpwg.github.io/specs/rfc7540.html) perusteet ovat yksinkertaiset, mutta silti "kaikki-mitä-tiesit-on-väärin" -tasoa. Siinä missä HTTP-protokollan ykkösversio sallii vain yhden requestin per yhteys, voi niitä kakkosessa olla useita ja lisäksi palvelin voi **työntää** selaimelle tiedostoja ilman että selain niitä erikseen pyytää. Jos nykyään pienet ikonit sisällytetään base64-enkoodattuna data stringinä CSS:n mukaan, muodostaa HTTP/2:n push-ominaisuus protokollatason inline-mahdollisuuden.

![Push-ominaisuus nopeuttaa tiedostojen latausta](https://pbs.twimg.com/media/CNYFaJlUkAA7oS9.jpg:large)
*Push-ominaisuus nopeuttaa tiedostojen latausta.*

HTTP/2:n myötä useat nykypäivänä käytetyt keinot nopeuttaa sivuston latausta, kuten usean domainin käyttäminen ja tiedostojen yhdistäminen, muuttuu anti-patterneiksi ja voivat haitata sivuston latausta. Kun useiden tiedostojen lataamisesta ei aiheudu käyttäjälle haittaa, voidaan esimerkiksi javascript-koodia tallentaa selaimen välimuistiin moduuli-tasolla.

### Nykytilanne
Nykypäivänä monet sivustot käyttävät tiedostoissa joko versionumerointia tai sisältöön perustuvaa hashia. Tämä mahdollistaa tiedostojen tallentamisen välimuistiin pitkäksi aikaa, mutta tiedostojen muuttuessa koko sovellus pitää päivittää eikä itse HTML:ää voi tallentaa välimuistiin pitkäksi aikaa. Eräs mahdollisuus on käyttää manifesti-tiedostoa, jossa määritetään sivuston käyttämät resurssit. Näin manifestille voidaan asettaa lyhyt tallennusaika ja se voidaan päivittää erikseen, muusta sivustosta riippumatta. Manifesti voi olla esimerkiksi:

    module.exports = {
      baseUrl : 'https://varmais.fi/static/',
      resources : {
        vendor : 'vendor-asd241asd.js',
        app : ‘app-lkhdf873.js'
      }
    };

### HTTP/2
HTTP/2:n tapauksessa moduulit voidaan ladata erikseen käyttäen samaa yhteyttä, joten moduulien yhdistäminen samaan tiedostoon ei ole pakollista. Vaikka tiedostot myös pakataan erikseen, ei menetetty hyöty ole merkittävä. (jQuery, underscore ja Backbone yhdessä vs. erikseen - ero alle kilotavun luokkaa.) Todennäköisesti jonkinlaista tiedostojen yhdistelyä on kuitenkin tarpeellista harrastaa, joten oletaan että tulevaisuudessakin moduulit yhdistetään sen perusteella kuinka todennäköisesti ne muuttuvat.

HTTP/2-palvelin voi päätellä, että selaimen pyytäessä manifestia selain haluaisi myös siinä mainitut tiedostot ja alkaa työntää niitä selaimelle ilman erillistä pyyntöä. Jos selain on jo tallentanut tiedostot välimuistiin, voi se estää latauksen jatkumisen, mutta silti kaistaa menee hukkaan tässä tapauksessa. Periaatteessa palvelin voi käyttää sessio-tietoja päättelyyn, mutta tämä tekisi pyynnöistä tilariippuvaisia ja monista CDN:stä todella surullisia. Jos taas palvelin osaisi lukea manifestista työnnettävät tiedostot, voisi manifesti olla pseudotasolla seuraavanlainen:

    module.exports = {
      baseUrl: 'http://varmais.fi/static/',
      resources: {
        vendor: {
          file: 'vendor-asd241asd.js',
          push: true
        },
        app: {
          file: 'app-lkhdf873.js',
          push: true
        },
        widget: {
          file: 'widget-e43b8ad12f.js',
          push: false
        }
      }
    };

### Näistä meidän pitää vielä jutella
Kun HTTP/2:a ja sen edeltäjää [SPDY:tä](https://en.wikipedia.org/wiki/SPDY) on testattu, on havaittu satunnaista pakettien häviämistä. Tämä johtuu siitä että virustorjunta-ohjelmilla ja palomuureilla on tiettyjä oletuksia HTTP-protokollan toiminnasta, eikä esimerkiksi tiedostojen lähettäminen koneelle pyytämättä kuulu niihin. Osittain tästä syystä HTTP/2 on mahdollista vain salatun yhteyden yli. Toinen syy on se tosiseikka, että palvelimen ja selaimen tulee erikseen sopia HTTP/2:n käytöstä ja TLS-kättely on siihen erinomainen mahdollisuus.

Vanhojen selainten tukeminen tulee aiheuttamaan myös HTTP/2:n tapauksessa harmaita hiuksia. Venyykö esimerkin manifesti tupla-pitkäksi ja tuleeko koko build-prosessi tuplata kun luodaan optimaalinen tapa ladata sivu sekä vanhalla että uudella tavalla?

### Todellisuudessa ollaan vielä kaukana
CDN-palveluntarjoajien luulisi olevan ensimmäisten joukossa kiinnostuneita HTTP/2-tuen tarjoamisesta, mutta tietääkseni yksikään isoimmista toimijoista ei ole luvannut päivämäärää HTTP/2:n käyttämiselle. Push-ominaisuuteen liittyvät kysymykset tulee myös ratkaista: pitäisikö selaimen kertoa esimerkiksi headerillä jos se haluaa ottaa tiedostoja vastaan kysymättä?

Web-kehittäjiä kiinnostaa tietenkin käytettävät työkalut ja niiden tilanne. Tällä hetkellä vain Firefoxin kehitystyökalut osaavat eritellä palvelimen työntämät tiedostot. Miten sitten esimerkiksi Webpackiin määritetään erikseen HTTP/1 ja HTTP/2 -prosessit? Kehitysympäristön pystytyskään ei ole vielä aivan triviaalia, ja protokollan spesifikaatio ja toteutukset elävät. 

On hyvä olla tietoinen tulevasta ja opiskella tulevien teknologioiden vaatimuksia, varsinkin kun kyseessä on niin käänteentekeävästä asiasta kuten HTTP/2. Mitään hätää ei kuitenkaan vielä ole, sillä esimerkiksi [IPv6 on ollut tulossa jo kaksi vuosikymmentä](https://en.wikipedia.org/wiki/IPv6#Working-group_proposals) ja vasta hiljattain IPv6-liikenne muodosti ensi kertaa [noin 10 % internet-liikenteestä](http://www.google.com/intl/en/ipv6/statistics.html). HTTP/2:n osalta paljon on vielä TBD ja työkalut ovat vielä vaiheessa, mutta mielenkiinnolla odotetaan mitä tulevaisuus tuo tullessaan.

Luettavaa:
https://github.com/h2o/h2o
https://www.npmjs.com/package/http2-push-manifest
http://blog.kazuhooku.com/2015/12/optimizing-performance-of-multi-tiered.html
https://www.cloudflare.com/http2/
https://code.google.com/p/chromium/issues/detail?id=464501
