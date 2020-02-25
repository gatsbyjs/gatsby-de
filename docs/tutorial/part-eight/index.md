---
title: Eine Seite für das Go Live vorbereiten
typora-copy-images-to: ./
disableTableOfContents: true
---

Wow! Du hast es weit geschafft. Du hast gelernt:

- eine neue Gatsby Seite zu erstellen
- Pages und Components zu erstellen
- Components zu stylen
- Plugins zu installieren
- Datenquellen zu erstellen und zu transformieren 
- GraphQL zu nutzen, um Daten abzufragen
- programmatisch Pages aus deinen Daten zu erstellen 

In diesem letzten Abschnitt wirst du die gängigen Schritte durchlaufen, um deine Seite für das Go Live vorzubereiten. Dazu werden wir das mächtige Diagnose-Tool [Lighthouse](https://developers.google.com/web/tools/lighthouse/) benutzen. Außerdem werden wir noch einige weitere, häufig genutzte Plugins installieren.

## Audit mit Lighthouse

Ein Zitat von der [Lighthouse Website](https://developers.google.com/web/tools/lighthouse/):

> Lighthouse is an open-source, automated tool for improving the quality of web pages. You can run it against any web page, public or requiring authentication. It has audits for performance, accessibility, progressive web apps (PWAs), and more.

Lighthouse ist Teil der Chrome DevTools. Ein Lighthouse Audit hilft dir dabei, gängige Fehler zu finden und schlägt auch direkt Lösungen zur Behebung dieser Fehler vor. Diese Lösungen in dein Projekt einzubauen ist ein ideal, um deine Seite für das Go Live vorzubereiten. Lighthouse gibt dir die Sicherheit, dass deine Seite so schnell und barrierefrei wie möglich ist.

Versuchs einfach!

Als erstes musst du einen Production Build von deiner Gatsby Seite erstellen. Der Gatsby Development Server optimiert dein Projekt zwar für die Entwicklung und obwohl das Projekt so aussieht, als wäre es "production-ready", ist es dafür nicht optimiert.

### ✋ Erstelle einen Production Build

1.  Stoppe den Development Server (sollte er laufen) und führe folgenden Befehl aus:

```shell
gatsby build
```

> 💡 Wie du in [Teil 1](/tutorial/part-one/) gelernt hast, erstellt dieser Befehl einen Production Build deiner Seite und stellt die statischen Dateien im `public` Ordner zur Verfügung.

2.  Um den Production Build lokal anzusehen, führe folgenden Befehl aus:

```shell
gatsby serve
```

Wenn das Skript läuft, kannst du deine Seite unter `http://localhost:9000` betrachten.

### Führe den Lighthouse Audit durch

Du führst jetzt deinen ersten Test mit Lighthouse durch.

1.  Falls noch nicht geschehen, öffne deine Seite in einem Chrome Inkognito Fenster, sodass keine Chrome Extenstions den Test stören. Öffne danach deine Chrome DevTools.

2.  Klicke auf das "Audits" Tab, in dem du folgendes sehen solltest:

![Lighthouse audit start](./lighthouse-audit.png)

3.  Klicke auf "Perform an audit..." (Es sollten dabei standardmäßig alle Audit Typen ausgewählt sein). Klicke anschließend auf "Run audit". (Es dauert ca. eine Minute, bis der Audit fertig ist). Wenn der Audit fertig ist, solltest du deine Ergebisse sehen:

![Lighthouse audit results](./lighthouse-audit-results.png)

Wie du sehen kannst, sind die Performance Ergebnisse von Gastby bereits "out of the box", d.h. ohne Optimierungen, ausgezeichnet. Dir fehlen allerdings einige Punkte in den Bereichen PWA, Accessibility (Barrierefreiheit), Best Practices und SEO um dein Gesamtergebnis noch zu steigern (und dabei deine Seite benutzer- und suchmaschinenfreundlicher zu machen).

## Füge eine manifest Datei hinzu

Es sieht so aus, als wären deine Ergebnisse im Bereich Progressive Web App (PWA) noch verbesserungswürdig. Lass uns das angehen.

Aber zunächst: was _sind_ PWAs eigentlich _genau_?

PWAs sind normale Webseiten, die sich moderne Browserfunktionen zu Nütze machen, um die Web Experience mit app-ähnlichen Features und Vorteilen zu ergänzen. Sieh dir dazu die [Übersicht auf Google](https://developers.google.com/web/progressive-web-apps/) an, um heruaszufinden, was genau eine PWA ausmacht.

Ein Web App Manifest zu haben, ist eine von drei akzeptierten [Grundvoraussetzungen für PWAs](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1).

Bei [Google](https://developers.google.com/web/fundamentals/web-app-manifest/) findet man folgenden Satz:

> The web app manifest is a simple JSON file that tells the browser about your web application and how it should behave when 'installed' on the user's mobile device or desktop.

Das [Gatsby's manifest plugin](/packages/gatsby-plugin-manifest/) konfiguriert Gatsby so, dass es bei jedem Build der Seite ein `manifest.webmanifest` erstellt.

### ✋ Das `gatsby-plugin-manifest` verwenden

1.  Installiere das plugin:

```shell
npm install --save gatsby-plugin-manifest
```

2. Füge ein favicon für dein Projekt unter `src/images/icon.png` hinzu. Du kannst beispielhaft [dieses Icon](https://raw.githubusercontent.com/gatsbyjs/gatsby/master/docs/tutorial/part-eight/icon.png) verwenden, falls du keins zur Hand hast. Für weitere Informationen schau dir die Dokumentation vom [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md) an.

3. Füge das Plugin in das `plugins` Array in deiner `gatsby-config.js` Datei hinzu.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Ermöglicht das "Zum Homebildschirm hinzufügen" und deaktiviert die Browser UI (inkl. Zurück-Button)
        // weitere Infos: https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // Dies ist ein relativer Pfad zum Root deines Projekts.
      },
    },
  ]
}
```

Das ist alles, was du brauchst, um ein Web manifest zu deinem Gatsby Projekt hinzuzufügen. Es handelt sich hierbei um die Grundkonfiguration. Für weitere Einstellungsmöglichkeiten, schau in die [Plugin Dokumentation](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode).

## Mach dein Projekt offline verfügbar

Eine weitere Voraussetzung dafür, dass dein Projekt als PWA erkannt wird, ist das Nutzen eines sog. [Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API). Ein Service Worker läuft im Hintergrund und entscheidet, ob er Inhalte aus dem Cache oder aus dem Netzwerk anzeigen soll. Dadurch funktioniert deine Seite unabhängig davon, ob dein Client im Internet verbunden ist, oder nicht.

Das [Gatsby offline plugin](/packages/gatsby-plugin-offline/) erstellt einen Service Worker für dein Projekt, lässt deine Seite dadurch auch offline funktionieren und rüstet sie gegen schlechte Netzwerkbedingungen.

### ✋ `gatsby-plugin-offline` verwenden

1.  Installiere das Plugin:

```shell
npm install --save gatsby-plugin-offline
```

2.  Füge das Plugin in das `plugins` Array in deiner `gatsby-config.js` Datei hinzu.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Ermöglicht das "Zum Homebildschirm hinzufügen" und deaktiviert die Browser UI (inkl. Zurück Button)
        // weitere Infos: https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // Dies ist ein relativer Pfad zum Root deines Projekts.
      },
    },
    // highlight-next-line
    `gatsby-plugin-offline`,
  ]
}
```

Das ist alles, was du brauchst, um Service Worker in dein Gatsby Projekt einzubinden.

> 💡 Das gatsby-plugin-offline sollte _nach_ dem gatsby-plugin-manifest deklariert werden, sodass das Offlineplugin auch das `manifest.webmanifest` cachen kann.

## Füge Metadaten hinzu

Metadaten auf den Seiten -- dazu gehören bspw. Seitentitel oder -beschreibungen -- sind ein Schlüssel dafür, dass Suchmaschinen wie Google deinen Inhalt verstehen und entscheiden können, wann sie deine Seite in den Suchergebnissen zeigen sollen. 

[React Helmet](https://github.com/nfl/react-helmet) ist ein Package, das dir ein React Interface zur Verfügung stellt, mit dem du dein [Document Head](https://developer.mozilla.org/de-DE/docs/Web/HTML/Element/head) verwalten kannst.

Das [React Helmet Plugin](/packages/gatsby-plugin-react-helmet/) von Gatsby bereitet die im React Helmet generierten Daten für das serverseitige Rendern der Seite vor. Wird das Plugin benutzt, werden die Attribute, die du in React Helmet definierst in die statischen HTML Dateien geschrieben, die Gatsby für dich beim Build generiert.

### ✋ `React Helmet` und `gatsby-plugin-react-helmet` verwenden

1.  Installiere beide Packages:

```shell
npm install --save gatsby-plugin-react-helmet react-helmet
```

2.  Stelle sicher, dass du in deinem `siteMetadata` Objekt einen `author` und eine `description` angegeben hast. Füge außerdem das Plugin in das `plugins` Array in deiner `gatsby-config.js` Datei hinzu.

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
    // highlight-start
    description: `A simple description about pandas eating lots...`,
    author: `gatsbyjs`,
    // highlight-end
  },
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Ermöglicht das "Zum Homebildschirm hinzufügen" und deaktiviert die Browser UI (inkl. Zurück Button)
        // weitere Infos: https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // Dies ist ein relativer Pfad zum Root deines Projekts.
      },
    },
    `gatsby-plugin-offline`,
    // highlight-next-line
    `gatsby-plugin-react-helmet`,
  ],
}
```

3.  Erstelle eine Datei namens `seo.js` im Ordner `src/components` und schreibe folgendes in die Datei:

```jsx:title=src/components/seo.js
import React from "react"
import PropTypes from "prop-types"
import Helmet from "react-helmet"
import { useStaticQuery, graphql } from "gatsby"

