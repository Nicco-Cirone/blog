---
title: "Le strade piu' pericolose di Roma: La mappa e i dati."
date: 2016-10-01 13:39:38
layout: post
categories: [ Data Visualization, Tableau, Italy ]
---

Segnaletica inesistente, intersezioni non regolate, luminosita' insufficiente, buche.

Tra le molte cause degli incidenti stradali, alcune sono legate ad incuria, o mancanza di investimenti. D'altra parte, e' inevitabile che una citta' di oltre 4 milioni di abitanti e 1,285 km2 di estensione, peraltro martoriata dai buchi di bilancio, affronti difficolta' nella gestione della spesa pubblica.

Perche' non farsi aiutare dai dati, dunque, in un'ottica di *smart city*?

[Il sito di open data del Comune di Roma](http://dati.comune.roma.it/) mette a disposizione i dati granulari su tutti gli incidenti stradali, trimestre per trimestre. Usando Tableau, una simile banca dati permette di costruire strumenti come il seguente, che possano aiutare i *decision makers* a capire quali quadranti del proprio territorio abbiano maggior bisogno di interventi, e di che tipo.

 

[caption id="attachment\_188" align="alignnone" width="1096"][![le-strade-piu-pericolose-di-roma]({{ site.baseurl }}/assets/uploads/le-strade-piu-pericolose-di-roma.webp)](https://public.tableau.com/shared/S948S7HB4?:display_count=yes) [Clicca per andare alla dashboard interattiva](https://public.tableau.com/shared/S948S7HB4?:display_count=yes)[/caption]



La mappa riporta tutti gli incidenti stradali registrati a Roma nel 2015, ed e' colorata in base alle specifiche del contesto, evidenziando le situazioni problematiche. In questo modo e' possibile individuare, municipio per municipio, le aree che maggiormente necessitano di segnaletica, illuminazione, o interventi di assestamento.

![incidenti-2]({{ site.baseurl }}/assets/uploads/incidenti-2.webp)

 

Sulla via Ostiense, ad esempio, circa 20 persone sono rimaste ferite in incidenti in condizioni di luminosita' inesistente.

![ostiense]({{ site.baseurl }}/assets/uploads/ostiense.webp)

Il quinto municipio, d'altro canto, ha un chiaro problema di segnaletica assente: filtrando la mappa per gli incidenti laddove la presenza di segnaletica ha maggior impatto (principalmente gli scontri in marcia, spesso dovuti a incroci mal segnalagti), si avverte un problema difuso, che nel solo 2015 ha causato piu' di 40 tra morti e feriti.

![segnaletica-v-municipio]({{ site.baseurl }}/assets/uploads/segnaletica-v-municipio.webp)

Allo stesso modo, filtrando i dati per i soli incidenti che hanno coinvolto ciclomotori o motocicli, su strade con buche o dissestate, ad aver bisogno di assestamenti e' senza soprattutto il I Municipio.

![buche-i-municipio]({{ site.baseurl }}/assets/uploads/buche-i-municipio.webp)

L'obiettivo di questo post e' solo offrire un paio di esempi su come strumenti di visualizzazione dati come questo possano guidare lo sviluppo di *data driven administrations*, basate su una gestione dei soldi pubblici orientata alla massimizzazione dei risultati/benefici.
