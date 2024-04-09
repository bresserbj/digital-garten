---
title: SOLID
date: 20240409
author: bresserbj
---

### SOLID

Alle Prinzipien von SOLID sind "Entwurfsprinzipien"

Qualitätsmerkamel von SOLID:
* Einfacher Analysierbar / Lesbar
* Flexibel
* Wartbar

Ein Entwurfsprinzip ist ein Prinzip, dass immer und ständig bei einem Entwurf von Designs / Architektur berücksichtigt werden soll.

#### Single Responsibility Priciples (SRP)

- Ein beliebiges System (Methode, Klasse, Schicht) hat nur eine einzige Verantwortung/Aufgabe
- Vorteile: kurz, knapp, übersichtlich, wenig komplex -> einfach wartbar, lesbar

Hilft durch das "richtige" schneiden von Bestandteilen der Software diese besser analysierbar, erweiterbar und damit austauschbar zu machen.

#### Open Closed Principle (OCP)

- Offen für Erweiterungen
- Geschlossen für Veränderungen
- Es werden Erweiterungspunkte geschaffen, sodass Quellcode nicht groß geändert werden muss.

Sorgt dafür, dass die Anwendung erweiterbarer, modifizierbarer und so flexibler an die Projektsituation angepasst werden kann

#### Liskov Subtitution Priciple (LSP)

- Definiert einen Implementierungs-Kontrakt
- Subklassen müssen an die Stelle der Basisklasse treten können

Stellt sicher, dass hinter einem kontrakt eine beliebige Implementierung genutzt werden kann, ohne dass der Aufrufer dies bemerkt.

#### Interface Segregation Pricinple (ISP)

- Regelt die Granularität der Kontrakte
- Kontrakte immer für einzelne Konsumenten optimieren

#### Dependency Inversion Pricinple (DIP)

- Abhängigkeiten belasten alle relevanten Qualitätsattribute

Sorgt dafür, dass in einer Anwendung keine Abhängigkeiten gegen Implementierung bestehen, sondern nur gegen Kontrakte