function SEO({ description, lang, meta, title }) {
  const { site } = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
            description
            author
          }
        }
      }
    `
  )

  const metaDescription = description || site.siteMetadata.description

  return (
    <Helmet
      htmlAttributes={{
        lang,
      }}
      title={title}
      titleTemplate={`%s | ${site.siteMetadata.title}`}
      meta={[
        {
          name: `description`,
          content: metaDescription,
        },
        {
          property: `og:title`,
          content: title,
        },
        {
          property: `og:description`,
          content: metaDescription,
        },
        {
          property: `og:type`,
          content: `website`,
        },
        {
          name: `twitter:card`,
          content: `summary`,
        },
        {
          name: `twitter:creator`,
          content: site.siteMetadata.author,
        },
        {
          name: `twitter:title`,
          content: title,
        },
        {
          name: `twitter:description`,
          content: metaDescription,
        },
      ].concat(meta)}
    />
  )
}

SEO.defaultProps = {
  lang: `en`,
  meta: [],
  description: ``,
}

SEO.propTypes = {
  description: PropTypes.string,
  lang: PropTypes.string,
  meta: PropTypes.arrayOf(PropTypes.object),
  title: PropTypes.string.isRequired,
}

export default SEO
```

Dieser Code erstellt sinnvolle Defaultwerte für die meisten Metadaten Tags und erstellt ein `<SEO>` Component, das du im Rest deines Projekts verwenden kannst. Nicht schlecht, oder?

