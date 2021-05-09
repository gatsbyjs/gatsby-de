---
title: API Dateien
---

Gatsby verwendet 4 Dateien im Stammverzeichnis Ihres Projekts, um Ihre Site zu konfigurieren und ihr Verhalten zu steuern. Sowohl Sites als auch Plugins können diese Dateien implementieren. Alle diese Dateien sind optional.

- [gatsby-config.js](/docs/api-files-gatsby-config) - Aktiviert Plugins, definiert allgemeine Standortdaten und enthält andere Standortkonfigurationen, die in die GraphQL-Datenschicht von Gatsby integriert sind.
- [gatsby-browser.js](/docs/api-files-gatsby-browser) - Gibt Ihnen die Kontrolle über Gatsbys Verhalten im Browser. Beispiel: Reagieren Sie auf einen Benutzer, der Routen ändert, oder rufen Sie eine Funktion auf, wenn der Benutzer zum ersten Mal eine Seite öffnet.
- [gatsby-node.js](/docs/api-files-gatsby-node) - Ermöglicht es Ihnen, auf Ereignisse im Gatsby-Erstellungszyklus zu reagieren. Beispiel: Dynamisches Hinzufügen von Seiten, Bearbeiten von GraphQL-Knoten beim Erstellen oder Ausführen einer Aktion nach Abschluss eines Builds.
- [gatsby-ssr.js](/docs/api-files-gatsby-ssr) - Zeigt den serverseitigen Renderprozess von Gatsby an, damit Sie steuern können, wie Ihre HTML-Seiten erstellt werden.
