---
title: Domain Driven Design
date: 20250108
author: bresserbj
---

## Domain Driven Design

Domain Driven Design (DDD) kann man grob in zwei Segmente von Tools unterteilen: strategisch und taktisch.
Sowohl die strategischen als auch taktischen Muster und Praktiken von DDD richten die Softwarearchitektur auf 
das Geschaeftsmodell (Business) aus.

Strategische Aspekte von DDD beantworten die Fragen des "Was?" und "Wieso?", waehrend die taktischen Aspekte das
"Wie?" beschreiben.

### Strategisch

Die strategischen Tools von DDD werden benutzt um die Business Domains und Strategie des Unternehmens zu identifizieren.
Auf Basis des Wissens der Business Domains kann anschliessend eine "high level" design Entscheidung getroffen werden,
wie man die Systeme in einzelne Komponenten zerlegt und daraufhin Integrationsmuster definiert.

### Taktisch

Die taktischen Werkzeuge von DDD befassen sich mit einem anderen Aspekt der Probleme. Mit diesen Mustern kann Code auf 
eine Art und Weise geschrieben werden, der die Anwendungsdomaine widerspiegelt, ihre Ziele anspricht und die Sprache des
Unternehmens spricht.

------

## Strategisch: Analyze von Business Domains

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

----

## Strategisch: Domain Knowledge 


