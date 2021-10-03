---
title: "Recipes: Arbeiten mit Themes"
tableOfContentsDepth: 1
---

Ein [Gatsby theme](/docs/themes/what-are-gatsby-themes) abstrahiert die Gatsby-Konfiguration (gemeinsame Funktionalität, Data Sourcing, Design) in ein installierbares Paket. Das bedeutet, dass die Konfiguration und Funktionalität nicht direkt in Ihr Projekt geschrieben wird, sondern versioniert, zentral verwaltet und als Abhängigkeit installiert wird. Du kannst ein Theme nahtlos aktualisieren, Themen zusammenstellen und sogar ein kompatibles Theme gegen ein anderes austauschen.

## Erstellen einer neuen Seite mit Theme

Du hast ein Theme gefunden, das du für dein Projekt verwenden möchtest? Großartig! Du kannst es für die Verwendung konfigurieren, indem du die folgenden Schritte ausführst.

> Wenn du einen Blick auf weitere Theme-Optionen werfen möchtest, schau die [liste der Themes](https://www.npmjs.com/search?q=gatsby-theme) an.

### Voraussetzungen

- Stell sicher, dass du die [Gatsby CLI](/docs/gatsby-cli) installiert hast.

### Schritt-für-Schritt Anleitung

1. Erstelle eine Gatsby Seite

```shell
gatsby new {your-project-name}
```

2. Wechsel den Ordner und installiere das Theme

In diesem Beispiel unser Theme ist `gatsby-theme-blog`. Du kannst das zuvor genannte Theme durch den Namen deines Themes ersetzen.

```shell
cd {your-project-name}
npm install gatsby-theme-blog
```

3. Füge dein Theme zur `gatsby.config.js` hinzu

Befolge die Anweisungen in der README des von dir verwendeten Themes, um festzustellen, welche Konfiguration benötigt wird.

```shell
module.exports = {
  plugins: [
    {
      resolve: `gatsby-theme-blog`,
      options: {
        /*
        - basePath defaults to `/`
        - contentPath defaults to `content/posts`
        - assetPath defaults to `content/assets`
        - mdx defaults to `true`
        */
        basePath: `/blog`,
      },
    },
  ],
}
```

1. Führe `gatsby develop` aus, das Theme sollte unter `http://localhost:8000/{basePath}` verfügbar sein

> Um zu erfahren, wie du ein Design weiter anpassen kannst, sie dir die verfügbaren Pfade auf [Gatsby-theme-blog Dokumentation](https://www.npmjs.com/package/gatsby-theme-blog) an.

### Zusätzliche Ressourcen

- Um zu erfahren, wie du ein Design weiter anpassen kannst, lies die Dokumentation auf [Gatsby theme shadowing](https://www.gatsbyjs.org/docs/themes/shadowing/) durch.

- Du kannst außerdem [mehrere Themes](https://www.gatsbyjs.org/docs/themes/using-multiple-gatsby-themes/) in einem Projekt verwenden.

## Erstellen einer neuen Seite mit einem Theme-Starter

Das Erstellen einer Seite basierend auf einem Starter, der ein Thema konfiguriert, folgt demselben Prozess wie das Erstellen einer Seite basierend auf einem Starter, der **kein** Thema konfiguriert hat. In diesem Beispiel kannst du [das offizielle Gatsby Blog Theme verwenden um eine neue Seite zu erstellen.](https://github.com/gatsbyjs/gatsby-starter-blog-theme).

### Voraussetzungen

- Stell sicher, dass du [Gatsby CLI](/docs/gatsby-cli) installiert hast.

### Schritt-für-Schritt Anleitung

1. Erstelle eine Gatsby Seite

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

2. Starte deine neue Seite:

```shell
cd {your-project-name}
gatsby develop
```

### Zusätzliche Ressourcen

- Erfahre, wie du ein vorhandenes Gatsby-Theme nutzen kannst im [kurzen konzeptuellen Guide](/docs/themes/using-a-gatsby-theme) oder in der detaillierteren [Schrit-für-Schritt Anleitung](/tutorial/using-a-theme).

## Entwickeln eines neuen Themes

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-the-gatsby-theme-workspace-starter-to-begin-building-a-new-theme"
  lessonTitle="Use the Gatsby Theme Workspace Starter to Begin Building a New Theme"
/>

### Voraussetzungen

- Die [Gatsby CLI](/docs/gatsby-cli) installiert
- [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable) installiert

### Schritt-für-Schritt Anleitung

1. Erstelle einen neuen Theme Workspace, indem du den [Gatsby theme workspace starter](https://github.com/gatsbyjs/gatsby-starter-theme-workspace) nutzt:

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-theme-workspace
```

2. Starte deine Beispiel Seite:

```shell
yarn workspace example develop
```

### Zusätzliche Ressourcen

- Folge dem [detaillierten Guide](/docs/themes/building-themes/) zur Nutzung des Gatsby Theme Workspace Starter.
- Lerne dein eigenes Theme zu entwickeln im [Gatsby Theme Authoring Videokurs auf Egghead](https://egghead.io/courses/gatsby-theme-authoring), oder in dem [schriftlichen Tutorial zum Videokurs](/tutorial/building-a-theme).
