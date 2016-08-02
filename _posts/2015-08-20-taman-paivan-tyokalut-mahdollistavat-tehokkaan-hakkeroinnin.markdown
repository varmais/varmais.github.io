---
layout: post
author: 'Teemu Tiilikainen'
title: Tämän päivän työkalut mahdollistavat tehokkaan hakkeroinnin
date: 2015-08-20 00:02:00.000000000 +03:00
---
Elämme mahtavia aikoja, kun meille on tarjolla niin monia laadukkaita työkaluja, jotka tekevät hakkeroinnista helpompaa kuin ikinä. Pari viikkoa takaperin eräs opiskelijatoveri (Hänen [Github-profiilista](https://github.com/lauri-kaariainen) löytyy mm. kiva multitouch tracker mobiiliselaimille.) kirjoitti Javascript-soveulluksen, joka parsi ja esitti Alkon tuotteet luettavana listana. Alko tarjoaa tuotteet sivuillaan CSV-tiedostona ([Alko hinnasto tekstitiedostona](http://www.alko.fi/PageFiles/5634/fi/Alkon%20hinnasto%20tekstitiedostona.txt)) ja ajattelin, että on parempi parsia data backendissä ja tarjota se JSON:ina fronttiin.

Kirjoitin pikaisesti [NodeJS-parserin](https://github.com/varmais/alko/blob/master/utils.js) ja JSON API:n. Aluksi käytin vanilla Javascriptiä frontissa, mutta huomasin nopeasti että muutamalla rivillä AngularJS + Bootstrap -magiaa voin luoda taulukon, paginaation (3000+ DOM-elementin muokkaaminen on aika raskasta selaimelle.), filtteröinnin ja modal-toiminnon jokaiselle tuotteelle.

JSON:ista parsitut objektit cachetetään käyttäjän localStorageen viikoksi, jonka ansiosta sivuston uudelleen käytön tulisi olla nopeampaa hitaan ensimmäisen sivulatauksen jälkeen.

Sovellus esillä testaamista varten Herokussa: http://alko.herokuapp.com/ ja koodit Githubissa: https://github.com/varmais/alko
