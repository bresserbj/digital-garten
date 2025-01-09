---
title: Domain Driven Design
date: 20250108
author: bresserbj
---

Domain Driven Design (DDD) kann man grob in zwei Segmente von Tools unterteilen: strategisch und taktisch.
Sowohl die strategischen als auch taktischen Muster und Praktiken von DDD richten die Softwarearchitektur auf 
das Geschaeftsmodell (Business) aus.

Strategische Aspekte von DDD beantworten die Fragen des "Was?" und "Wieso?", waehrend die taktischen Aspekte das
"Wie?" beschreiben.

**Strategisch** - 
Die strategischen Tools von DDD werden benutzt um die Business Domains und Strategie des Unternehmens zu identifizieren.
Auf Basis des Wissens der Business Domains kann anschliessend eine "high level" design Entscheidung getroffen werden,
wie man die Systeme in einzelne Komponenten zerlegt und daraufhin Integrationsmuster definiert.

**Taktisch** - 
Die taktischen Werkzeuge von DDD befassen sich mit einem anderen Aspekt der Probleme. Mit diesen Mustern kann Code auf 
eine Art und Weise geschrieben werden, der die Anwendungsdomaine widerspiegelt, ihre Ziele anspricht und die Sprache des
Unternehmens spricht.

------

## Strategisch

Um eine effiziente Loesung du entwickeln muss man zuerst das Problem verstanden haben. In diesem Fall ist das Problem
die Software, die es zu bauen gilt.

Unternehmen arbeiten in Geschaeftsfeldern (Business Domain). 
Ein Geschäftsfeld (Business Domain) definiert den Haupttätigkeitsbereich eines Unternehmens.
Es ist die Dienstleistung, die das Unternehmen für seine Kunden erbringt.

Um die Ziele seines Geschaefsfelds zu erfuellen ist es fuer Unternehmen notwendig seine Aktivitaeten in Subdomains
aufzuteilen. Alle Subdomains gemeinsam formen die Business Domain.

### Subdomains

Eine Subdomain wird in drei Typen unterteilt: Core, Generic und Supporting
Subdomains sind hierbei wie Karten eines Kartenhauses zu sehen: Nimmt man eine Karte weg faellt die Struktur in sich zusammen.

- Core Subdomains spiegeln den kompetativen Vorteil des Unternehmens in seiner Branche wieder.
- Generic Subdomains, bieten keinen kompetativen Vorteil und koennen auch von anderen Unternehmen so genutzt werden.
- Supporting Subdomains bieten keinen kompetativen Vorteil, haben aber eine niedrige Einstiegshuerde.

Um zwischen diesen Typen unterscheiden zu koennen macht es Sinn die nachfolgende Tabelle zu beruecksichtigen:

| Subdomain type | Competitive advantage | Complexity | Volatility | Implementation    | Problem     |
|----------------|-----------------------|------------|----------|-------------------|-------------|
| Core           | Yes                   | High       | High     | Inhouse           | Interesting |
| Generic        | No                    | High       | Low      | Buy               | Solved      |
| Supporting     | No                    | Low        | Low      | Inhouse/Outsource | Obvious     |


### Domain Knowledge 

Um effiziente Software zu entwickeln, die die Beduerfnisse des Businesses abdeckt, ist es wichtig das Grundwissen ueber 
die Business Domain zu erlangen. Dafuer ist es wichtig die Domain Experten zu verstehen, vor Allem wie diese ueber das 
Problem denken. Ohne dieses Verstaendnis und die Gruende hinter den Anforderungen ist es nicht moeglich alle Aspekte 
des Business abzudecken. 

Der Schluessel hierzu liegt in der Kommunikation und der Schaffung einer gemeinsamen Sprache die ein gemeinsames 
Verstaendnis bildet, ohne "Ubersetzungen" weiterer Parteien - dies wuerde nur dazu fuehren das falsche Loesungen 
implementiert werden oder die richtigen Loesungen zu den falschen Problemen.

### Ubiquitous Language

Eine allgegenwaertige Sprache ist der Grundpfeiler von Domain Driven Design und laesst sich leicht so erklaeren:

> Wenn Parteien effizient miteinander kommunizieren sollen, muessen diese die gleiche Sprache sprechen.

Die Sprache respresentiert in diesem Fall die Business Domain als auch das mentale Model des Domain Experten. Sie ist 
vor Allem die Sprache des Businesses und sollte entsprechend nur Begriffe umfassen die in Verbindung mit der Business
Domain stehen - kein technischer Jargon!

Auch ist bei der allgegenwaertigen Sprache auf konsistenz zu achten. Sie soll praezise sein und jeder Begriff soll nur
eine Bedeutung haben; sie koenne nicht austauschbar verwendet werden. Jeder Begriff ist ausdruecklich in seinem 
spezifischem Zusammenhang zu verwenden.

**Beispiel:**

"Verkaufsprovisionen werden nach Genehmigung der Transaktion verbucht."

nicht

"Die Verkaufsprovisionen basieren auf korrelierten Datensaetzen aus den Tabellen der Transaktionen und der genehmigten Verkaeufe."

### Model der Business Domain

> A model is a simplified representation of a thing or phenomenon that intentionally emphasizes certain aspects while 
> ignoring others. Abstraction with a specific use in mind.
> 
> -- <cite>Rebecca Wirfs-Brock</cite>

Alle Modelle haben einen Zweck, der nur die Details enthaelt, die zur Erfuellung dieses Zwecks erforderlich sind.
Ein Model soll hierbei ein Problem loesen, mit grade mal genug Informationen fuer diesen Zweck. 

Die allgegenwaertige Sprache die wir verwenden ist nicht detailiert genug um die Domaine abzudecken. Das ist der Zweck
des Modells, das gerade genug Aspekte der Business Domain enthalten soll, um eine Implementierung des erforderlichen 
Systems zu ermoeglichen.

#### Tools:

Ein Tool um die Business Domain besser zu verstehen ist ein **Glossar**, wobei dieses vorrangig fuer Nomen verwendet werden 
kann und damit teilweise nicht die noetige Detailtiefe beinhaltet die man benoetigt.

Ein anderes Tool sind **Gherkin Tests**. Tests geschrieben in Gherkin sind ein grossartiges Tool um die Sprache der 
Business Domain abzubilden und um gleichzeitig die Luecken zwischen dem Domain Experten und den Entwicklern zu schliessen.

**Beispiel:**

```feature
Scenario: Notify the agent about a new support case
    Given Vicent Jules submits a new support case says:
    """
    I need help configuring AWS Infinidash
    """
    When the ticket is assigned to Mr. Wolf
    Then the agent receives a notification about the new ticket
```

### Bounded Contexts

Die allgegenwaertige Sprache soll konsistent, eineindeutig und praezise sein. Es kann allerdings vorkommen, dass dies 
nicht direkt moeglich ist, da zB mehrere Subdomains die gleichen Begrifflichkeiten anders nutzen. Hier gaebe es die 
Moeglichkeit diese Begriffe zB ueber einen Prefix zu differenzieren und entsprechend in der Implementierung zwei Modelle 
zu nutzen, was allerdings zu kognitivem Overload fuehren wird. 

An dieser Stelle kommt der "Bounded Context" ins Spiel der sich dieser Problematik annimmt. Dieser teilt die definierte
allgegenwaertige Sprache in mehrere kleinere Sprachen auf, die in ihrem Kontext eineindeutig werden.
Es werden der allgegenwaertigen Sprache also Grenzen gesteckt welche ebenfalls auf die Models angewandt werden.

Also kann man sagen, dass die allgegenwaertige Sprache nocht allgegenwaertig ist und nicht universel. Erst durch die 
Nutzung dieser Sprache in Verbindung mit einem Bounded Context wird dies erfuellt werden.

### Physical Boundaries

Der Bounded Context fungiert hier nicht nur als Grenze fuer die Models, sondern kann auch physisch genutzt werden.
Jeder definierte Bounded Context sollte also als eigener Service oder eigenes Projekt implementiert werden.
Somit kann jedem Bounded Context in Bezug auf die Implementierung der Techstack zugeordnet werden, welchen er benoetigt.



------

## Taktisch

