---
title: Schnellstart
---

Dieser Schnellstart ist für fortgeschrittene Entwickler gedacht. Für eine sanftere Einführung in Gatsby, [fange mit dem Tutorial an](/tutorial/)!

## Nutze die Gatsby-CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Quick Start with Gatsby: Create, Develop, and Build Gatsby Sites From the Command Line"
/>
Das Video stammt von [egghead.io](https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line).

**Anmerkung**: Dieses Video verwendet `npx`, um ein npm-Paket auszuführen, ohne es vorher zu installieren. Das Ausführen des Befehls `npx gatsby new` ist das gleiche wie das Ausführen von `gatsby new` nach der Installation der Gatsby-CLI auf deinem Computer.

### Installiere die Gatsby-CLI

```shell
npm install -g gatsby-cli
```

### Erstelle eine neue Seite

```shell
gatsby new gatsby-site
```

### Wechsle das Verzeichnis in deinen Seiten-Ordner

```shell
cd gatsby-site
```

### Starte den Entwicklungsserver

```shell
gatsby develop
```

Gatsby wird eine Hot-Reloading-Entwicklungsumgebung starten, die standardmäßig unter `http://localhost:8000` zugänglich ist.

Versuche, die JavaScript-Seiten in `src/pages` zu bearbeiten. Gespeicherte Änderungen werden im Browser live neu geladen.

### Erstelle eine auslieferungsfertige Version

```shell
gatsby build
```

Gatsby führt einen optimierten Produktiv-Build für deine Website durch und generiert statisches HTML und routenbezogene JavaScript-Code-Paket.


### Führe die auslieferungsfertige Version lokal aus

```shell
gatsby serve
```

Gatsby startet einen lokalen HTML-Server zum Testen deiner erstellten Webseite. Vergesse nicht, die Website mit `gatsby build` zu erstellen, bevor du diesen Befehl verwendest.

### Zugriff auf die Dokumentation für CLI-Befehle

Um eine detaillierte Dokumentation für die CLI-Befehle zu sehen, führe `gatsby --help` im Terminal aus.

Für bestimmte Befehle führe `gatsby COMMAND_NAME --help` aus, z.B. `gatsby new --help`.

Weitere Informationen über die Gatsby-CLI findest du im Abschnitt [CLI-Referenz](/docs/gatsby-cli/) in der Dokumentation.
