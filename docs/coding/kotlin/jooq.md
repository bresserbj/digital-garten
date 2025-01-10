---
title: Joop
date: 20250110
author: bresserbj
---

## Was ist jOOQ?

jOOQ bietet eine typesichere, DSL-aehnliche API, mit der SQL-Abfragen direkt umgesetzt werden koennen.
Anders als andere ORM-Frameworks wie zB Hibernate, setzt es hier darauf SQL auf eine praegnaten und saubere 
Weise direkt in den Code zu integrieren, anstatt SQL zu abstrahieren.

### Vorteile

1. **Typesicherheit**:
Jede Abfrage wird kompiliert, sodass Fehler wie falsche Spaltennamen oder ungueltige Datentypen waehrend der Kompilierung
erkannt werden.


2. **SQL-Unterstuetzung:**
Unterstuetzt die meisten SQL-Dialekte und erlaubt SQL nativ zu schreiben.


3. **Erzeugung von Code:**
Generiert Klassen fuer Tabellen, Spalten und anderen Elemente. DAdurch kann das Objekt direkt verwendet werden, anstatt
strings fuer die SQL-Abfragen zu schreiben.

   
4. **Flexibilitaet:** 
Man muss sich sich nicht an ORM-Konventionen wie Entities und LazyLoading halten.


5. **Leichte Integration:**
Funktioniert mit bestehenden JDBC-Treibern und mit anderen Frameworks wie SpringBoot.

### Nachteile

1. **SQL-Lastig:**
Erfordert SQL-Kenntnisse und direkte SQL-Integration in den Code.


2. **Kein automatisches Caching:** 
Benoetigt man ein Caching muss dieses selbst implementiert oder durch ein separates Framework wie Redis eingefuehrt werden.
Im Vergleich: Hibernate bietet ein integriertes First- und Second-Level-Caching an.


3. **Codegenerierung:**
Die benoetigten Klassen muessen kompiliert werden und ist damit nicht im NoSQL-Bereich geeginet oder wenn dynamische
Datenbankstrukturen notwendig sind.


4. **Potenzielle SQL-Injektions:**
Auch wenn qOOQ typsicher ist, kann man durch die Implementierung roher SQL-Abfragen das Risiko fuer SQL-Injektions
erhoehen - hier ist vorsicht bei der Entwicklung geboten.

## Wann setzte ich jOOQ ein?

Es ist ideal wenn:

- SQL direkt und typesicher eingesetet werden soll
- man komplexe und massgeschneiderte Abfragen benoetigt
- man die kontrolle ueber die Datenbank-Interaktionen behalten moechte
 
Es ist nicht ideal wenn:

- man nur einfache CRUD-Operationen benoetigt
- Caching oder ORM-Funktionen wie LazyLoading benoetigt werden
- eine dynamische DAtenbankstruktur vorliegt

## Wie funktioniert jOOQ?

jOOQ generiert die Klassen (z.B. fuer Tabellen, Views, Sequenzen) basierend auf der Datenbankstruktur. Dieser Prozess
funktioniert wie folgt:

1. Verbindung zur Datenbank
2. Analyse der Struktur (Tabellen, Spalten, Typen, Constraints)
3. Codegenerierung basierend auf den Metadaten, darunter Tabellen- und Record-Klassen
4. Speicherung in `target/generated-sources/jooq`

### Was wird fuer die Generierung benoetigt?

1. Maven-Plugin
2. Datenbankverbindung - Details wie JDBC-Url, Benutzername, Passwort
3. Generator-Konfiguration - Welche Datenbank wird verwendet, Ausschluss von Tabellen und Schemas, Zielort

### Beispiel:

Folgende Tabelle wurde erzeugt:

```sql
CREATE TABLE users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

#### jOOQ generiert

**Tabellenklasse:Users**
```kotlin
package com.example.jooqdemo.generated.tables

import org.jooq.impl.TableImpl
import org.jooq.TableField
import org.jooq.impl.DSL
import com.example.jooqdemo.generated.tables.records.UserRecord

