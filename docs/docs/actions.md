---
title: Actions
description: Dokumentation für Actions und wie diese helfen State in Gatsby zu manipulieren
jsdoc:
  - "gatsby/src/redux/actions/public.js"
  - "gatsby/src/redux/actions/restricted.js"
contentsHeading: Functions
---

Gatsby verwendet [Redux](http://redux.js.org) unter der Haube um State zu verwalten. Wenn du eine Gatsby API verwendest bekommst du eine Auswahl von Actions (ähnlich der [bindActionCreators](https://redux.js.org/api/bindactioncreators/) Actions in Redux), welche du verwenden kannst um State in deiner App zu verwalten.

Das `actions` Objekt beinhaltet Funktionen die mit ES6 Objekt Destructuring einzeln bereitgestellt werden können.

```javascript
// Für die createNodeField Funktion
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
}
```
