---
title: Transformer-Plugins
typora-copy-images-to: ./
disableTableOfContents: true
---

> Dieses Tutorial ist Teil einer Reihe über die Datenschicht von Gatsby. Stellen Sie sicher, dass Sie [Teil 4](/tutorial/part-four/) und [Teil 5](/tutorial/part-five/) durchgegangen sind, bevor Sie hier fortfahren.

## Um was geht es in diesem Tutorial?

Das vorherige Tutorial zeigte, wie Quellen-Plugins Daten _in_ Gatsbys Datensystem bringen. In diesem Tutorial erfahren Sie, wie Transformer-Plugins den Inhalt von Quellen-Plugins _transformieren_. Die Kombination aus Quellen-Plugins und Transformer-Plugins kann alle Datenquellen und -transformationen verarbeiten, die beim Erstellen einer Gatsby-Site benötigt werden.

## Transformer plugins

Oft ist das Format der Daten, die Sie von Quellen-Plugins erhalten, nicht das, was Sie brauchen, um Ihre Website zu erstellen. Mit dem Quellen-Plugin des Dateisystems können Sie Daten _über_ Dateien abfragen. Aber was ist, wenn Sie Daten _aus_ Dateien abfragen möchten?

Um dies zu ermöglichen, unterstützt Gatsby Transformer-Plugins welche die rohen Daten von Quellen-Plugins entgegen nehmen und in etwas anderes _transformieren_ können.

Zum Beispiel Markdown. Markdown schreibt sich leicht, aber wenn man einen baut Seite, müssen Sie die Markdown Dateien in HTML umgewandelt werden.

Fügen Sie Ihrer Site unter eine Markdown-Datei unter
`src/pages/sweet-pandas-eating-sweets.md` hinzu. (Dies wird Ihr erster Markdown Blogeintrag)
und lernen, wie, mit Hilfe von Transformer-Plugins und GraphQL, die Daten in HTML umwandelt werden.

```markdown:title=src/pages/sweet-pandas-eating-sweets.md
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Pandas are really sweet.

Here's a video of a panda eating sweets.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

Sobald Sie die Datei gespeichert haben, schauen Sie auf `/my-files/` Wieder - Die neue Abschriftendatei befindet sich in Der Tisch. Dies ist eine sehr leistungsstarke Funktion von Gatsby. Wie im vorherigen Beispiel `siteMetadata` können Quell-Plugins Daten live neu laden.
`gatsby-source-filesystem` sucht immer nach neuen Dateien und wann Sie führen Ihre Abfragen erneut aus.

Fügen Sie ein Transformer-Plugin hinzu, das Markdown-Dateien transformieren kann:

```shell
npm install --save gatsby-transformer-remark
```

Fügen Sie es dann, wie gewohnt, in die `gatsby-config.js` Folgendes ein:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-transformer-remark`, // highlight-line
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

Starten Sie den Entwicklungsserver neu, aktualisieren Sie GraphiQL (oder öffnen Sie es erneut), und schauen Sie sich die Autovervollständigungen an:

![markdown-autocomplete](markdown-autocomplete.png)

Wählen Sie `allMarkdownRemark` erneut aus und führen Sie es wie für `allFile` aus. Sie wirden dort die Markdown-Datei sehen, die Sie gerade hinzugefügt haben. Schauen Sie sich die Felder, die auf dem `MarkdownRemark` Knoten verfügbar sind, an.

![markdown-query](markdown-query.png)

In Ordnung! Hoffentlich fangen einige Grundlagen an, sich zu etablieren. Quellen-Plugins bringen Daten _in_ das Gatsby-Datensystem. Dieses Muster kann alle Datenquellen und -umwandlungen verarbeiten, die Sie beim Erstellen einer Gatsby-Site benötigen.

## Erstellen Sie eine Liste der Markdown-Dateien Ihrer Site in `src/pages/index.js`

Jetzt müssen Sie eine Liste Ihrer Markdown-Dateien auf der Startseite erstellen. Wie viele Blogs, möchten Sie mit einer Liste von Links auf der Titelseite enden, die auf jeden Blogeintrag verlinken. Mit GraphQL können Sie _query_ für die aktuelle Liste der Blog-Beiträge abfragen, sodass Sie die Liste nicht manuell pflegen müssen.

