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

Du has auch mit Hilfe der CSS Modules, Styling Components erkundet.

## Was beinhaltet dieses Tutorial?

In den nächsten vier Teilen des Tutorials (inklusive diesen) wirst du in die Datenschicht von Gatsby eintauchen, welche ein mächtiges Feature von Gatsby darstellt und dich mit Leichtigkeit Seiten aus Markdown, WordPress, Headless CMS, sowie vielen anderen Datenquellen, erstellen lässt.

**HINWEIS:** Gatsby’s Datenschicht wird mit GraphQL betrieben. Für ein ausführliches
über GraphQL, empfehlen wir [How to GraphQL](https://www.howtographql.com/).

## Daten in Gatsby

Eine Gatsby Website hat vier Teile: HTML, CSS, JS und Daten. Die erste Hälfte
des Tutorials fokussierte sich auf die ersten drei. Nun, lass uns lernen wie
Daten in Gatsby Websites verwendet werden können.

**Was sind Daten?**

Eine sehr informatikfokussierte Antwort würde lauten: Daten sind Dinge wie `"Strings"`,
Integers (`42`), Objekte (`{ pizza: true }`), etc.

Für den Zweck des Arbeitens mit Gatsby ist jedoch "Alles was außerhalb einer React-Komponente lebt",
eine viel nützlichere Antwort.

Bisher, has du *direkt* in den Komponenten Text geschrieben und Bilder hinzugefügt.
Dies ist eine *hervorragende* Möglichkeit Websites zu bauen. Nichtsdestotrotz, willst
du manchmal Daten *außerhalb* der Komponenten speichern und dann diese Daten *in* die
Komponente bei Bedarf reinbringen.

Wenn du eine Seite mit WordPress (so haben andere Beitragende
eine schöne Oberfläche für das Hinzufügen & Warten der Inhalte) und Gatsby baust,
sind die *Daten* für die Website (Seiten und Beiträge) in WordPress und du *ziehst*
diese Daten bei Bedarf in deine Komponenten.

Dateitypen wie Markdown, CSV, etc. sowie Datenbanken und APIs aller Arten, können
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
2.  Falls es keine gibt, lese die [Plugin Authoring](/docs/creating-plugins/) Anleitung und ziehe es in Erwägung dein eigenes Plugin zu bauen!

### How Gatsby's data layer uses GraphQL to pull data into components

There are many options for loading data into React components. One of the most
popular and powerful of these is a technology called
[GraphQL](http://graphql.org/).

GraphQL was invented at Facebook to help product engineers _pull_ needed data into
components.

GraphQL is a **q**uery **l**anguage (the _QL_ part of its name). If you're
familiar with SQL, it works in a very similar way. Using a special syntax, you describe
the data you want in your component and then that data is given
to you.

Gatsby uses GraphQL to enable components to declare the data they need.

## Create a new example site

Create another new site for this part of the tutorial. You're going to build a Markdown blog called "Pandas Eating Lots". It's dedicated to showing off the best pictures and videos of pandas eating lots of food. Along the way, you'll be dipping your toes into GraphQL and Gatsby's Markdown support.

Open a new terminal window and run the following commands to create a new Gatsby site in a directory called `tutorial-part-four`. Then navigate to the new directory:

```shell
gatsby new tutorial-part-four https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-four
```

Then install some other needed dependencies at the root of the project. You'll use the Typography theme
"Kirkham", and you'll try out a CSS-in-JS library, ["Emotion"](https://emotion.sh/):

```shell
npm install --save gatsby-plugin-typography typography react-typography typography-theme-kirkham gatsby-plugin-emotion @emotion/core
```

Set up a site similar to what you ended with in [Part Three](/tutorial/part-three). This site will have a layout component and two page components:

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
        Pandas Eating Lots
      </h3>
    </Link>
    <Link
      to={`/about/`}
      css={css`
        float: right;
      `}
    >
      About
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
    <h1>Amazing Pandas Eating Things</h1>
    <div>
      <img
        src="https://2.bp.blogspot.com/-BMP2l6Hwvp4/TiAxeGx4CTI/AAAAAAAAD_M/XlC_mY3SoEw/s1600/panda-group-eating-bamboo.jpg"
        alt="Group of pandas eating bamboo"
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
    <h1>About Pandas Eating Lots</h1>
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
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

`gatsby-config.js` (must be in the root of your project, not under src)

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

Add the above files and then run `gatsby develop`, per usual, and you should see the following:

![start](start.png)

You have another small site with a layout and two pages.

Now you can start querying 😋

## Your first GraphQL query

When building sites, you'll probably want to reuse common bits of data -- like the _site title_ for example. Look at the `/about/` page. You'll notice that you have the site title (`Pandas Eating Lots`) in both the layout component (the site header) as well as in the `<h1 />` of the `about.js` page (the page header).

But what if you want to change the site title in the future? You'd have to search for the title across all your components and edit each instance. This is both cumbersome and error-prone, especially for larger, more complex sites. Instead, you can store the title in one location and reference that location from other files; change the title in a single place, and Gatsby will _pull_ your updated title into files that reference it.

The place for these common bits of data is the `siteMetadata` object in the `gatsby-config.js` file. Add your site title to the `gatsby-config.js` file:

```javascript:title=gatsby-config.js
module.exports = {
  // highlight-start
  siteMetadata: {
    title: `Title from siteMetadata`,
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

Restart the development server.

### Use a page query

Now the site title is available to be queried; Add it to the `about.js` file using a [page query](/docs/page-query):

```jsx:title=src/pages/about.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-next-line
export default ({ data }) => (
  <Layout>
    <h1>About {data.site.siteMetadata.title}</h1> {/* highlight-line */}
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
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

It worked! 🎉

![Page title pulling from siteMetadata](site-metadata-title.png)

The basic GraphQL query that retrieves the `title` in your `about.js` changes above is:

```graphql:title=src/pages/about.js
{
  site {
    siteMetadata {
      title
    }
  }
}
```

> 💡 In [part five](/tutorial/part-five/#introducing-graphiql), you'll meet a tool that lets us interactively explore the data available through GraphQL, and help formulate queries like the one above.

Page queries live outside of the component definition -- by convention at the end of a page component file -- and are only available on page components.

### Use a StaticQuery

[StaticQuery](/docs/static-query/) is a new API introduced in Gatsby v2 that allows non-page components (like your `layout.js` component), to retrieve data via GraphQL queries.
Let's use its newly introduced hook version — [`useStaticQuery`](/docs/use-static-query/).

Go ahead and make some changes to your `src/components/layout.js` file to use the `useStaticQuery` hook and a `{data.site.siteMetadata.title}` reference that uses this data. When you are done, your file will look like this:

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
        About
      </Link>
      {children}
    </div>
    // highlight-start
  )
}
// highlight-end
```

Another success! 🎉

![Page title and layout title both pulling from siteMetadata](site-metadata-two-titles.png)

Why use two different queries here? These examples were quick introductions to
the query types, how they are formatted, and where they can be used. For now,
keep in mind that only pages can make page queries. Non-page components, such as
Layout, can use StaticQuery. [Part 7](/tutorial/part-seven/) of the tutorial explains these in greater
depth.

But let's restore the real title.

One of the core principles of Gatsby is that _creators need an immediate connection to what they're creating_ ([hat tip to Bret Victor](http://blog.ezyang.com/2012/02/transcript-of-inventing-on-principle/)). In other words, when you make any change to code you should immediately see the effect of that change. You manipulate an input of Gatsby and you see the new output showing up on the screen.

So almost everywhere, changes you make will immediately take effect. Edit the `gatsby-config.js` file again, this time changing the `title` back to "Pandas Eating Lots". The change should show up very quickly in your site pages.

![Both titles say Pandas Eating Lots](pandas-eating-lots-titles.png)

## What's coming next?

Next, you'll be learning about how to pull data into your Gatsby site using
GraphQL with source plugins in [part five](/tutorial/part-five/) of the
tutorial.
