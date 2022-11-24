---
title: Markdown-Seiten hinzufügen
---

Gatsby kann Markdown-Dateien verwenden, um Seiten auf deiner Website zu erstellen.
Du fügst Plugins hinzu, um Ordner mit Markdown-Dateien zu lesen und zu verstehen und daraus automatisch Seiten zu erstellen.

Hier sind die Schritte, die Gatsby folgt, um dies zu erreichen.

1.  Liest Dateien aus dem Dateisystem in Gatsby ein
2.  Wandelt Markdown in HTML um und [frontmatter](#frontmatter-for-metadata-in-markdown-files) zu Daten
3.  Fügt eine Markdown-Datei hinzu
4.  Erstellt eine Seitenkomponente für die Markdown-Dateien
5.  Erstellt statische Seiten mit Node.js `createPage` API von Gatsby 

## Dateien aus dem Dateisystem in Gatsby einlesen

Benutze das plugin [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/#gatsby-source-filesystem), um Dateien zu lesen.

### Installation

`npm install --save gatsby-source-filesystem`

### Plugin hinzufügen

**Hinweis:** Es gibt zwei Möglichkeiten, ein Plugin in `gatsby-config.js` hinzuzufügen. Entweder du fügst einen String mit dem Namen des Plugins ein oder falls du Optionen einfügen möchtest, ein Objekt.

Öffne `gatsby-config.js`, um das `gatsby-source-filesystem`-Plugin hinzuzufügen. Füge nun das Objekt aus dem nächsten Block dem Array `plugins` hinzu. Durch die Übergabe eines Objekts, das den Schlüssel „path“ enthält, bestimmst du den Dateisystempfad.

```javascript:title=gatsby-config.js
plugins: [
  {
    resolve: `gatsby-source-filesystem`,
    options: {
      name: `markdown-pages`,
      path: `${__dirname}/src/markdown-pages`,
    },
  },
]
```

Wenn du den obigen Schritt abgeschlossen hast, hast du die Markdown-Dateien aus dem Dateisystem „gesourct“. Nun kannst du das Markdown in HTML und das YAML frontmatter in JSON „umzuwandeln“.

## Markdown zu HTML und Frontmatter zu Daten mit `gatsby-transformer-remark` umwandeln

Du wirst das Plugin [`gatsby-transformer-remark`](/packages/gatsby-transformer-remark/) nutzen, um Markdown-Dateien und deren Inhalt zu erkennen. Das Plugin konvertiert den Frontmatter-Metadaten-Teil deiner Markdown-Dateien als `frontmatter` und den Inhaltsteil als HTML.

### Installation des Transformer-Plugin

`npm install --save gatsby-transformer-remark`

### Plugin konfigurieren

Füge dies zu `gatsby-config.js` zu nachdem du das `gatsby-source-filesystem` hinzugefügt hast.

```javascript:title=gatsby-config.js
plugins: [
  {
    resolve: `gatsby-source-filesystem`,
    options: {
      path: `${__dirname}/src/markdown-pages`,
      name: `markdown-pages`,
    },
  },
  `gatsby-transformer-remark`,
]
```

## Eine Markdown-Datei hinzufügen

Erstelle einen Ordner im `/src` Verzeichnis deiner Gatsby-Anwendung namens `markdown-pages`.
Erstelle nun darin eine Markdown-Datei mit dem Namen `post-1.md`.

### Frontmatter für Metadaten in Markdown-Dateien

Wenn du eine Markdown-Datei erstellst, kannst du eine Reihe von Schlüssel-Wert-Paaren einfügen, die verwendet werden können, um zusätzliche Daten zu liefern, die für bestimmte Seiten in der GraphQL-Datenschicht relevant sind. Diese Daten werden als Frontmatter bezeichnet und sind durch die dreifachen Striche am Anfang und Ende des Blocks gekennzeichnet. Dieser Block wird von `gatsby-transformer-remark` als `frontmatter` zerlegt. Die GraphQL-API stellt die Schlüssel-Wert-Paare als Daten in deinen React-Komponenten bereit.

```markdown:title=src/markdown-pages/post-1.md
---
path: "/blog/my-first-post"
date: "2019-05-04"
title: "My first blog post"
---
```

Wichtig in diesem Schritt ist das Schlüsselpaar `path`. Der Wert, der dem Schlüssel `path` zugewiesen wird, wird verwendet, um zu deinem Beitrag zu navigieren.

## Eine Seitenvorlage für die Markdown-Dateien erstellen

Erstelle einen Ordner im `/src` Verzeichnis Ihrer Gatsby-Anwendung namens `templates`.
Erstelle nun eine `blogTemplate.js` in ihr mit folgendem Inhalt:

```jsx:title=src/templates/blogTemplate.js
import React from "react"
import { graphql } from "gatsby"

export default function Template({
  data, // this prop will be injected by the GraphQL query below.
}) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post-container">
      <div className="blog-post">
        <h1>{frontmatter.title}</h1>
        <h2>{frontmatter.date}</h2>
        <div
          className="blog-post-content"
          dangerouslySetInnerHTML={{ __html: html }}
        />
      </div>
    </div>
  )
}

export const pageQuery = graphql`
  query($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`
```

Zwei Dinge sind in der obigen Datei wichtig:

1.  Eine GraphQL-Abfrage wird in der zweiten Hälfte der Datei durchgeführt, um die Markdown-Daten zu erhalten. Gatsby hat dir automatisch alle Markdown-Metadaten und HTML im Ergebnis dieser Abfrage zur Verfügung gestellt.

    **Hinweis: Um mehr über GraphQL zu erfahren, schaue dir diese [hervorragende Quelle](https://www.howtographql.com/)** an

2.  Das Ergebnis der Abfrage wird von Gatsby in die `Template` Komponente als `data` eingespeist. `markdownRemark` ist die Eigenschaft, in der du alle Details der Markdown-Datei findest. Daraus kannst du eine Vorlage für die Ansicht deines Blogeintrags konstruieren. Da es sich um eine React-Komponente handelt, kannst du sie mit jeder der [empfohlenen Styling-Systeme](/docs/styling/) in Gatsby bearbeiten.

### Statische Seiten mit der Node.js `createPage` API von Gatsby erstellen

Gatsby stellt eine leistungsstarke Node.js-API zur Verfügung, die Funktionen wie die Erstellung dynamischer Seiten ermöglicht. Diese API ist verfügbar in der `gatsby-node.js` Datei im Stammverzeichnis deines Projekts, auf der gleichen Ebene wie `gatsby-config.js`. Jeder Export, der in dieser Datei enthalten ist, wird von Gatsby ausgeführt, wie in seiner [Node API Spezifikation](/docs/node-apis/) beschrieben. Du solltest dich jedoch nur um die API `createPages` kümmern.

Verwende die `graphql` um die Daten einer Markdown-Datei wie unten beschrieben abzufragen. Verwende danach den `createPage` Action Creator, um eine Seite für jede der Markdown-Dateien zu erstellen, indem du die `blogTemplate.js`, die du im vorherigen Schritt erstellt hast, benutzt.

**HINWEIS:** Gatsby ruft die `createPages` API (falls vorhanden) zur Erstellungszeit mit eingespeisten Parametern, `actions` und `graphql`.

```javascript:title=gatsby-node.js
const path = require(`path`)

exports.createPages = async ({ actions, graphql, reporter }) => {
  const { createPage } = actions

  const blogPostTemplate = path.resolve(`src/templates/blogTemplate.js`)

  const result = await graphql(`
    {
      allMarkdownRemark(
        sort: { order: DESC, fields: [frontmatter___date] }
        limit: 1000
      ) {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `)

  // Handle errors
  if (result.errors) {
    reporter.panicOnBuild(`Error while running GraphQL query.`)
    return
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.path,
      component: blogPostTemplate,
      context: {}, // additional data can be passed via context
    })
  })
}
```

Damit solltest einige der grundlegenden Markdown-Funktionen auf deiner Gatsby-Site kennengelernt haben. Du kannst das Frontmatter und die Vorlagendatei weiter anpassen, um die gewünschten Effekte zu erzielen!

Weitere Informationen findest du im Beispiel `using-markdown-pages`. Du findest es in der [Gatsby Beispielbereich](https://github.com/gatsbyjs/gatsby/tree/master/examples).

## Andere Tutorials

Schau dir die Tutorials auf der [Awesome Gatsby](/docs/awesome-gatsby-resources/#gatsby-tutorials) Seite für weitere Informationen zur Erstellung von Gatsby-Seiten mit Markdown an.

## Gatsby Markdown Vorlagen

Außerdem gibt es eine Reihe von [Gatsby Vorlagen](/starters?c=Markdown), die für die Arbeit mit Markdown vorkonfiguriert sind.
