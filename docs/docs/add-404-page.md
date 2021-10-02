---
title: Hinzufügen einer 404 Seite
---

Um eine 404 Seite zu erstellen, füge eine Seite, die dem Regex `^\/?404\/?$` (`/404/`, `/404`, `404/` oder `404`) entspricht. Üblicherweise erstellst du dafür eine React Komponente in `src/pages/404.js`.

Gatsby stellt sicher, dass deine 404 Seite als `404.html` erstellt wird, da die meisten statischen Hosting Plattformen diese als Standard verwenden. Wenn du deine Seite anderweitig hostest, musst du eine individuelle Regel erstellen, um die Seite für 404 Fehler aufrufbar zu machen.

Da Gatsby diese Seite standardmäßig für dich erstellt, ist es nicht notwendig diese in `gatsby-node.js` zu konfigurieren.

Beim Entwickeln mit `gatsby develop` nutzt Gatsby eine eigene 404 Seite, welche deine überschreibt. Du kannst deine 404 Seite allerdings ansehen, indem du auf "Preview custom 404 page" klickst um sicherzustellen, dass alles wie gewünscht funktioniert.

Der untenstehende Screenshot zeigt die Standard 404 Seite, die Gatsby für dich erstellt. Er listet auch alle Seiten der Webseite auf.
![Gatsby Standard 404 Seite](./images/gatsby-default-404.png)

Der Screenshot zeigt eine individuelle 404 Seite:
![Individuelle 404 Seite](./images/gatsby-custom-404.png)
