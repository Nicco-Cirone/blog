---
title: "Gli italiani odiano i loro trasporti pubblici: Un marimekko in Tableau 10"
date: 2016-10-15 15:20:02
layout: post
---

Siccome questa settimana, per lavoro, mi e' servito imparare come si produce un Marimekko Chart in Tableau 10 (istruzioni in fondo), ho deciso di applicare subito quello che ho imparato a un dataset interessante sul livello di soddisfazione dei cittadini europei riguardo i loro mezzi pubblici.

In questo modo ho anche prodotto [il mio primo contributo](https://twitter.com/VizWizBI/status/786625190876807168) al progetto social di data visualization [#MakeoverMonday](http://www.makeovermonday.co.uk/), coordinato dal mio coach e mentor [Andy Kriebel](https://twitter.com/VizWizBI?lang=en-gb) e da [Andy Cotgreave](https://twitter.com/acotgreave?lang=en-gb).

Premessa obbligatoria: [Il Marimekko non e' una Best Practice di Data Visualization](https://www.perceptualedge.com/example13.php), e si basa sull'assunto erroneo che piu' informazioni si codificano in un grafico, piu' questo sia informativo. Al contrario, un singolo grafico dovrebbe codificare un solo messaggio in modo tale che questo sia desumibile a una prima occhiata.

Nel grafico qui sotto la larghezza delle barre e' proporzionale alla popolazione della citta', mentre le barre stesse sono colorate in base alla percentuale di risposte per livello di soddisfazione. I colori delle barre sono facilmente identificabili anche da persone daltoniche, al contrario del piu' classico - ed escludente - verde/rosso.

Indovinate a quale paese appartengono le barre piu' gialle?

[caption id="attachment\_327" align="alignnone" width="1001"][![marimekko-dashboard]({{ site.baseurl }}/assets/uploads/marimekko-dashboard2.webp)](https://public.tableau.com/views/MakeoverMondayGoodJobItaly_0/MMMarimekko?:embed=y&:display_count=yes) Clicca per esplorare la dashboard[/caption]

 

Non e' cosi' difficile da indovinare, e la risposta si puo' trovare facilmente evidenziando le barre italiane:

[caption id="attachment\_329" align="alignnone" width="999"]![highlight]({{ site.baseurl }}/assets/uploads/highlight1.webp) L'highlighter e' una nuova funzione di Tableau 10[/caption]

Per ciascuna categoria di citta' - grandi, medie o piccole - le ultime citta' per soddisfazione relativa ai mezzi pubblici sono sempre Italiane.

Per la precisione, Roma e' la peggiore grande citta', Napoli la pegigiore tra le medie, e Palermo la peggiore tra le piccole.

Hanno un buon punteggio invece Torino, tra le medie, e Verona e Bologna tra le piccole.

In questo caso specifico il Marimekko chart puo' funzionare perche' il messaggio e' semplice, ma dato che - come detto - non e' best practice, mi sono ritrovato alla fine a fare una seconda dashboard, con gli stessi dati.

[caption id="attachment\_326" align="alignnone" width="1003"]![explorative-dashboard]({{ site.baseurl }}/assets/uploads/explorative-dashboard1.webp) Dashboard esplorativa in Italiano[/caption]

Ma se il Marimekko non e' Best Practice, allora perche' farlo?

In primo luogo, perche' dovevo. Sfortunatamente, i consulenti BCG adorano il Marimekko, e ne richiedono di complicatissimi e variopinti.

Inoltre, fare un Marimekko e' un'ottimo strumento per imparare ad usare le Table Calculations in Tableau, e ad essere sinceri ne avevo gia' fatto uno mesi fa proprio per questo motivo.

[caption id="attachment\_368" align="alignnone" width="1200"]![fishing-activity-in-the-uk-2014-2]({{ site.baseurl }}/assets/uploads/fishing-activity-in-the-uk-2014-2.webp) Marimekko in Tableau 9, dati sulla pesca in UK nel 2014[/caption]

Allora applicai [una tecnica scoperta dal Tableau Zen Master Joe Mako](http://public.tableau.com/profile/joe.mako#!/vizhome/Marimekko/Marimekko), che prevedeva una serie di Table Calculations orientate in diversi modi. E' stato molto divertente, e mi ha permesso di imparare meglio come usare Partitioning ed Addressing per orientare le Table Calculations.

Ora, con le nuove funzionalita' di Tableau 10, c'e' anche un nuovo metodo per farlo, scoperto da un altro Zen Master, Jonathan Drummey.

Per costruire il mio sui trasporti ho dovuto cambiare leggermente i calcoli, in quanto quelli originali prevedevano una sola misura da scomporre sia per il colore che per la larghezza delle barre, mentre a me ne servivano due diverse.

Ecco come appare la mia canvas alla fine:

![canvas]({{ site.baseurl }}/assets/uploads/canvas.webp)

Nota il sorting su City (Descending, e basato su un LOD), fondamentale per indirizzare correttamente la Table Calculation "Tot Measure per Width Dimension".

Qui sotto i calcoli che ho usato:

**% of Total:**sum([Answers])/total(sum([Answers]))

**Size:**{fixed [City] : avg([Population (2014 or 2013)])}

**Tot Measure per Width Dimension:**

IF first()==0 THEN
min({fixed [City] : avg([Population (2014 or 2013)])})
ELSEIF min([City])!=lookup(min([City]),1) THEN
PREVIOUS\_VALUE(0)+min({fixed [City] : avg([Population (2014 or 2013)])})
ELSE min({fixed [City] : avg([Population (2014 or 2013)])})
END

**Population per City:**{exclude [Rating] : avg([Population (2014 or 2013)])}

Per replicare il Marimekko in Tableau 10, dopo aver creato i calcoli riadattando le misure e le dimensioni necessarie, segui i passi seguenti, in ordine:
1. Trascina "% of Total" sulle Rows
2. Trascina "City" sul Detail
3. Sorta "City" descending usando "Population per City" (Sum)
4. Trascina "Rating" sul Color
5. Trascina "Tot Measure per Width Dimension" sulle Columns
6. Cambia i Marks in Bars
7. Trascina "Size" sulla Size e seleziona "Fixed" con alignment "Right"
8. Cambia la Table Calculation "% of Total" e seleziona compute using "Rating"
9. Cambia la Table Calculation "Tot Measure per Width Dimension" e seleziona compute using "City"


Il grafico dovrebbe essere anche filtrabile per entrambe le dimensioni, ma il filtro non sempre funziona quando ci sono molti elementi, come in questo caso.