object Users : TableImpl<UserRecord>("users") {
    val ID: TableField<UserRecord, Long> = createField("id", DSL.bigint(), this)
    val NAME: TableField<UserRecord, String> = createField("name", DSL.varchar(100), this)
    val EMAIL: TableField<UserRecord, String> = createField("email", DSL.varchar(100), this)
}
```

**POJO:User**
```kotlin
package com.example.jooqdemo.generated.tables.pojos

data class User(
    var id: Long? = null,
    var name: String? = null,
    var email: String? = null
)
```

**Record:UserRecord**
```kotlin
package com.example.jooqdemo.generated.tables.records

import org.jooq.impl.UpdatableRecordImpl

class UserRecord : UpdatableRecordImpl<UserRecord>(Users) {
    var id: Long? = null
    var name: String? = null
    var email: String? = null
}
```

#### Nutzung

```kotlin
package com.example.jooqdemo.config

import org.jooq.DSLContext
import org.jooq.impl.DSL
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration
import javax.sql.DataSource

@Configuration
class JooqConfig(private val dataSource: DataSource) {

    @Bean
    fun dslContext(): DSLContext {
        return DSL.using(dataSource.connection)
    }
}
```

```kotlin
package com.example.jooqdemo.repository

import com.example.jooqdemo.generated.tables.Users
import com.example.jooqdemo.generated.tables.Orders
import com.example.jooqdemo.generated.tables.pojos.User
import org.jooq.DSLContext
import org.springframework.stereotype.Repository

@Repository
class UserRepository(private val dsl: DSLContext) {

    // INSERT: Benutzer hinzufügen
    fun addUser(name: String, email: String): Int {
        return dsl.insertInto(Users.USERS)
            .columns(Users.USERS.NAME, Users.USERS.EMAIL)
            .values(name, email)
            .execute()
    }

    // UPDATE: Benutzer-E-Mail aktualisieren
    fun updateUserEmail(userId: Long, newEmail: String): Int {
        return dsl.update(Users.USERS)
            .set(Users.USERS.EMAIL, newEmail)
            .where(Users.USERS.ID.eq(userId))
            .execute()
    }

    // DELETE: Benutzer löschen
    fun deleteUserById(userId: Long): Int {
        return dsl.deleteFrom(Users.USERS)
            .where(Users.USERS.ID.eq(userId))
            .execute()
    }

    // JOIN: Benutzer mit Bestellungen abrufen
    fun findUsersWithOrders(): List<Pair<User, Int>> {
        return dsl.select(Users.USERS.asterisk(), Orders.ORDERS.ID.count())
            .from(Users.USERS)
            .join(Orders.ORDERS).on(Users.USERS.ID.eq(Orders.ORDERS.USER_ID))
            .groupBy(Users.USERS.ID)
            .fetch { record -> 
                val user = record.into(User::class.java)
                val orderCount = record.get(Orders.ORDERS.ID.count())
                user to orderCount
            }
    }

    // Raw SQL: Manuelle SQL-Abfrage
    fun rawQuery(): List<User> {
        return dsl.fetch("SELECT * FROM users WHERE email LIKE ?", "%example.com%")
            .into(User::class.java) // Mappe das Ergebnis auf das `User`-POJO
    }
}
```

```kotlin
package com.example.jooqdemo.service

import com.example.jooqdemo.repository.UserRepository
import com.example.jooqdemo.generated.tables.pojos.User
import org.springframework.stereotype.Service

@Service
class UserService(private val userRepository: UserRepository) {

    fun addUser(name: String, email: String): Int {
        return userRepository.addUser(name, email)
    }

    fun updateUserEmail(userId: Long, newEmail: String): Int {
        return userRepository.updateUserEmail(userId, newEmail)
    }

    fun deleteUserById(userId: Long): Int {
        return userRepository.deleteUserById(userId)
    }

    fun findUsersWithOrders(): List<Pair<User, Int>> {
        return userRepository.findUsersWithOrders()
    }

    fun rawQuery(): List<User> {
        return userRepository.rawQuery()
    }
}

```