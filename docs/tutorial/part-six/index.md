---
title: Transformer-Plugins
typora-copy-images-to: ./
disableTableOfContents: true
---

> Dieses Tutorial ist Teil einer Reihe über die Datenschicht von Gatsby. Stell bitte sicher, dass du zunächst [Teil 4](/tutorial/part-four/) und [Teil 5](/tutorial/part-five/) durchgegangen bist, bevor du hier fortfährst.

## Um was geht es in diesem Tutorial?

Das vorherige Tutorial hat gezeigt, wie Quellen-Plugins Daten _in_ Gatsbys Datensystem bringen. In diesem Tutorial erfährst du, wie Transformer-Plugins den Inhalt von Quellen-Plugins _transformieren_. Die Kombination aus Quellen-Plugins und Transformer-Plugins kann alle Datenquellen und -transformationen verarbeiten, die beim Erstellen einer Gatsby-Site benötigt werden.

## Transformer plugins

Oft ist das Format der Daten, die von Quellen-Plugins zurückgegeben werden, nicht das, was du brauchst, um deine Website zu erstellen. Mit dem Dateisystems Quellen-Plugin kannst du Daten _über_ Dateien abfragen. Aber was ist, wenn du Daten _aus_ Dateien abfragen möchtest?

Um dies zu ermöglichen, unterstützt Gatsby Transformer-Plugins, welche die Daten von Quellen-Plugins entgegen nehmen und in etwas anderes _transformieren_ können.

Zum Beispiel Markdown. Markdown schreibt sich leicht, aber wenn du eine Website baust, müssen Sie die Markdown-Dateien in HTML umgewandelt werden.

Füge deiner Website unter eine Markdown-Datei unter
`src/pages/sweet-pandas-eating-sweets.md` hinzu. (Dies wird dein erster Markdown Blogbeitrag)
und lerne, wie mit Hilfe von Transformer-Plugins und GraphQL, die Daten in HTML umwandelt werden.

```markdown:title=src/pages/sweet-pandas-eating-sweets.md
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Pandas are really sweet.

Here's a video of a panda eating sweets.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

Wenn du die Datei gespeichert hast, öffne erneut `/my-files/` - die neu erstellte Markdown-Datei befindet sich in der Tabelle. Dies ist eine sehr leistungsstarke Funktion von Gatsby. Wie im vorherigen Beispiel `siteMetadata` können Quellen-Plugins Daten live neu laden.
`gatsby-source-filesystem` sucht kontinuierlich nach neuen Dateien und führt Ihre Abfragen erneut aus, wenn Dateien neu erstellt oder verändert wurden.

Füge nun ein Transformer-Plugin hinzu, das Markdown-Dateien umwandeln kann:

```shell
npm install --save gatsby-transformer-remark
```

Füge dann, wie gewohnt, in die `gatsby-config.js` Folgendes ein:

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

Starte den Entwicklungsserver neu, aktualisiere GraphiQL (oder öffne es erneut), und schau dir dann die Autovervollständigungen an:

![markdown-autocomplete](markdown-autocomplete.png)

Wähle `allMarkdownRemark` erneut aus und führe es erneut, wie für `allFile`, aus. Du wirst dort die Markdown-Datei sehen, die du gerade hinzugefügt hast. Schau dir die Felder, die auf dem `MarkdownRemark` Knoten verfügbar sind, an.

![markdown-query](markdown-query.png)

Super! Hoffentlich etablieren sich damit die ersten Grundlagen. Quellen-Plugins bringen Daten _in_ das Gatsby-Datensystem. Mit diesem Schema können alle Datenquellen und -umwandlungen verarbeitet werden, die beim Erstellen einer Gatsby Website benötigt werden.

## Erstell eine Liste der Markdown-Dateien deiner Website in `src/pages/index.js`

Jetzt sollst du eine Liste deiner Markdown-Dateien auf der Startseite erstellen. Wie in vielen Blogs, soll auf der Startseite eine Liste von Links, die auf jeden Blogeintrag verlinken, angezeigt werden. Mit GraphQL kannst du _query_ für eine aktuelle Liste von Blog-Beiträge abfragen, sodass die Liste nicht manuell gepflegt werden muss.

Wie schon bei der Seite `src/pages/my-files.js` ersetze nun in `src/pages/index.js` mit dem Folgenden, um eine eine GraphQL-Abfrage, mit HTML und CSS, hinzuzufügen.

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

Dein Blog-Beitrag sieht aber ein bisschen einsam aus, also lass uns noch einen hinzufügen in
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

Das Ergebnis sieht gut aus! Außer, dass die Reihenfolge der Beiträge nicht stimmt.

Dies lässt jedoch leicht beheben. Um eine Verbindung eines bestimmten Typs abzufragen, kannst du der GraphQL-Abfrage eine Reihe von Argumenten übergeben. Du kannst den Knoten mit `sort` und `filter`, die Anzahl der zu überspringenden Knoten und das `limit` für die Anzahl der abzurufenden Knoten festlegen. Mit diesen leistungsstarken Operatoren kannst du beliebige Daten abfragen - in dem Format, das du benötigst.

Ändern Sie in der GraphQL-Abfrage Ihrer Indexseite `allMarkdownRemark` in `allMarkdownRemark` (sort: {fields: [frontmatter___date], order: DESC}). _Hinweis: Zwischen `frontmatter` und` date` gibt es 3 Unterstriche._ Speichern Sie die Abfrage und die Sortierreihenfolge sollte nun richtig sein.

Versuchen Sie, GraphiQL zu öffnen und mit verschiedenen Sortieroptionen zu spielen. `allFile` lässt sich mit mit anderen Verbindungen sortieren.

Weitere Informationen zu unseren Abfrageoperatoren finden Sie in unserer [GraphQL reference guide.](/docs/graphql-reference/)

## Herausforderung

Versuche, eine neue Seite mit einem Blogeintrag zu erstellen, und sieh, was mit der Liste der Blogbeiträge auf der Startseite passiert!

## Was kommt als nächstes

Das ist toll! Du hast in diesem Tutoial eine schöne Startseite erstellt, auf der deine abgefragten Dateien als eine Liste von Blogbeitrags-Titeln und Text-Auszügen dargestellt werden. Du willst jedoch nicht nur die Text-Auszüge sehen, sondern auch die tatsächlichen Seiten für deine Markdown-Dateien.

Du kannst weitere Seiten erstellen, indem du React-Komponenten in `src/pages` erstellst. 
Als Nächstes lernst du, wie sich _programmatisch_ Seiten aus _Daten_ erstellen lassen. 
Gatsby ist allerdings _nicht_ darauf beschränkt, Seiten aus Dateien zu erstellen, wie dies bei vielen statischen Site-Generatoren der Fall ist. 
Mit Gatsby kannst du GraphQL verwenden, um deine _Daten_ abzufragen und die Ergebnisse _Seiten_ zuzuordnen - alles zur Erstellungszeit.
Dies ist eine wirklich mächtige Idee. 
Im nächsten Tutorial werden die Vorraussetzungen und Möglichkeiten erläutert, mit denen [Seiten programmatisch aus Daten erstellt werden](/tutorial/part-seven/) können.
