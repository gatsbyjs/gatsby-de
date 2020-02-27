---
title: Source Plugins
typora-copy-images-to: ./
disableTableOfContents: true
---

> Dieses Tutorial ist Teil einer Reihe über Gatsby's Datenschicht. Stell sicher, dass du [Teil 4](/tutorial/part-four/) durchgegangen bist, bevor du hier fortsetzt.

## Was beinhaltet dieses Tutorial?

In diesem Tutorial lernst du, wie man mit Hilfe von GraphQL und Quellen-Plugins Daten in Gatsby Seiten integrieren kannst. Bevor du dich jedoch mit diesen Plugins vertraut machst solltest du wissen, wie man etwas names GraphiQL verwendet. GraphiQL ist ein Tool, dass dir bei der korrekten Strukturierung deiner Abfragen hilft.

## Einführung von GraphiQL

GraphiQL ist die integrierte Entwicklungsumgebung (abgekürzt _IDE_, von der folgenden englischen Namensgebung: integrated development environment) für GraphQL. Es ist ein effizientes (und rundum hervorragendes) Tool, das du beim Erstellen von Gatsby Webseiten häufig verwenden wirst.

Du kannst darauf zugreifen, wenn der Entwicklungsserver deiner Gatsby Seite läuft — üblicherweise unter
`http://localhost:8000/___graphql`.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

Schau dir in der eingebauten `Site` "type" an und welche Felder darin verfügbar sind -- einschließlich des `SiteMetadata`-Objektes, das du zuvor abgefragt hast.  Versuche, GraphiQL zu öffnen und mit deinen Daten herumzuspielen! Drücke <kbd>Strg + Leertaste<kbd> (oder verwende <kbd>Umschalt + Leertaste</kbd> als alternatives Tastaturkürzel) um das Autovervollständigungsfenster aufzurufen und Strg + Eingabe um die GraphQL-Abfrage auszuführen. Du wirst im weiteren Verlauf des Tutorials wesentlich mehr mit GraphiQL arbeiten.

## Verwendung des GraphiQL-Explorers

Der GraphiQL Explorer ermöglicht dir die interaktive Erstellung vollständiger Abfragen, indem du dich durch die verfügbaren Felder und Eingaben klicken kannst, ohne dass du diese Abfragen wiederholt von Hand abtippen musst.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>