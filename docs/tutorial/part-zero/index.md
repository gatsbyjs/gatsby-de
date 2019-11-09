---
title: Setz deine eigene Entwicklungsumgebung auf
typora-copy-images-to: ./
disableTableOfContents: true
---

Bevor du anfängst deine erste Seite mit Gatsby zu bauen, solltest du dich mit einigen Kern-Webtechnologien vertraut machen und sicherstellen, dass alle notwendigen Softwarewerkzeuge installiert sind.

## Mache dich mit der Kommadozeile vertraut

Die Kommandozeile ist eine textbasierte Schnittstelle, die zum Ausführen von Kommandos auf deinem Computer da ist. Diese wird des Öfteren auch
Terminal genannt. In dieser Anleitung werden wir beide Begriffe abwechselnd verwenden. Es ist der Verwendung von Finder auf einem Mac, oder dem
Explorer auf Windows ähnlich. Finder und Explorer sind Beispiele einer grafischen Benutzerobefläche (GUI). Die Kommandozeile ist ein mächtiges, textbasierte Werkzeug, um mit deinem Computer zu interagieren.

Nimm dir einen Moment Zeit, um die Kommandozeilenschnittstelle (CLI) auf deinem Computer zu finden. Abhängig von deinem Betriebssystem, orientiere dich an [**Anleitung für Mac**](http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/), [**Anleitung für Windows**](https://www.quora.com/How-do-I-open-terminal-in-windows) oder [**Anleitung für Linux**](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/).

## Installieren von Homebrew für Node.js

Für die Installation von Gatsby und Node.js, ist die Nutzung von [Homebrew](https://brew.sh/) ratsam. Die Einrichtung am Anfang kann dich vor späteren Kopfzerbrechen bewahren!

Wie installierst, oder verifizierst du Homebrew auf deinem Computer:

1. Öffne dein Terminal.
1. Überprüfe ob Homebrew bereits installiert ist, in dem du `brew -v` ausführst. Du solltest dann "Homebrew" und eine Versionsnummer sehen.
1. Wenn nicht, lade und installiere [Homebrew mit der passenden Anleitung](https://docs.brew.sh/Installation) für dein Betriebssystem (Mac, Linux oder Windows).
1. Nach dem du Homebrew installiert hast, wiederhole Schritt 2 um dies sicherzustellen.

### Mac Nutzer: Installation von Xcode Command Line Tools

1. Öffne dein Terminal.
1. Installiere die Xcode Command Line Tools durch das Ausführen von `xcode-select --install` auf einem Mac.
   1. Wenn dies nicht gelingt, lade dir diese [direkt von der Apple Website](https://developer.apple.com/download/more/) herunter, nach dem du dich mit einem Apple Entwicklerkonto angemeldet hast.
1. Nach der Aufforderung zum Starten der Installation, wirst du erneut Aufgefordert die Softwarelizenz für die zu installierenden Tools zu akzeptieren.

## ⌚ Installiere Node.js und npm

Node.js ist eine Umgebung, die JavaScript außerhalb eines Web-Browsers ausführen kann. Gatsby wurde mit Node.js gebaut. Damit du Startklar für Gatsby bist, sollte eine neuere Version auf deinem Computer installiert sein.

_Hinweis: Gatsby's minimal unterstützte Node.js Version ist Node 8, du kannst aber ruhig eine neuere Version verwenden._

1. Öffne dein Terminal.
1. Führe `brew update` aus, um sicherzustellen, dass du die neueste Version von Homebrew hast.
1. Führe folgenden Befehl aus, um Node und npm in einem Zug zu installieren: `brew install node`

Sobald du die Installationsschritte abgeschlossen hast, stelle sicher dass alles ordentlich installiert wurde:

### Überprüfe deine Node.js Installation

1. Öffne dein Terminal.
2. Führe `node --version` aus. (Wenn du noch nicht mit der Kommandozeile vertraut bist, "Führe `Kommando` aus" bedeutet "Tippe `node --version` in die Kommandozeile ein und drücke die Enter Taste".
Ab hier ist es das, was wir mit "Führe `Kommando` aus" meinen).
3. Führe `npm --version` aus.

Die Ausgabe der einzelnen dieser Kommandos sollte eine Versionsnummer sein. Deine Versionen könnten nicht mit den unten gezeigten übereinstimmen! Wenn die Eingabe dieser Kommandos, dir keine Versionsnummer zeigt, gehe zurück und stelle sicher, dass du Node.js installiert hast.

![Überprüfen von node und npm Versionen im Terminal](01-node-npm-versions.png)

## Installiere Git

Git ist ein freies, Open-Source Versionsverwaltungssystem, welches für schnelle und effiziente Abwicklung von kleinen und großen Projekten gestaltet wurde.
Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. When you install a Gatsby "starter" site, Gatsby uses Git behind the scenes to download and install the required files for your starter. You will need to have Git installed to set up your first Gatsby site.

The steps to download and install Git depend on your operating system. Follow the guide for your system:

- [Install Git on macOS](https://www.atlassian.com/git/tutorials/install-git#mac-os-x)
- [Install Git on Windows](https://www.atlassian.com/git/tutorials/install-git#windows)
- [Install Git on Linux](https://www.atlassian.com/git/tutorials/install-git#linux)

## Using the Gatsby CLI

The Gatsby CLI tool lets you quickly create new Gatsby-powered sites and run commands for developing Gatsby sites. It is a published npm package.

The Gatsby CLI is available via npm and should be installed globally by running `npm install -g gatsby-cli`.

_**Note**: when you install Gatsby and run it for the first time, you'll see a short message notifying you about anonymous usage data that is being collected for Gatsby commands, you can read more about how that data is pulled out and used in the [telemetry doc](/docs/telemetry)._

To see the commands available, run `gatsby --help`.

![Check gatsby commands in terminal](05-gatsby-help.png)

> 💡 If you are unable to successfully run the Gatsby CLI due to a permissions issue, you may want to check out the [npm docs on fixing permissions](https://docs.npmjs.com/getting-started/fixing-npm-permissions), or [this guide](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md).

## Create a Gatsby site

Now you are ready to use the Gatsby CLI tool to create your first Gatsby site. Using the tool, you can download “starters” (partially built sites with some default configuration) to help you get moving faster on creating a certain type of site. The “Hello World” starter you’ll be using here is a starter with the bare essentials needed for a Gatsby site.

1.  Open up your terminal.
2.  Run `gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world`. (_Note: Depending on your download speed, the amount of time this takes will vary. For brevity's sake, the gif below was paused during part of the install_).
3.  Run `cd hello-world`.
4.  Run `gatsby develop`.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./03-create-site.mp4" />
  <p>Sorry! You browser doesn't support this video.</p>
</video>

What just happened?

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

- `new` is a gatsby command to create a new Gatsby project.
- Here, `hello-world` is an arbitrary title — you could pick anything. The CLI tool will place the code for your new site in a new folder called “hello-world”.
- Lastly, the GitHub URL specified points to a code repository that holds the starter code you want to use.

```shell
cd hello-world
```

- This says 'I want to change directories (`cd`) to the “hello-world” subfolder'. Whenever you want to run any commands for your site, you need to be in the context for that site (aka, your terminal needs to be pointed at the directory where your site code lives).

```shell
gatsby develop
```

- This command starts a development server. You will be able to see and interact with your new site in a development environment — local (on your computer, not published to the internet).

### View your site locally

Open up a new tab in your browser and navigate to [**http://localhost:8000**](http://localhost:8000/).

![Check homepage](04-home-page.png)

Congrats! This is the beginning of your very first Gatsby site! 🎉

You’ll be able to visit the site locally at [**_http://localhost:8000_**](http://localhost:8000/) for as long as your development server is running. That’s the process you started by running the `gatsby develop` command. To stop running that process (or to “stop running the development server”), go back to your terminal window, hold down the “control” key, and then hit “c” (ctrl-c). To start it again, run `gatsby develop` again!

**Note:** If you are using VM setup like `vagrant` and/or would like to listen on your local IP address, run `gatsby develop -- --host=0.0.0.0`. Now, the development server listens on both 'localhost' and your local IP.

## Set up a code editor

A code editor is a program designed specifically for editing computer code. There are many great ones out there.

### Download VS Code

Gatsby documentation sometimes includes screenshots that were taken in VS Code, so if you don't have a preferred code editor yet, using VS Code will make sure that your screen looks just like the screenshots in the tutorial and docs. If you choose to use VS Code, visit the [VS Code site](https://code.visualstudio.com/#alt-downloads) and download the version appropriate for your platform.

### Install the Prettier plugin

We also recommend using [Prettier](https://github.com/prettier/prettier), a tool that helps format your code to avoid errors.

You can use Prettier directly in your editor using the [Prettier VS Code plugin](https://github.com/prettier/prettier-vscode):

1.  Open the extensions view on VS Code (View => Extensions).
2.  Search for "Prettier - Code formatter".
3.  Click "Install". (After installation you'll be prompted to restart VS Code to enable the extension. Newer versions of VS Code will automatically enable the extension after download.)

> 💡 If you're not using VS Code, check out the Prettier docs for [install instructions](https://prettier.io/docs/en/install.html) or [other editor integrations](https://prettier.io/docs/en/editors.html).

## ➡️ What’s Next?

To summarize, in this section you:

- Learned about the command line and how to use it
- Installed and learned about Node.js and the npm CLI tool, the version control system Git, and the Gatsby CLI tool
- Generated a new Gatsby site using the Gatsby CLI tool
- Ran the Gatsby development server and visited your site locally
- Downloaded a code editor
- Installed a code formatter called Prettier

Now, move on to [**getting to know Gatsby building blocks**](/tutorial/part-one/).

## References

### Overview of core technologies

It’s not necessary to be an expert with these already — if you’re not, don’t worry! You’ll pick up a lot through the course of this tutorial series. These are some of the main web technologies you’ll use when building a Gatsby site:

- **HTML**: A markup language that every web browser is able to understand. It stands for HyperText Markup Language. HTML gives your web content a universal informational structure, defining things like headings, paragraphs, and more.
- **CSS**: A presentational language used to style the appearance of your web content (fonts, colors, layout, etc). It stands for Cascading Style Sheets.
- **JavaScript**: A programming language that helps us make the web dynamic and interactive.
- **React**: A code library (built with JavaScript) for building user interfaces. It’s the framework that Gatsby uses to build pages and structure content.
- **GraphQL**: A query language that allows you to pull data into your website. It’s the interface that Gatsby uses for managing site data.

### What is a website?

For a comprehensive introduction to what a website is--including an intro to HTML and CSS--check out “[**Building your first web page**](https://learn.shayhowe.com/html-css/building-your-first-web-page/)”. It’s a great place to start learning about the web. For a more hands-on introduction to [**HTML**](https://www.codecademy.com/learn/learn-html), [**CSS**](https://www.codecademy.com/learn/learn-css), and [**JavaScript**](https://www.codecademy.com/learn/introduction-to-javascript), check out the tutorials from Codecademy. [**React**](https://reactjs.org/tutorial/tutorial.html) and [**GraphQL**](http://graphql.org/graphql-js/) also have their own introductory tutorials.

### Learn more about the command line

For a great introduction to using the command line, check out [**Codecademy’s Command Line tutorial**](https://www.codecademy.com/courses/learn-the-command-line/lessons/navigation/exercises/your-first-command) for Mac and Linux users, and [**this tutorial**](https://www.computerhope.com/issues/chusedos.htm) for Windows users. Even if you are a Windows user, the first page of the Codecademy tutorial is a valuable read. It explains what the command line is, not just how to interface with it.

### Learn more about npm

npm is a JavaScript package manager. A package is a module of code that you can choose to include in your projects. If you just downloaded and installed Node.js, npm was installed with it!

npm has three distinct components: the npm website, the npm registry, and the npm command line interface (CLI).

- On the npm website, you can browse what JavaScript packages are available in the npm registry.
- The npm registry is a large database of information about JavaScript packages available on npm.
- Once you’ve identified a package you want, you can use the npm CLI to install it in your project or globally (like other CLI tools). The npm CLI is what talks to the registry — you generally only interact with the npm website or the npm CLI.

> 💡 Check out npm’s introduction, “[**What is npm?**](https://docs.npmjs.com/getting-started/what-is-npm)”.

### Learn more about Git

You will not need to know Git to complete this tutorial, but it is a very useful tool. If you are interested in learning more about version control, Git, and GitHub, check out GitHub's [Git Handbook](https://guides.github.com/introduction/git-handbook/).