Wie bei der `src/pages/my-files.js` Seite ersetzen `src/pages/index.js` mit Im Folgenden wird eine GraphQL-Abfrage mit anfänglichem HTML und Stil hinzugefügt.

```jsx:title=src/pages/index.js
import React from "react"
import { graphql } from "gatsby"
import { css } from "@emotion/core"
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      <div>
        <h1
          css={css`
            display: inline-block;
            border-bottom: 1px solid;
          `}
        >
          Amazing Pandas Eating Things
        </h1>
        <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
        {data.allMarkdownRemark.edges.map(({ node }) => (
          <div key={node.id}>
            <h3
              css={css`
                margin-bottom: ${rhythm(1 / 4)};
              `}
            >
              {node.frontmatter.title}{" "}
              <span
                css={css`
                  color: #bbb;
                `}
              >
                — {node.frontmatter.date}
              </span>
            </h3>
            <p>{node.excerpt}</p>
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          excerpt
        }
      }
    }
  }
`
```

Nun sollte die Startseite so aussehen:

![frontpage](frontpage.png)

Aber Ihr einziger Blog-Beitrag sieht ein bisschen einsam aus. Also lassen Sie uns noch einen hinzufügen bei
`src/pages/pandas-and-bananas.md`

```markdown:title=src/pages/pandas-and-bananas.md
---
title: "Pandas and Bananas"
date: "2017-08-21"
---

Do Pandas eat bananas? Check out this short video that shows that yes! pandas do
seem to really enjoy bananas!

<iframe width="560" height="315" src="https://www.youtube.com/embed/4SZl1r2O_bY" frameborder="0" allowfullscreen></iframe>
```

![two-posts](two-posts.png)

Das Ergebnis sieht gut aus! Außer dass die Reihenfolge der Beiträge falsch ist.

Dies ist jedoch leicht zu beheben. Wenn Sie eine Verbindung eines bestimmten Typs abfragen, können Sie der GraphQL-Abfrage eine Reihe von Argumenten übergeben. Sie können die Knoten `sort` und `filter`, die Anzahl der zu überspringenden Knoten und das `limit` für die Anzahl der abzurufenden Knoten festlegen. Mit diesen leistungsstarken Operatoren können Sie beliebige Daten auswählen - in dem Format, das Sie benötigen.

Ändern Sie in der GraphQL-Abfrage Ihrer Indexseite `allMarkdownRemark` in `allMarkdownRemark` (sort: {fields: [frontmatter___date], order: DESC}). _Hinweis: Zwischen `frontmatter` und` date` gibt es 3 Unterstriche._ Speichern Sie die Abfrage und die Sortierreihenfolge sollte nun richtig sein.

Versuchen Sie, GraphiQL zu öffnen und mit verschiedenen Sortieroptionen zu spielen. `allFile` lässt sich mit mit anderen Verbindungen sortieren.

Weitere Informationen zu unseren Abfrageoperatoren finden Sie in unserer [GraphQL reference guide.](/docs/graphql-reference/)

## Herausforderung

Versuchen Sie, eine neue Seite mit einem Blogeintrag zu erstellen, und sehen Sie, was mit der Liste der Blogeinträge auf der Startseite geschieht!

## Was kommt als nächstes

Das ist toll! Sie haben gerade eine schöne Indexseite erstellt, auf der Sie Ihre abgefragten Dateien als eine Liste von Blog-Beitrags-Titeln und Text-Auszügen darstellen. Sie möchten jedoch nicht nur Text-Auszüge sehen, sondern auch tatsächliche Seiten für Ihre Markdown-Dateien.

Sie können weitere Seiten erstellen, indem Sie die React-Komponenten in `src/pages` platzieren. 
Als Nächstes lernen Sie, wie Sie _programmatisch_ Seiten aus _Daten_ erstellen. 
Gatsby ist _nicht_ darauf beschränkt, Seiten aus Dateien zu erstellen, wie dies bei vielen statischen Site-Generatoren der Fall ist. 
Mit Gatsby können Sie GraphQL verwenden, um Ihre _Daten_ abzufragen und die Abfrageergebnisse _Seiten_ zuzuordnen - alles zur Erstellungszeit.
Dies ist eine wirklich starke Idee. 
Im nächsten Tutorial werden die Vorraussetzungen und Möglichkeiten erläutert, mit denen [Seiten programmatisch aus Daten erstellt werden](/tutorial/part-seven/) können.
