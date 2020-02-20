---
title: Hinzufügen einer Manifest Datei
---

Wenn du einen [Test mit Lighthouse](/docs/audit-with-lighthouse/) ausführst, hast du vielleicht schon die fehlende Bewertung in der Kategorie "Progressive Web App" gesehen. Hier erklären wir dir, wie du diese verbessern kannst.

Aber zuerst, was _sind_ überhaupt PWAs?

PWAs sind normale Webseiten die sich Funktionen moderner Browser zunutze machen um das User Erlebnis so zu verändern, dass die Webseite sich wie eine App anfühlt. Hier kannst du [Google's Übersicht (in Englisch)](https://developers.google.com/web/progressive-web-apps/) lesen, auf der erklärt wird, was die Merkmale und Vorteile von PWAs sind. Hier findest du außerdem unser [Progressive web apps (PWAs) Dokument](/docs/progressive-web-app/), mit dem wir dir zeigen, wie deine Gatsby Seite eine PWA werden kann.

Das Bereitstellen eines Web App Manifests ist eine der drei [grundlegenden Anforderungen an eine PWA](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1).

Zitat [Google](https://developers.google.com/web/fundamentals/web-app-manifest/):

> Das Web App Manifest ist eine einfache JSON Datei, welche dem Browser Informationen über deine Applikation und wie diese sich verhalten soll, wenn sie auf dem Gerät deiner Benutzers 'installiert' ist, bereitstellt.

[Gatsby's Manifest Plugin](/packages/gatsby-plugin-manifest/) konfiguriert Gatsby so, dass eine `manifest.webmanifest` Datei bei jedem Build Prozess der Seite erstellt wird.

## `gatsby-plugin-manifest` verwenden

1.  Installiere das Plugin:

```shell
npm install --save gatsby-plugin-manifest
```

2. Füge deiner Seite ein Favicon unter `src/images/icon.png` hinzu. Das Icon wird benötigt, um alle Bilder für das Manifest zu erstellen. Mehr Informationen hierzu findest du in der Plugin Dokumentation: [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md).

3. Füge das Plugin dem `plugins` Array in deiner `gatsby-config.js` Datei hinzu.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: "GatsbyJS",
        short_name: "GatsbyJS",
        start_url: "/",
        background_color: "#6b37bf",
        theme_color: "#6b37bf",
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: "standalone",
        icon: "src/images/icon.png", // This path is relative to the root of the site.
        // An optional attribute which provides support for CORS check.
        // If you do not provide a crossOrigin option, it will skip CORS for manifest.
        // Any invalid keyword or empty string defaults to `anonymous`
        crossOrigin: `use-credentials`,
      },
    },
  ]
}
```

Das ist alles, was du tun musst, um deiner Gatsby Seite ein Web Manifest hinzuzufügen. Das Beispiel hier spiegelt eine Basiskonfiguration wieder. Für weiterführende Informationen schau dir die [Plugin Dokumentation](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode) an.