4.  Du kannst jetzt das neue `<SEO>` Component in deinen Templates und Seiten benutzen und ihm `props` übergeben. So kannst du es bspw. zu deinem `blog-post.js` Template hinzufügen:

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"
// highlight-next-line
import SEO from "../components/seo"

export default ({ data }) => {
  const post = data.markdownRemark
  return (
    <Layout>
      // highlight-start
      <SEO title={post.frontmatter.title} description={post.excerpt} />
      // highlight-end
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
    </Layout>
  )
}

export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
      // highlight-next-line
      excerpt
    }
  }
`
```

Das oben gezeigte Beispiel basiert auf dem [Gatsby Starter Blog](/starters/gatsbyjs/gatsby-starter-blog/). Mit den props im `<SEO>` Component kannst du die Metadaten dynmaisch für einen Blogpost ändern. In diesem Fall wird der `title` und das `excerpt` aus der Markdown Datei des Blogposts (falls vorhanden) statt des Defaultwerts aus den `siteMetadata` Daten benutzt. 

Wenn du jetzt den Lighthouse Audit wie oben beschrieben nochmal durchführst, solltest du ein Ergebnis nah an 100 bekommen. Vielleicht sogar 100 glatt!

> 💡 Für weitere Infos und Beispiele schau dir [Ein SEO Component hinzufügen](/docs/add-seo-component/) und die [React Helmet Dokumentation](https://github.com/nfl/react-helmet#example) an.

## Weitere Verbesserungen 

In diesem Abschnitt haben wir dir ein paar Gatsby-spezifische Tools gezeigt, mit denen du deine Performance verbessern kannst und deine Seite für das Go Live vorbereiten kannst.

Lighthouse ist ein großartiges Tool für Verbesserungen und um zu lernen. Schau dir das detaillierte Feedback von Lighthouse an und mach deine Seite noch besser!

## Nächste Schritte

### Die offizielle Dokumentation

- [Die offizielle Dokumentation](https://www.gatsbyjs.org/docs/): Sieh dir unsere offizielle Dokumentation zu den Themen _[Schnellstart](https://www.gatsbyjs.org/docs/quick-start/)_ , _[Detallierte Guides](https://www.gatsbyjs.org/docs/preparing-your-environment/)_, _[API Referenz](https://www.gatsbyjs.org/docs/gatsby-link/)_ u.v.m. an.

### Offizielle Plugins

- [Offizielle Plugins](https://github.com/gatsbyjs/gatsby/tree/master/packages): Eine komplette Liste aller durch das Gatsby Team gepflegten Plugins.

### Offizielle Starterpackages

1.  [Gatsby's Default Starter](https://github.com/gatsbyjs/gatsby-starter-default): Mit diesem Package gelingt dir ein schneller Start. Es kommt mit den meisten Gatsby Konfigurationsdateien, die du für die Entwicklung brauchen könntest _[Live Beispiel](https://gatsbyjs.github.io/gatsby-starter-default/)_
2.  [Gatsby's Blog Starter](https://github.com/gatsbyjs/gatsby-starter-blog): Ein Starterpackage für einen genial-schnellen Blog. _[Live Beispiel](https://gatsbyjs.github.io/gatsby-starter-blog/)_
3.  [Gatsby's Hello-World Starter](https://github.com/gatsbyjs/gatsby-starter-hello-world): Ein Starterpackage mit dem absoluten Minimum an Dateien, die Gatsby benötigt _[Live Beispiel](https://gatsby-starter-hello-world-demo.netlify.com/)_

## Das wars, Leute

Naja, nicht ganz. Aber zumindest für dieses Tutorial. Es gibt [weiterführende Tutorials](/tutorial/additional-tutorials/), in denen du andere, konkrete Anwendungsbeispiele findest.

Das ist nur der Anfang, bleib dran!

- Hast du ein cooles Gatsby Projekt erschaffen? Teile es auf Twitter mit dem Tag [#buildwithgatsby](https://twitter.com/search?q=%23buildwithgatsby), und [@erwähne uns](https://twitter.com/gatsbyjs)!
- Hast du einen Blogpost über Gatsby geschrieben? Teile auch diesen!
- Trage zu Gatsby bei! Schau dir unsere [offenen Issues](https://github.com/gatsbyjs/gatsby/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22) im Gastby Github Repository an und [werde ein offizieller Contributor](/contributing/how-to-contribute/).

Schau dir diese ["how to contribute"](/contributing/how-to-contribute/) Dokumentation für weitere Anregungen an!

Wir können es kaum erwarten, zu sehen, was DU beizutragen hast 😄.
