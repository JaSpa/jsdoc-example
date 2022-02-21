# Installation und Nutzung von JSDoc

* **Website:** [jsdoc.app][]
* **GitHub:** [jsdoc/jsdoc][]

[jsdoc.app]: https://jsdoc.app/index.html
[jsdoc/jsdoc]: https://github.com/jsdoc/jsdoc


*"Die Shell" meint im Folgenden unter macOS das Terminal und unter Windows cmd.exe oder die Windows PowerShell.*


## Installation

### NPM

* JSDoc wird über [NPM][] installiert, ein Paketmanager für JavaScript
* NPM benötigt [node.js][] um JavaScript lokal auszuführen

* **Windows, macOS:** Einfachste Installation von node.js und NPM über den aktuellen [LTS Installer](installation-node). **Wichtig:** LTS installieren, nicht die "aktuelleste" Version, rät die [NPM-Website](npm-bin-win_mac).
* **Linux:** Siehe Instruktionen auf der [NPM-Website](npm-bin-linux)

[NPM]: https://npmjs.com
[installation-node]: https://nodejs.org/en/download/
[node.js]: https://nodejs.org/en/
[npm-bin-win_mac]: https://docs.npmjs.com/downloading-and-installing-node-js-and-npm#os-x-or-windows-node-installers
[npm-bin-linux]: https://docs.npmjs.com/downloading-and-installing-node-js-and-npm#linux-or-other-operating-systems-node-installers


## JSDoc

[*Referenz*][installation-jsdoc]

[installation-jsdoc]: https://github.com/jsdoc/jsdoc#installation-and-usage

Zwei Möglichkeiten der Installation

1) global
1) pro Projekt

| Global                                                    | Projekt                                                                      |
|-----------------------------------------------------------|------------------------------------------------------------------------------|
| `+` Simpel                                                | `+` Jeder arbeitet garantiert mit der gleichen Version                       |
| `-` Jeder muss dran denken, das Template zu installieren  | `+` Einfache Verwendung von Templates                                        |
| `-` Kann Versionskonflikte zwischen Personen ergeben      | `-` Initiale Einrichtung erfordert etwas mehr Arbeit                         |

*Ich empfehle die Installation je Projekt.*


### Globale Installation

Jeder Nutzer muss (einmalig) in der Shell den Befehl

```sh
npm install --global jsdoc
```

ausführen. Bei macOS mit `sudo`, unter Windows die Shell als Admin öffnen.

Danach werden keine Admin/Root-Privilegien mehr benötigt. Das Kommando

```sh
jsdoc --version
```

sollte erfolgreich Informationen über die installierte Version ausgeben.

Ein späteres Update ist möglich über

```sh
npm update --global jsdoc
```

Bei dieser Installationsmethode kann es passieren, dass unterschiedliche Personen mit verschiedenen Versionen von JSDoc arbeiten.


### Installation je Projekt

#### Erstinstallation

**Folgende Schritte müssen nur von *einer* Person pro Projekt ausgeführt und committet werden.**

Im Wurzelverzeichnis die Datei `package.json` mit (mindestens) folgendem Inhalt erstellen:

```json
{
  "name": "",
  "description": "",
  "version": "0.1.0"
}
```

(Die Werte haben keine tiefere Bedeutung, die Schlüssel müssen für den Zweck der JSDoc-Installation nur vorhanden sein.)

Eine Person führt in der Shell in diesem Verzeichnis

```sh
npm install --save-dev jsdoc
```

aus. Die Dateien `package.json` und `package-lock.json` (letztere durch das Kommando erstellt) sollten in Git aufgenommen werden. Der Ordner `node_modules/` sollte mit Git ignoriert werden. Dafür eine Zeile

```
node_modules
```

in die Datei `.gitignore` im Wurzelverzeichnis hinzufügen/Datei `.gitignore` mit dieser Zeile erstellen und committen.


#### Nutzung

**Die folgenden Schritte sind für den alltäglichen Gebrauch.**

Einmalig muss jede*r, nachdem durch `git pull` die `package.json` und die `package-lock.json` vorhanden sind, in der Shell folgendes Kommando ausführen:

```sh
npm install
```

Dadurch wird für dieses Projekt, für jede*n Kollaborateur*in genau die gleiche Version installiert. Root/Admin-Privilegien sind nicht notwendig. Der Erfolg der Installation kann mit dem Kommando

