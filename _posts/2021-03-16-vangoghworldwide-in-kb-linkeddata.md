---
title: "Van Gogh in KB linked open data"
categories:
  - Linked data
tags:
  - linked data
  - Van Gogh  
---

Ik heb op je verzoek een beetje gespeeld op data.bibliotheken.nl om te kijken voor vangoghworldwide.org wat wij over Van Gogh hebben als linked data. Hieronder beschrijf ik wat ik gedaan heb. Het geeft ook handvatten om verder te kijken. Is dit nuttig zo?

Groet, René


Wb Van Gogh:  ik wilde eerst weten of Vincent van Gogh in de NTA staat. Ik gebruikte deze query (via data.bibliotheken.nl/sparql) om grof te zoeken op namen met 'Gogh' er in.

select * where { 
  ?s schema:mainEntityOfPage/schema:isPartOf <http://data.bibliotheken.nl/id/dataset/persons> . 
  ?s schema:name ?naam .
  ?naam bif:contains "Gogh" .
}

Dat gaf drie Vincents waarvan bij nadere beschouwing dit onze man is:
http://data.bibliotheken.nl/id/thes/p068472676

Lodview laat meteen als zien dat we 259 publicaties over hem hebben, via SPARQL ook:

select * where {
?s schema:about <http://data.bibliotheken.nl/id/thes/p068472676> 
}

Lodview kent 169 publicaties waarvan hij de auteur is, volgens SPARQL ook:

select * where {
?s schema:author <http://data.bibliotheken.nl/id/thes/p068472676> 
}

Lodview kent 157 publicaties waarvan hij contributor is, volgen SPARQL:

select * where {
?s schema:contributor <http://data.bibliotheken.nl/id/thes/p068472676> 
}

Ook zien we dat hij een DBNL-URI heeft, namelijk http://data.bibliotheken.nl/id/dbnla/gogh006.  Lodview vertelt dat Van Gogh bij DBNL bekend is als auteur van 4 bronnnen. Query als voorgaande.

Misschien hebben we meer van / over Van Gogh maar zijn deze werken niet volledig gethesaureerd? Hier een voorbeeld van het opzoeken van een publicatie genoemd op vangoghworldwide.org. Ik kies een willekeurige titel:

Titel: French paintings of the XIXth and XXth centuries : collection of Mr. J. de Jong of E,J. van Wisselingh & Co., Amsterdam, Holland 1957[unpaged]

Ik zou dit op deze wijze opzoeken:

select * where { 
# in de NBT:
?s foaf:isPrimaryTopicOf/void:inDataset <http://data.bibliotheken.nl/id/dataset/nbt> . 
# geef alle titels:
?s schema:name ?titel .
# beperkt tot titels waarin French en paintings voorkomen:
?titel bif:contains "'French' AND 'Paintings'" .
}

Ik denk dat dat inha een handige manier is om te zoeken. Er zou natuurlijk ook op een letterlijke titel gezocht kunnen worden
( met ?s schema:name "{letterlijke titel}") maar de kans is dan best groot dat dat toch geen match geeft (door bijvoorbeeld tikfouten, overtollige spaties, etc).

Belangrijk te weten dat bif:contains alleen werkt als er per term minsten 4 tekens zijn. bif:contains "'Van' AND 'Gogh'" gaat dus niet werken. Wat wel werkt is ?titel bif:contains "'Van Gogh'" .

Momenteel is er een probleem met diakritische tekens waardoor zoeken op bijvoorbeeld bif:contains "'René'" nu niet de gewenste resultaten levert (hier wordt aan gewerkt).

Als een query te veel resultaten geeft dan zijn er natuurlijk meer filters in te stellen. Ik geef een voorbeeld met de publicatiedatum (jaar):

select * where { 
?s foaf:isPrimaryTopicOf/void:inDataset <http://data.bibliotheken.nl/id/dataset/nbt> . 
?s schema:name ?titel .
?titel bif:contains "'Van Gogh'" .
?s schema:publication/schema:startDate "1992" .
}

Hier is van belang te weten dat het jaar nu nog als een string (dus "1992") is opgenomen. In een nieuwe versie van de NBT (waar we aan werken) zal het tzt expliciet als datumveld opgenomen worden.
