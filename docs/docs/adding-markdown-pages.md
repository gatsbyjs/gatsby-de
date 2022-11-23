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

2.  The result of the query is injected by Gatsby into the `Template` component as `data`. `markdownRemark` is the property that you'll find has all the details of the Markdown file. You can use that to construct a template for your blog post view. Since it's a React component, you could style it with any of the [recommended styling systems](/docs/styling/) in Gatsby.

### Create static pages using Gatsby’s Node.js `createPage` API

Gatsby exposes a powerful Node.js API, which allows for functionality such as creating dynamic pages. This API is available in the `gatsby-node.js` file in the root directory of your project, at the same level as `gatsby-config.js`. Each export found in this file will be run by Gatsby, as detailed in its [Node API specification](/docs/node-apis/). However, you should only care about one particular API in this instance, `createPages`.

Use the `graphql` to query Markdown file data as below. Next, use the `createPage` action creator to create a page for each of the Markdown files using the `blogTemplate.js` you created in the previous step.

**NOTE:** Gatsby calls the `createPages` API (if present) at build time with injected parameters, `actions` and `graphql`.

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

This should get you started on some basic Markdown functionality in your Gatsby site. You can further customize the frontmatter and the template file to get desired effects!

For more information, have a look in the working example `using-markdown-pages`. You can find it in the [Gatsby examples section](https://github.com/gatsbyjs/gatsby/tree/master/examples).

## Other tutorials

Check out tutorials listed on the [Awesome Gatsby](/docs/awesome-gatsby-resources/#gatsby-tutorials) page for more information on building Gatsby sites with Markdown.

## Gatsby Markdown starters

There are also a number of [Gatsby starters](/starters?c=Markdown) that come pre-configured to work with Markdown.
