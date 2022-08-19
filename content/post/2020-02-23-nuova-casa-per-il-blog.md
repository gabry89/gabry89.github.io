+++
title = "Nuova casa per il blog"
date = 2020-02-23T14:22:51Z
description = "Ho trasferito il blog su una nuova infrastruttura per ottimizzare i costi"
draft = false
cover = "/images/plattform-hetzner-cloud.svg"
slug = "nuova-casa-per-il-blog"
summary = "Ho trasferito il blog su una nuova infrastruttura per ottimizzare i costi"
+++


In una sera di noia, ho trasferito questo blog e il dominio su una nuova infrastruttura per ottimizzare la gestione e i costi.

Come avevo scritto tempo fa, la macchina che ospitava il blog era una piccola EC2 t2.micro su AWS, mentre il dominio era registrato su Route 53.

Ben presto mi sono reso conto che AWS non è per nulla economico e i costi sono anche imprevendibili, perché ogni risorsa viene calcolata in modo preciso in base all'uso. Se si ha tanto traffico, si paga di più perché le risorse computazionali e di data transfer sono superiori. A questi livelli il delta è di un paio di euro al massimo, ma è comunque fastidioso per uno spazio di cazzeggio personale.

Con il setup su AWS, questo blog mi costava circa 14€ al mese, sommando EC2, transfer e zona DNS di Route53.

{{< figure src="/images/2020-02-23-14_49_48-Billing-Management-Console.png" >}}

Ho mantenuto questa situazione per tanto tempo, a causa della mia inestimabile pigrizia. Il mio obiettivo principale era quello di migrare Ghost ad un CMS statico, ma dopo mesi di ricerca non sono ancora riuscito a trovare un setup pratico che mi abbia convinto ad abbandonare Ghost.

Preso in un momento di buona produttività, ho abbandonato temporaneamente l'idea di staticizzare il blog e mi sono messo d'impegno almeno per migrarlo ad un servizio più economico: ho creato su [Hetzner](https://www.hetzner.com/cloud) un VPS, in pochi minuti ho tirato su un Ghost pulito, import del backup in JSON dal vecchio blog, import della cartella dei media, cambio dei DNS e in poco più di un'oretta avevo il blog up & running su una nuova macchina, il cui costo è di 3.65€ al mese.

{{< figure src="/images/2020-02-23-14_51_51-Microsoft-Store.png" >}}

Il passaggio successivo è stata la migrazione del dominio, per smettere di pagare 0.50$ al mese di zona DNS di Route53.

Avendo un TLD poco comune, sono andato su [tld-list.com](https://tld-list.com) e ho ordinato per transfer e renew più economici.

{{< figure src="/images/2020-02-23-15_01_31-Cheapest-.tips-Domain-Registration--Renewal--Transfer-Prices-_-TLD-List.png" >}}

Detto fatto, anche qui in meno di mezz'ora ho effettuato il transfer del dominio da Route53 a Porkbun 🐷. (tra l'altro è davvero figo porkbun, esperienza ottima!)

Al termine, ho eliminato tutti i servizi attivi su AWS. 👋🏻

Next step: staticizzare il blog e azzerare i costi, ma per quello vedremo più avanti...