```sh
npx jsdoc --version
```

sichergestellt werden. Die Ausgabe sollte der Versionsstring sein, *ohne* vorher zu fragen, ob JSDoc installiert werden soll.


#### Skripte

**Die folgenden Schritte vereinfachen den Aufruf von JSDoc.**

Um den Auruf von JSDoc so einfach wie möglich zu gestalten bietet es sich an, in der Datei `package.json` ein entsprechendes Skript hinzuzufügen:

```js
{
  "name": "",
  /* ... */
  "scripts": {
    "doc": "jsdoc ARGUMENTE_HIER"
  }
}
```

Der Aufruf von

```sh
npm run doc
```

entspricht dann dem Aufruf `jsdoc ARGUMENTE_HIER`. (Zu `ARGUMENTE_HIER` siehe Abschnitt *Konfiguration.*) Wenn es zu einer Fehlermeldung kommt bzw. JSDoc nicht ausgeführt wird, kann das Problem sein, dass `npm install` nicht ausgeführt wurde.


## Konfiguration

* [Kommandozeilenargumente](jsdoc-opts)
* [Konfigurationsdatei](jsdoc-config)

Kommandozeilenargumente können auch in der Konfigurationsdatei gesetzt werden. Angabe der Konfigurationsdatei erfolgt über `jsdoc -c DATEI`.

[jsdoc-opts]: https://jsdoc.app/about-commandline.html
[jsdoc-config]: https://jsdoc.app/about-configuring-jsdoc.html


### Eingabedateien

Die Eingabedateien werden in der Konfigurationsdatei im Key `"source"` angegeben ([Dokumentation](jsdoc-source)).

*Beispiel:*

```json
{
  "source": {
    "include": [
      "PFAD/ZU/JAVASCRIPT",
      "ANDERER/PFAD/ZU/JS"
    ]
  }
}
```

[jsdoc-source]: https://jsdoc.app/about-configuring-jsdoc.html#specifying-input-files


### Ausgabe

Das Ergebnis landet standardmäßig im Ordner `out`. Über den Schlüssel `"opts": { "destination": "AUSGABE_PFAD" }` kann dieser Ordner angepasst werden. Dieser Ausgabepfad sollte auch von Git über einen Eintrag in `.gitignore` ignoriert werden.

*Pfadvorschlag:* Im Wurzelverzeichnis einen Ordner `doc/` anlegen, in diesem Ordner die Konfigurationsdatei `config.json` ablegen und Ausgabe in `doc/out` leiten.

```json
{
  "opts": {
    "destination": "doc/out"
  }
}
```


### Templates

*Am Beispiel von [docdash][].*

1. Installieren.

    1) Wenn die globale Installation für JSDoc gewählt wurde, muss der Ordner des Templates in das Repository eingebunden/reinkopiert werden und committed werden.
    2) Bei einer per-Projekt Installation führt eine Person `npm install --save-dev docdash` aus.

2. JSDoc vom Template erzählen

   In der Konfigurationsdatei den Schlüssel `"opts": { "template": "TEMPLATE_PFAD" }` hinzufügen. `"TEMPLATE_PFAD"` muss dabei auf den Ordner zeigen, der die Datei `publish.js` enthält. Im Fall der Installation über npm ist das `node_modules/docdash`.

Templates haben unterschiedliche Konfigurationsmöglichkeiten. Die des Standardtemplates von JSDoc sind [hier dokumentiert](jsdoc-templ-opts), die Optionen anderer Templates sind auf den Seiten der Templates verlinkt ([siehe hier](docdash-opts) zB. die Optionen für docdash).

[docdash]: https://github.com/clenemt/docdash.git
[docdash-opts]: https://github.com/clenemt/docdash#options
[jsdoc-templ-opts]: https://jsdoc.app/about-configuring-default-template.html


### Dokumentations-"Homepage"

Mit der Option `"opts": { "readme": "DATEI_PFAD" }` kann auf eine Datei verwiesen werden die auf der `index.html` der erzeugten Dokumentation eingebunden wird.


## Beispielprojekt

Ein kleines Beispielprojekt das die per-Projekt Installation nutzt liegt unter [JaSpa/jsdoc-example].

[JaSpa/jsdoc-example]: https://github.com/JaSpa/jsdoc-example
