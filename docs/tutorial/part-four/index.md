---
title: Daten in Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

Willkommen zum vierten Teil des Tutorials! Zur Hälfte fertig! Hoffentlich fangen
die Dinge an vertrauter zu werden 😀

## Rückblick auf die erste Hälfte des Tutorials

Bislang hast du gelernt React.js einzusetzen und wie mächtig es ist, *eigene* Komponenten
schreiben zu können, die als benutzerdefinierte Bausteine für Websites agieren.

Mit Hilfe der CSS Modules hast du auch erfahren, wie Komponenten gestyled werden können.

## Was beinhaltet dieses Tutorial?

In den nächsten vier Teilen des Tutorials (inklusive diesem) wirst du in die Datenschicht von Gatsby eintauchen, welche ein mächtiges Feature von Gatsby darstellt und dich mit Leichtigkeit Seiten aus Markdown, WordPress, Headless CMS, sowie vielen anderen Datenquellen, erstellen lässt.

**HINWEIS:** Gatsby’s Datenschicht wird mit GraphQL betrieben. Für ein ausführliches
Tutorial über GraphQL, empfehlen wir [How to GraphQL](https://www.howtographql.com/).

## Daten in Gatsby

Eine Gatsby Website besteht vier aus Teilen: HTML, CSS, JS und Daten. Die erste Hälfte
des Tutorials fokussierte sich auf die ersten drei. Nun, lass uns lernen wie
Daten in Gatsby Webseiten eingebunden werden können.

**Was sind Daten?**

Eine sehr informatikfokussierte Antwort würde lauten: Daten sind Dinge wie `"Strings"`,
Integers (`42`), Objekte (`{ pizza: true }`), etc.

Für den Zweck des Arbeitens mit Gatsby ist jedoch "Alles was außerhalb einer React-Komponente lebt",
eine viel nützlichere Antwort.

Bisher, has du *direkt* in den Komponenten Text geschrieben und Bilder hinzugefügt.
Dies ist eine *hervorragende* Möglichkeit Websites zu entwickeln. Nichtsdestotrotz, willst
du manchmal Daten *außerhalb* der Komponenten speichern und dann diese Daten *in* die
Komponente bei Bedarf reinbringen.

Wenn du eine Seite mit WordPress (so haben andere Beitragende
eine schöne Oberfläche für das Hinzufügen & Pflegen der Inhalte) und Gatsby baust,
sind die *Daten* für die Website (Seiten und Beiträge) in WordPress und du *ziehst*
diese Daten bei Bedarf in deine Komponenten.

Dateitypen wie Markdown, CSV, etc. sowie Datenbanken und Schnittstellen aller Arten, können
als Datenquellen agieren.

**Gatsby's Datenschicht lässt dich von diesen (und vielen anderen) Datenquellen,
Daten in deine Komponenten beziehen**, in jeder beliebiger Gestalt und Form.

## Nutzen von unstrukturierten Daten vs GraphQL

### Muss ich GraphQL und Quellen-Plugins nutzen, um Daten in eine Gatsby Website einzubinden?

Natürlich nicht! Du kannst die node API `createPages` nutzen, um unstrukturierte Daten auf Gatsby Seiten direkt zu beziehen, anstatt eine GraphQL Datenschicht zu nutzen. Dies stellt eine gute Wahl für kleine Websites dar, während GraphQL und Quellen-Plugins dir beim Zeitsparen helfen, wenn es sich um komplexe Websites handelt.

Schau dir die [Nutzen von Gatsby ohne GraphQL](/docs/using-gatsby-without-graphql/) Anleitung an, um zu lernen, wie du Daten auf deiner Gatsby Website mittels node `createPages` API beziehen kannst und um eine Beispielseite zu sehen.

### Wann verwende ich unstrukturierte Daten und wann GraphQL?

Wenn du eine kleine Website baust, stellt in dieser Anleitung beschriebene Vorgehensweise, eine effiziente Möglichkeit dar, mit Hilfe der `createPages` API unstrukturierte Daten einzubinden. Wenn die Website später komplexer werden sollte, du weitere komplexe Websites erstellst oder einfach deine Daten umwandeln willst, solltest du diese Schritte befolgen:

1.  Siehe in der [Plugin Bibliothek](/plugins/) nach, ob es bereits Quellen-Plugins und/oder Transformer-Plugins gibt, die du nutzen möchtest
2.  Falls es keine gibt, lies die [Plugin Authoring](/docs/creating-plugins/) Anleitung und ziehe es in Erwägung dein eigenes Plugin zu entwickeln!

### Wie Gatsby's Datenschicht GraphQL nutzt, um Komponenten mit Daten zu versorgen

Es gibt viele Möglichkeiten für das Laden der Daten innerhalb von React-Komponenten. Eine der meistverwendeten
und wirksamen Möglichkeiten ist eine Technologie namens [GraphQL](http://graphql.org/).

GraphQL wurde von Facebook erfunden, um den Produktingenieuren zu helfen, notwendige Daten
in die Kompontenten zu *ziehen*.

GraphQL ist eine **Q**uery **L**anguage (der *QL* Teil des Namens). Wenn du mit
SQL vertraut bist, ist die Funktionsweise sehr ähnlich. Unter Anwendung von spezieller Syntax
beschreibst du die Daten, die du in deiner Komponente haben willst und dann erhältst
du diese Daten.

Gatsby nutzt GraphQL um den Komponenten zu ermöglichen, benötigte Daten zu deklarieren.

## Erstelle eine neue Beispielseite

Erstelle eine andere neue Seite für diesen Abschnitt des Tutorials. Du wirst ein Markdown-Blog namens "Pandas essen viel" erstellen. Es ist der Darstellung von besten Bildern und Videos von Pandas
die viel Essen gewidmet. Nebenbei wirst du in GraphQL und Gatsby's Markdown-Support eintauchen.

Öffne ein neues Fenster im Terminal und führe folgende Befehle aus, um eine neue Gatsby-Seite im Ordner `tutorial-part-four` zu erstellen. Navigiere danach in das erstellte Verzeichnis:

```shell
gatsby new tutorial-part-four https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-four
```

Installiere danach einige notwendige Abhängigkeiten im Root-Verzeichnis des Projekts. Du wirst das Typografie-Thema
"Kirkham" nutzen und eine CSS-in-JS Bibliothek ausprobieren, ["Emotion"](https://emotion.sh/):

```shell
npm install --save gatsby-plugin-typography typography react-typography typography-theme-kirkham gatsby-plugin-emotion @emotion/core
```

Setze eine Website auf die der am Ende des [Teil Drei](/tutorial/part-three) Tutorials ähnlich ist. Diese Website hat eine Layout-Komponente und zwei Seiten-Komponenten:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
import { Link } from "gatsby"

import { rhythm } from "../utils/typography"

export default ({ children }) => (
  <div
    css={css`
      margin: 0 auto;
      max-width: 700px;
      padding: ${rhythm(2)};
      padding-top: ${rhythm(1.5)};
    `}
  >
    <Link to={`/`}>
      <h3
        css={css`
          margin-bottom: ${rhythm(2)};
          display: inline-block;
          font-style: normal;
        `}
      >
        Pandas essen viel
      </h3>
    </Link>
    <Link
      to={`/about/`}
      css={css`
        float: right;
      `}
    >
      Über uns
    </Link>
    {children}
  </div>
)
```

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>Erstaunliche Pandas essen Dinge</h1>
    <div>
      <img
        src="https://2.bp.blogspot.com/-BMP2l6Hwvp4/TiAxeGx4CTI/AAAAAAAAD_M/XlC_mY3SoEw/s1600/panda-group-eating-bamboo.jpg"
        alt="Gruppe von Pandas essen Bambus"
      />
    </div>
  </Layout>
)
```

```jsx:title=src/pages/about.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>Über Pandas essen viel</h1>
    <p>
      Wir sind die einzige Seite auf deinem Computer, die der Darstellung von besten
      Bildern und Videos von Pandas die viel Essen gewidmet ist.
    </p>
  </Layout>
)
```

```javascript:title=src/utils/typography.js
import Typography from "typography"
import kirkhamTheme from "typography-theme-kirkham"

const typography = new Typography(kirkhamTheme)

export default typography
export const rhythm = typography.rhythm
```

`gatsby-config.js` (muss im Root-Verzeichnis deines Projektes sein, nicht unter src)

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
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

Füge die oberen Dateien hinzu und führe danach wie immer `gatsby develop` aus, du solltest nun folgendes sehen:

![start](start.png)

Du hast eine weitere kleine Website mit einem Layout und zwei Seiten.

Nun kannst du mit Abfragen beginnen 😋

## Deine erste GraphQL-Abfrage

Während der Entwicklung von Websites, möchtest du wahrscheinlich die häufig benutzten Teile der Daten wiederverwenden -- zum Beispiel den *Seitentitel*. Schau dir die `/about/` Seite an. Du wirst merken, dass du den Seitentitel (`Pandas essen viel`) in beiden Komponenten (Website Titel) und im `<h1 />` der `about.js` Seite (Seitentitel) hast.

Aber was wäre, wenn du den Titel der Website in der Zukunft ändern möchtest? Du müsstest nach dem Titel in allen deinen Komponenten suchen und jede Instanz manuell ändern. Dies ist nicht nur mühselig, sondern auch fehleranfällig, speziell bei größeren und komplexeren Websites. Stattdessen, speicherst du den Titel an einem Ort und referenzierst diesen Ort in anderen Dateien; Ändere den Titel an einem einzigen Ort und Gatsby wird den aktualisierten Titel in den Dateien, die diesen referenzieren *erneuern*.

Der Ort für diese häufig verwendeten Teile der Daten ist das `siteMetadata` Objekt in der `gatsby-config.js`. Füge den Titel deiner Website in die `gatsby-config.js` Datei hinzu:

```javascript:title=gatsby-config.js
module.exports = {
  // highlight-start
  siteMetadata: {
    title: `Titel von siteMetadata`,
  },
  // highlight-end
  plugins: [
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

Starte den Entwicklungsserver neu.

### Nutzung einer Seitenabfrage

Nun ist der Titel der Website bereit für eine Abfrage. Füge diesen zu der `about.js` Datei hinzu in dem du eine [Seitenabfrage](/docs/page-query) nutzt:

```jsx:title=src/pages/about.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-next-line
export default ({ data }) => (
  <Layout>
    <h1>Über {data.site.siteMetadata.title}</h1> {/* highlight-line */}
    <p>
      Wir sind die einzige Seite auf deinem Computer, die der Darstellung von besten
      Bildern und Videos von Pandas die viel Essen gewidmet ist.
    </p>
  </Layout>
)

// highlight-start
export const query = graphql`
  query {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end
```

Es hat funktioniert! 🎉

![Abfragen des Titels der Seite von siteMetadata](site-metadata-title.png)

Die grundlegende GraphQL-Abfrage welche die Änderungen von `title` in deiner `about.js` erhält, sieht folgendermaßen aus:

```graphql:title=src/pages/about.js
{
  site {
    siteMetadata {
      title
    }
  }
}
```

> 💡 Im [fünften Teil](/tutorial/part-five/#introducing-graphiql) wirst du ein Tool kennenlernen, welches es dir ermöglicht, die durch GraphQL verfügbaren Daten interaktiv zu entdecken und Abfragen, wie die vorherige zu formulieren.

Seitenabfragen existieren außerhalb der Komponenten-Definition -- grundsätzlich am Ende einer Seitenkomponente Datei -- und nur in Seitenkomponenten verfügbar.

### Nutzung einer StaticQuery

[StaticQuery](/docs/static-query/) ist eine neue API die mit Gatsby v2 eingeführt wurde und es ermöglicht, Komponenten die keine Seite darstellen (wie deine `layout.js` Komponente), mittels GraphQL-Abfragen mit Daten zu versorgen.
Lass uns die neu vorgestellte Hook-Variante - [`useStaticQuery`](/docs/use-static-query/) nutzen.

Leg los und passe deine `src/components/layout.js` Datei an, um den `useStaticQuery` Hook und die `{data.site.siteMetadata.title}` Referenz zu nutzen, die diese Daten nutzt. Wenn du fertig bist, sollte deine Datei wie folgt aussehen:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
// highlight-next-line
import { useStaticQuery, Link, graphql } from "gatsby"

import { rhythm } from "../utils/typography"
// highlight-start
export default ({ children }) => {
  const data = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
          }
        }
      }
    `
  )
  return (
    // highlight-end
    <div
      css={css`
        margin: 0 auto;
        max-width: 700px;
        padding: ${rhythm(2)};
        padding-top: ${rhythm(1.5)};
      `}
    >
      <Link to={`/`}>
        <h3
          css={css`
            margin-bottom: ${rhythm(2)};
            display: inline-block;
            font-style: normal;
          `}
        >
          {data.site.siteMetadata.title} {/* highlight-line */}
        </h3>
      </Link>
      <Link
        to={`/about/`}
        css={css`
          float: right;
        `}
      >
        Über uns
      </Link>
      {children}
    </div>
    // highlight-start
  )
}
// highlight-end
```

Erneut ein Erfolg! 🎉

![Seitentitel und Layouttitel beziehen nun beide den Wert von siteMetadata](site-metadata-two-titles.png)

Warum nutzen wir zwei unterschiedliche Abfragen hier? Diese Beispiele waren eine
schnelle Einführung zu den Abfragetypen, wie sie formattiert werden
und wo sie genutzt werden. Für den Augenblick, solltest du beachten,
dass Seitenabfragen nur von Seiten durchgeführt werden können. Komponenten die
keine Seite darstellen, wie Layout, können StaticQuery nutzen. Im [Teil 7](/tutorial/part-seven/) des Tutorials
werden diese ausführlicher behandelt.

Lass uns jedoch den echten Seitentitel wiederherstellen.

Eines der Grundprinzipien von Gatsby ist, dass *Erschaffer eine unmittelbare Verbindung zu ihrer Kreation brauchen* ([Kudos an Bret Victor](http://blog.ezyang.com/2012/02/transcript-of-inventing-on-principle/)). In anderen Worten, wenn du eine Änderung in deinem Code machst, solltest du unmittelbar die Auswirkung dieser Änderung sehen. Du manipulierst den Input von Gatsby und siehst wie der neue Output auf dem Bildschirm erscheint.

Dies findet fast überall statt, Änderungen die von dir durchgeführt werden, treten unmittelbar in Kraft. Bearbeite wieder die `gatsby-config.js` Datei, ändere dieses Mal den `title` zurück auf "Pandas essen viel". Die Änderung sollte sehr schnell auf deiner Website erscheinen.

![Beide Titel sind nun auf Pandas essen viel gesetzt](pandas-eating-lots-titles.png)

## Was kommt als Nächstes?

Als Nächstes wirst du im [fünften Teil](/tutorial/part-five/) des Tutorials lernen,
wie deine Gatsby Website mittels GraphQL und Quellen-Plugins mit Daten versorgt werden kann.
