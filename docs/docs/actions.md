---
title: Actions
description: Dokumentation für Actions und wie sie dir helfen State in Gatsby zu manipulieren
jsdoc:
  - "gatsby/src/redux/actions/public.js"
  - "gatsby/src/redux/actions/restricted.js"
contentsHeading: Functions
---

Intern verwendet Gatsby [Redux](http://redux.js.org) um State zu verwalten. Wenn du eine Gatsby API verwendest, bekommst du eine Auswahl von Actions (ähnlich der [bindActionCreators](https://redux.js.org/api/bindactioncreators/) Actions in Redux), welche du dann zum verwalten von State in deiner App verwenden kannst.

Das `actions` Objekt beinhaltet Funktionen, die mit ES6 Objekt Destructuring einzeln extrahiert werden können.

```javascript
// Für die createNodeField Funktion
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
}
```
