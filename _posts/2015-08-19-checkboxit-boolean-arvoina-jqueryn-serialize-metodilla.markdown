---
layout: post
author: 'Teemu Tiilikainen'
title: Checkboxit boolean-arvoina jQueryn serialize() -metodilla
date: 2015-08-19 23:53:54.000000000 +03:00
---
Joskus ohjelmistoprojektissa voi tulla tarpeen tietää checkbox-kentän arvo, vaikka käyttäjä ei olekaan valinnut kenttää aktiiviseksi. [HTML-speksin](http://www.w3.org/TR/html401/interact/forms.html#h-17.2.1) mukaan selain ei lähetä niiden checkbox-kenttien arvoja, joita käyttäjä ei ole valinnut. Joissain tilanteissa voi kuitenkin tulla tarpeen pitää esimerkiksi backend-koodi yksinkertaisena ja lähettää jokaisen kentän arvot booleaneina sen sijaan, että tarkistaisi jokaisen kentän arvon erikseen.

Jos projekti käyttää jQueryä, on sen [serialize-metodin](http://james.padolsey.com/jquery/#v=1.11.1&fn=serializeArray) ylikirjoitus tukemaan uutta “checkboxesAsBool” -asetusta triviaalia, ja tehdä se vieläpä niin ettei alkuperäiseen toimintaan muuten tehdä muutoksia.

Jos jos meillä on kyseinen formi:
```
<form id="form">
      <input id="box1" type="checkbox" name="box1" checked="checked" />
      <input id="box2" type="checkbox" name="box2" />
</form>
```
Oletuksena formi palauttaa serialisoituna seuraavaa:
```
$('#form').serialize()
=> "box1=on”
```
Mutta uudella asetuksella:
```
$('#form').serialize({checkboxesAsBools: true})
=> "box1=true&box2=false”
```
Koodi: https://github.com/varmais/jquery.checkboxes.js
