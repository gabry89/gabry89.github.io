---
title: Perché DAZN consiglia Edge e Internet Explorer?
date: 2018-08-26T12:41:07.000Z
#description: L'account ufficiale DAZN consiglia di usare Edge e Internet Explorer per una migliore esperienza di visione. Bevuti il cervello? Non proprio.
draft: false
cover: 
    image: /images/dazn-og.jpg
slug: perche-dazn-consiglia-edge
summary: "L'account ufficiale DAZN consiglia di usare Edge e Internet Explorer per una migliore esperienza di visione. Bevuti il cervello? Non proprio."
---


L'altro giorno l'account ufficiale [@DAZN_IT](https://twitter.com/DAZN_IT) ha riportato una serie di consigli utili per migliorare la qualità dello streaming. Uno fra tutti, nello specifico il punto 4, ha fatto sbarellare tutti.

{{< figure src="/images/2018-08-25-23_52_41-DAZN-Italia--@DAZN_IT--_-Twitter.png" caption="Internet Explorer migliore di Chrome e Firefox? Questi sono pazzi." >}}

Ennesimo consiglio da tabaccai? Beh... in questo specifico caso, non proprio.

Vi sorprenderà sapere infatti che insieme a loro c'è anche un'altra realtà di streaming, ben più importante e famosa, la quale consiglia l'uso di Edge e Internet Explorer per la massima qualità. Ebbene sì, sto parlando di Netflix.

Se andate sul sito ufficiale di Netflix, nella sezione relativa ai [requisiti di sistema](https://help.netflix.com/it/node/23742) vengono riportate anche le risoluzioni massime raggiungibili con i vari browser. Indovinate un po'? Chrome e Firefox cappati a 720p, Internet Explorer a 1080p, mentre Edge addirittura raggiunge uno splendido 4K.

{{< figure src="/images/2018-08-26-00_04_33-Requisiti-di-sistema-di-Netflix-per-lettori-HTML5-e-Silverlight.png" >}}

Ora giustamente molti di voi saranno confusi, e lo capisco, perché tutti noi guardiamo regolarmente da YouTube e altre fonti video in 1080p (a volte anche 4K) senza il minimo problema con i browser più disparati.

Quindi... perché? Cosa impedisce a Netflix/DAZN di sparare un FullHD/4K anche su Chrome o Firefox? Qual è la differenza tra servizi come Netflix/DAZN e YouTube? Perché quest'ultimo ci riesce mentre un colosso come Netflix no?

I colpevoli, ancora una volta, sono i sistemi per la gestione dei diritti d'autore.

Netflix su Chrome e Firefox utilizza **Widevine DRM** con il criterio di licenza `SW_SECURE_DECODE`. Con questo criterio i frame vengono decodificati dalla libreria Widevine anziché direttamente dalla CPU/GPU. Di conseguenza, i frame vengono decodificati senza alcun tipo di accelerazione hardware, il che significa che nei computer di fascia più bassa si possono riscontrare dei frame drop durante la riproduzione. Una situazione paradossale dove all'aumentare della qualità dello streaming peggiora la qualità della visione. Un'esperienza decisamente ben lontana dagli standard desiderati da Netflix.

Su Internet Explorer/Edge in Windows 10 invece, Netflix ha la possibilità di spingere la risoluzione dello streaming alla massima possibile. Questi due browser infatti utilizzano **Playready** per lo streaming DRM, e poiché Playready è integrato direttamente in Windows 10, sfrutta l'accelerazione hardware del sistema permettendo una riproduzione snella e fluida anche ad elevate risoluzioni.

Tornando quindi alla domanda iniziale, perché DAZN consiglia Internet Explorer? Perché su Chrome potete vedere Netflix solo in 720p?

Per colpa dei DRM.

**#FuckDRM**



