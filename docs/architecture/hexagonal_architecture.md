---
title: Hexagonal Architecture
date: 20250108
author: bresserbj
---

## Was ist eine Hexagonale Architektur?

Eine Hexagonale Architektur, oder auch "Ports und Adapter" genannt, zeichnet sich laut des Autors Alistair Cockburn
durch nachfolgende drei Ziele aus:

1. Es soll keinen Unterschied machen ob die Businesslogik von einer UI, einer API oder einem Test-Framework aufgerufen wird.
2. Die Businesslogik soll isoliert von der Datenbank und sonstiger Infrastruktur oder Drittsystemene entwickelt und
   getestet werden koennen. 
3. Aenderungen an externen Schnittstellen oder Modernisierungen der Infrastruktur sollen ohne Anpassungen an der 
   Businesslogik moeglich sein.

## Ports und Adapter

Erreicht werden diese drei Ziele durch die Isolation der Businesslogik ueber sogenannte "Ports" und "Adapter".

Die Businesslogik liegt im Kern der Anwendung und definiert "Ports" um von ausserhalb gesteuert zu werden und um zu steuern.
Wichtig ist es, dass es der Businesslogik irrelevant ist, welche technischen Details sich hinter den Ports befinden.
Alle Komponenten ausserhalb des Kerns sind gegen die Ports zu implementieren.

Die Anbindung der externen Komponente erfolgt durch "Adapter".

Applikation --> Port --> Database Adapter --> Database

An einem Port koennen mehrere Adapter angeschlossen werden. So kann zB. an einen Port zur Steuerung einer Anwendung 
ein Adapter fuer die UI und ein Adapter fuer eine API angeschlossen sein.

## Steuernde und gesteuerte Ports

Es gibt zwei Arten von Ports nd Adaptern - steuernde und gesteuerte.
Steuernde Ports und Adapter werden auch als "driving" oder "primary" bezeichnet.
Gesteuerte Ports werden als "driven" oder "secondary" bezeichnet.

### Dependency Rule

Um die technischen Details und Anbindungen zur Anwendung abzutrennen koenne nwir die sogenannte "Dependency Rule" nutzen.
Diese besagt, dass Abhaengigkeiten ausschliesslich von aussen nach innen, also in Richtung der Anwendung zeigen duerfen.



Adapter:
```kotlin
package com.bank.transfer.infrastructure.adapter.driver

import java.math.BigDecimal
import java.util.UUID

class TransferController(
   private val transferMoneyPort: TransferMoney
) {

   fun transfer(amount: BigDecimal, from: UUID, to: UUID): TransferMoneyRespone {
      return transferMoneyPort.transfer(TransferMoneyRequest(amount, from, to))
   }
}
```
Port:
```kotlin
package com.bank.transfer.app.port.driver

import java.math.BigDecimal
import java.util.UUID

interface TransferMoney {
   fun transfer(request: TransferMoneyRequest): TransferMoneyRespone
}

data class TransferMoneyRequest(
   val amount: BigDecimal,
   val from: UUID,
   val to: UUID
)

sealed class TransferMoneyResponse {
   object Success : TransferMoneyResponse()
   object InvalidAmount : TransferMoneyResponse()
}
```

Service:
```kotlin
package com.bank.transfers.app.service

class TransferService : TransferMoney {
   override fun transfer(request: TransferMoneyRequest): TransferMoneyRespone {
      TODO("Not yet implemented")
   }

}
```

### Dependency Inversion

Auch hier wird der Port durch ein Interface definiert. Allerdings sind die Beziehungen zwischen den Klassen vertauscht.
Der Adapter benutzt den Port nicht, sondern implementiert diesen und der Service implementiert den Port nicht sondern
benutzt ihn.

Port:
```kotlin
package com.bank.transfers.app.port.driven

interface AccountFinder {
    fun find(customerId: CustomerId): Account?
}
```

Adapter
```kotlin
package com.bank.transfers.infrastructure.adapter.driven

class InMemoryAccounts : AccountFinder {
    private val accounts = mutableMapOf<CustomerId, Account>()
   
   override fun find(customerId: CustomerId): Account? = accounts.get(customerId)
}
```

Services:
```kotlin
package com.bank.transfers.app.service

class AccountService(private val accountFinder: AccountFinder) {
   override fun find(customerId: CustomerId) {
      TODO("Not yet implemented")
   }
}
```

## Dilemma der Isolation

Die Isolierung der technischen Details fuehrt dazu, dass bei der Verwendung von ORM ein Dilemma darin besteht, dass der 
Kern der Anwendung die Details des Persistierenden Adapters nicht kennen soll, also die Entitaet. 

Hier gibt es einen Loesungsansatz indem man zum einen die Klasse der Entitaet im Adapter hat, die keine Businesslogik
beinhaltet, aber dafuer die technischen Annotationen fuer die Persistierung, und zum Anderen dem Kern der Anwendung eine
eigene Modelklasse implementiert.

Das gleiche Dilemma finden wir auch in API-Adaptern, die oft nicht alle Attribute einer Entitaet sichtbar machen sollen.



## Vorteile der Hexagonalen Architektur

- Aenderbarkeit - Die Businesslogik kann geaendert werden ohne die Adapter oder Infrastruktur aendern zu muessen und umgekehrt.
- Isolierung - Der Kern der Anwendung umfasst nur _fachliche_ Themen und _technische_ Themen sind in den Adaptern implementiert.
- Entwicklung - Entwicklung an Komponenten kann aufgeteilt werden.
- Testbarkeit - Komponenten koennen vollstaendig isoliert geteste werden.

## Nachteile der Hexagonalen Architektur

- Erheblicher Mehraufwand der sich fur CRUD-Microservices eher nicht lohnt.