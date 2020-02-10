---
title: Transformer-Plugins
typora-copy-images-to: ./
disableTableOfContents: true
---

> Dieses Tutorial ist Teil einer Reihe über die Datenschicht von Gatsby. Stellen Sie sicher, dass Sie durchgegangen sind [teil 4](/tutorial/part-four/) und [teil 5](/tutorial/part-five/) bevor Sie hier fortfahren.

## Was ist in diesem Tutorial?

Das vorherige Tutorial zeigte, wie Quell-Plugins Daten _in_ Gatsbys Datensystem bringen. In diesem Tutorial erfahren Sie, wie Transformer-Plugins den Rohinhalt von Quell-Plugins _transformieren_. Die Kombination aus Quell-Plugins und Transformer-Plugins kann alle Datenquellen und -transformationen verarbeiten, die Sie beim Erstellen einer Gatsby-Site benötigen.

## Transformer plugins

Oft ist das Format der Daten, die Sie von Quell-Plugins erhalten, nicht das, was Sie wollen Verwenden Sie, um Ihre Website zu erstellen. Mit dem Quell-Plugin des Dateisystems können Sie Daten abfragen 17 _über_ Dateien, aber was ist, wenn Sie _interne_ Datendateien abfragen möchten?

Um dies zu ermöglichen, unterstützt Gatsby Transformer-Plugins, die Raw-Dateien akzeptieren Inhalte aus Quell-Plugins und _transformieren_ sie in etwas Besseres.

Zum Beispiel Abschriften. Markdown ist schön einzuschreiben, aber wenn man einen baut Seite mit, müssen Sie den Markdown HTML sein.

Fügen Sie Ihrer Site unter eine Abschriften-Datei hinzu
`src/pages/sweet-pandas-eating-sweets.md` (Dies wird zu Ihrem ersten Abschlag Blogeintrag)
und lernen, wie man es mit Hilfe von Transformer-Plugins und in HTML umwandelt GraphQL.

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

Fügen Sie es dann wie gewohnt in die `gatsby-config.js` ein:

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

Starten Sie den Entwicklungsserver neu, aktualisieren Sie GraphiQL (oder öffnen Sie es erneut), und sehen Sie nach bei der Autovervollständigung:

![markdown-autocomplete](markdown-autocomplete.png)

Wählen Sie `allMarkdownRemark` erneut aus und führen Sie es wie für `allFile` aus. Du wirst  Dort sehen Sie die Markdown-Datei, die Sie kürzlich hinzugefügt haben. Erforschen Sie die Felder, die sind verfügbar auf dem `MarkdownRemark` Knoten.

![markdown-query](markdown-query.png)

In Ordnung! Hoffentlich fangen einige Grundlagen an, sich zu etablieren. Quell-Plugins bringen Daten _in_ das Gatsby-Datensystem. Dieses Muster kann alle Datenquellen und Datenumwandlungen verarbeiten, die Sie beim Erstellen einer Gatsby-Site benötigen.

## Erstellen Sie eine Liste der Markdown-Dateien Ihrer Site in `src/pages/index.js`

Jetzt müssen Sie eine Liste Ihrer Markdown-Dateien auf der Startseite erstellen. Wie viele 87 Blogs, möchten Sie mit einer Liste von Links auf der Titelseite enden, die auf jeden zeigen 88 Blogeintrag. Mit GraphQL können Sie _query_ für die aktuelle Liste der Abschriften Blog 87 Beiträge, sodass Sie die Liste nicht manuell pflegen müssen.

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

Aber dein einziger Blog-Beitrag sieht ein bisschen einsam aus. Also lasst uns noch einen hinzufügen bei
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

Welches sieht gut aus! Außer ... die Reihenfolge der Beiträge ist falsch.

Dies ist jedoch leicht zu beheben. Wenn Sie eine Verbindung eines bestimmten Typs abfragen, können Sie der GraphQL-Abfrage eine Reihe von Argumenten übergeben. Sie können Knoten `sort` und `filter`, die Anzahl der zu überspringenden Knoten festlegen und das `limit` für die Anzahl der abzurufenden Knoten festlegen. Mit diesen leistungsstarken Operatoren können Sie beliebige Daten auswählen - in dem Format, das Sie benötigen.

Ändern Sie in der GraphQL-Abfrage Ihrer Indexseite `allMarkdownRemark` in `allMarkdownRemark` (sort: {fields: [frontmatter___date], order: DESC}). _Hinweis: Zwischen `frontmatter` und` date` gibt es 3 Unterstriche._ Speichern Sie dies und die Sortierreihenfolge sollte festgelegt sein.

Versuchen Sie, GraphiQL zu öffnen und mit verschiedenen Sortieroptionen zu spielen. Sie können die sortieren `allFile` Verbindung zusammen mit anderen Verbindungen.

Weitere Informationen zu unseren Abfrageoperatoren finden Sie in unserer [GraphQL reference guide.](/docs/graphql-reference/)

## Herausforderung

Versuchen Sie, eine neue Seite mit einem Blogeintrag zu erstellen, und sehen Sie, was mit der Liste der Blogeinträge auf der Startseite geschieht!

## Was kommt als nächstes

Das ist toll! Sie haben gerade eine schöne Indexseite erstellt, auf der Sie Ihre Abschrift abfragen Dateien und Erstellen einer Liste von Blog-Post-Titeln und Auszügen. Sie möchten jedoch nicht nur Auszüge sehen, sondern auch tatsächliche Seiten für Ihre Markdown-Dateien.

Sie können weitere Seiten erstellen, indem Sie die React-Komponenten in `src / pages` platzieren. 
Als Nächstes lernen Sie jedoch, wie Sie _programmatisch_ Seiten aus _Daten_ erstellen. 
Gatsby ist _nicht_ darauf beschränkt, Seiten aus Dateien zu erstellen, wie dies bei vielen statischen Site-Generatoren der Fall ist. 
Mit Gatsby können Sie GraphQL verwenden, um Ihre _Daten_ abzufragen und die Abfrageergebnisse _Seiten_ zuzuordnen - alles zur Erstellungszeit.
Dies ist eine wirklich starke Idee. 
Im nächsten Lernprogramm werden die Auswirkungen und Verwendungsmöglichkeiten erläutert [programmgesteuert Seiten aus Daten erstellen](/tutorial/part-seven/).
