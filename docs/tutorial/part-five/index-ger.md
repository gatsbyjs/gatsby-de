---
title: Source Plugins
typora-copy-images-to: ./
disableTableOfContents: true
---

> Dieses Tutorial ist Teil einer Reihe über Gatsby's Datenschicht. Stell sicher, dass du [Teil 4](/tutorial/part-four/) durchgegangen bist, bevor du hier fortsetzt.

## Was beinhaltet dieses Tutorial?

In diesem Tutorial lernst du, wie man mit Hilfe von GraphQL und Quellen-Plugins Daten in Gatsby Seiten integrieren kannst. Bevor du dich jedoch mit diesen Plugins vertraut machst, solltest du wissen, wie man etwas namens GraphiQL verwendet. GraphiQL ist ein Tool, dass dir bei der korrekten Strukturierung deiner Abfragen hilft.

## Einführung von GraphiQL

GraphiQL ist die integrierte Entwicklungsumgebung (abgekürzt _IDE_, von der folgenden englischen Namensgebung: integrated development environment) für GraphQL. Es ist ein effizientes (und rundum hervorragendes) Tool, das du beim Erstellen von Gatsby Webseiten häufig verwenden wirst.

Du kannst darauf zugreifen, wenn der Entwicklungsserver deiner Gatsby Seite läuft — üblicherweise unter
`http://localhost:8000/___graphql`.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

Schau dir in der eingebauten `Site` "type" an und welche Felder darin verfügbar sind -- einschließlich des `SiteMetadata`-Objektes, das du zuvor abgefragt hast.  Versuche, GraphiQL zu öffnen und mit deinen Daten herumzuspielen! Drücke <kbd>Strg + Leertaste<kbd> (oder verwende <kbd>Umschalt + Leertaste</kbd> als alternatives Tastaturkürzel) um das Autovervollständigungsfenster aufzurufen und <kbd>Strg + Eingabe<kbd> um die GraphQL-Abfrage auszuführen. Du wirst im weiteren Verlauf des Tutorials wesentlich mehr mit GraphiQL arbeiten.

## Verwendung des GraphiQL-Explorers

Der GraphiQL Explorer ermöglicht dir die interaktive Erstellung vollständiger Abfragen, indem du dich durch die verfügbaren Felder und Eingaben klicken kannst, ohne dass du diese Abfragen wiederholt von Hand abtippen musst.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>

## Quell-Plugins

Daten in Gatsby-Sites können von überall her kommen: APIs (Abkürzung von application programming interface, Programmierschnittstelle), Datenbanken, CMS ( content management system, deutsch Inhaltsverwaltungssystem), lokale Dateien, etc.

Quell-Plugins holen die Daten aus ihrer Quelle. Das Dateisystem Quell-Plugin weiß z.B., wie man Daten aus dem Dateisystem holt. Das WordPress-Plugin weiß, wie man Daten von der WordPress-API abruft.

Füge [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) hinzu und erkunde, wie es funktioniert.

Zuerst installiert man das Plugin im Stammverzeichnis des Projekts:

```shell
npm install --save gatsby-source-filesystem
```

Dann füge es zu deiner `gatsby-config.js` hinzu:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    // highlight-start
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    // highlight-end
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

Speichere das und starte den Gatsby-Entwicklungsserver neu. Dann öffne GraphiQL erneut.

Im Explorer-Fenster sieht man nun `allFile` und `file` als Auswahlmöglichkeiten:

![graphiql-filesystem](graphiql-filesystem.png)

Klicke auf die Auswahlliste `allFile`. Platziere deinen Mauszeiger direkt nach "allFile" im Abfragebereich und gib dann <kbd>Strg + Enter</kbd> ein. Dadurch wird eine Abfrage für die `id` jeder Datei vorausgefüllt. Klicke auf "Play", um die Abfrage erneut auszuführen:

![filesystem-query](filesystem-query.png)

Im Explorer-Fenster ist das Feld `id` automatisch ausgewählt worden. Wähle weitere Felder aus, indem du das entsprechende Kontrollkästchen des Feldes markierst. Drücke nun auf "Play", um die Abfrage mit den neuen Feldern erneut auszuführen:

![filesystem-explorer-options](filesystem-explorer-options.png)

Alternativ kannst du Felder hinzufügen, indem du die Tastenkombination zum automatischen Ausfüllen verwendest (<kbd>Strg + Leertaste</kbd>). Dadurch werden abfragbare Felder auf den `File`-Knoten (Knoten ist ein ausgefallener Name für ein Objekt in einem "Graph") angezeigt.

![filesystem-autocomplete](filesystem-autocomplete.png)

Versuche es, mehrere Felder zu deiner Abfrage hinzuzufügen. Um die Abfrage erneut auszuführen, drücke <kbd>Strg + Enter</kbd>. Die Ergebnisse der aktualisierten Abfrage werden dann angezeigt:

![allfile-query](allfile-query.png)

Das Ergebnis  ist eine Liste von `File`-"Knoten". Jedes `File`-Knotenobjekt hat die Felder, nach denen du abgefragt hast.

## Eine Seite mit einer GraphQL-Abfrage erstellen

Der Aufbau neuer Seiten mit Gatsby beginnt oft in GraphiQL. Zuerst skizziert man die Datenabfrage in GraphiQL und kopiert diese dann in eine React-Seitenkomponente um mit dem Aufbau der Benutzeroberfläche zu beginnen.

Lasst uns dies versuchen.

Erstelle eine neue Datei unter `src/pages/my-files.js` mit der GraphQL-Abfrage `allFile`, die du gerade gemacht hast:

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data) // highlight-line
  return (
    <Layout>
      <div>Hello world</div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

Die Zeile `console.log(data)` ist oben farblich hervorgehoben. Es ist oft hilfreich, die Daten, die man von der GraphQL-Abfrage erhält, in der Browser-Konsole anzuzeigen, wenn man eine neue Komponente erstellt, so dass man die Daten in der Browser-Konsole untersuchen kann, während man die Benutzeroberfläche erstellt.

Wenn du die neue Seite unter `/my-files/` besuchst und deine Browser-Konsole öffnest siehst du so etwas wie:

![data-in-console](data-in-console.png)

Die Form der Daten stimmt mit der Form der GraphQL-Abfrage überein.

Füge etwas Code zu deiner Komponente hinzu, um die Dateidaten auszudrucken.

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>My Site's Files</h1>
        <table>
          <thead>
            <tr>
              <th>relativePath</th>
              <th>prettySize</th>
              <th>extension</th>
              <th>birthTime</th>
            </tr>
          </thead>
          <tbody>
            {data.allFile.edges.map(({ node }, index) => (
              <tr key={index}>
                <td>{node.relativePath}</td>
                <td>{node.prettySize}</td>
                <td>{node.extension}</td>
                <td>{node.birthTime}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

Und jetzt schau dir `http://localhost:8000/my-files` an... 😲

![my-files-page](my-files-page.png)

## Was kommt als nächstes?

Jetzt haben wir gelernt, wie Quell-Plugins Daten in das Datensystem von Gatsby bringen. Im nächsten Tutorial lernst du, wie Transformations-Plugins die von den Source-Plugins gelieferten Rohdaten umwandeln.  Die Kombination aus Source-Plugins und Transformations-Plugins kann alle Datenbeschaffungs- und Datenumwandlungsaufgaben übernehmen, die bei der Erstellung einer Gatsby-Website anfallen. Mehr über Transformations-Plugins erfährst du in [part six of the tutorial](/tutorial/part-six/).