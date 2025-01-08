---
title: Maven
date: 20250108
author: bresserbj
---

Maven ist ein Tool das zur Paketverwaltung in Java-Projekten eingesetzt werden kann um diese zu organisieren und auch
um Automatisierungen einzubauen. 

## Project Object Model - POM

Die Konfiguration eines Projektes, welches Maven einsetzt, wird mittels eines _Projekt Object Models_ (POM) realisiert.
Dieses PIM wird durch die _pom.xml_ im Projekt-Root abgebildet. 

Diese Datei beschreibt...

- das Projekt
- Abhaengigkeiten zu Paketen
- Konfigurationen von Plugins
- Relationen zu Modulen anderer Projekte

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>example</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>com.example</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
           //...
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
            //...
            </plugin>
        </plugins>
    </build>
</project>
```

### Dependencies

Abhaengigkeiten werden in der POM wie folgt definiert:

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.16</version>
</dependency>
```

### Properties

Um eine Verbesserung der Wartbarkeit innerhalb der POM zu gewaehrleisten koennen zB Versionsnummern in _properties_ 
ueberfuehrt werden um diese wieder zu verwenden. So muss man nur einmal die Versionsnummer anpassen:

```xml
<properties>
    <spring.version>5.3.16</spring.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>

```

## Lifecycle

- validate - Ueberprueft die Korrektheit des Projektes
- compile - Compiliert den SourceCode in ein Artefakt
- test - Ausfuehrung von Unittests
- package - Kompilierung des Codes
- integration-test - Tests, die auf den Paketen aufbauen
- verify - Validiert die Pakete
- install - Installiert die Pakete in das lokale Maven Repository
- deploy - Deployed die Pakete auf einen Remote Server oder in ein Repository

## Wichtige Befehle

Ein ausgefuehrter Befehl fuehrt auch immer die vorhergehenden notwendigen Punkte des Lifecycle aus.

| Befehl                   | Beschreibung                                                      |
|--------------------------|-------------------------------------------------------------------|
| mvn clean                | Cleans the project by deleting the target directory               |
| mvn compiler:compile     | Compiles the source classes                                       |
| mvn package              | Build the project and create the JAR/WAR files                    |
| mvn install              | Build the project and install package files to local repository   |
| mvn deploy               | Deploy the build artifact to the remote repository                |
| mvn validate             | Validate the project is correct als everything is available       |
| mvn dependency:tree      | Generates the dependency tree of the project                      |
| mvn dependency:analyze   | Analyze the project and identify the unused delcared dependencies |
| mvn test                 | Tests the source code                                             |
| mvn verify               | Run any checks on results of integration tests                    |
| mvn compile              | compile the source code of the project                            |
|                          |                                                                   |
