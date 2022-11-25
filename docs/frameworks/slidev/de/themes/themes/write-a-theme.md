# Erstelle ein Thema

Wir empfehlen unseren Generator zu nutzten, um ein Gerüst für das Thema zu erstellen:

```bash
$ npm init slidev-theme
```

Jetzt kann man alles verändern und mit dem code spielen. Man kann sich auch auf Beispiele von den [Offiziellen Themen](themes/gallery) beziehen.

## Möglichkeiten

Ein Thema kann folgende Punkte anpassen:

- Globale Styles
- Standardkonfigurationen bereitstellen (Schriften, Farbschema, Highlighters, etc.)
- benutzerdefinierte Layouts bereitstellen oder vorhandene überschreiben
- benutzerdefinierte Komponenten bereitstellen oder vorhandene überschreiben
- Windi CSS-Konfigurationen erweitern
- Tools wie Monaco und Prism konfigurieren

## Konventionen

Themen werden im npm Registry veröffentlicht und sollten folgenden Konventionen folgen:

- Der Paketname sollte mit `slidev-theme-` beginnen, zum Beispiel: `slidev-theme-awesome`.
- `slidev-theme` und `slidev` sollte in den `keywords` der `package.json` Datei enthalten sein.

## Einrichtung

Um den Testspielplatz für das Thema einzurichten, kann man eine `example.md` Datei mit folgendem Inhalt erstellen, damit Slidev weiß, dass dieser Ordner als Thema genutzt werden soll:

```md
---
theme: ./
---
```

Optional, kann man Skripte zur `packages.json` Datei hinzufügen.

```json
// package.json
{
  "scripts": {
    "dev": "slidev example.md",
    "build": "slidev build example.md",
    "export": "slidev export example.md",
    "screenshot": "slidev export example.md --format png"
  }
}
```
Um das Thema zu veröffentlichen, muss nur der `npm publish` Befehl ausgeführt werden. Es wird kein Build-Prozess benötigt (man kann die `.vue` und `.ts` Datei direkt veröffentlichen, Slidev ist clever genug, um das Thema zu verstehen).

Thema Kontribution folgt den selben Punkten, wie die Konventionen bei lokalen Anpassungen, in der [Dokumentation](custom/) steht mehr über Namenskonventionen.

## Standardkonfigurationen

> Verfügabr seit v0.19

Ein Thema kann [Standardkonfigurationen](custom/.html) in der  `package.json` Datei bereitstellen.

```json
// package.json
{
  "slidev": {
    "default": {
      "aspectRatio": "16/9",
      "canvasWidth": 980,
      "fonts": {
        "sans": "Robot",
        "mono": "Fira Code"
      }
    }
  }
}
```

Schriftarten werden automatisch von [Google Fonts](https://fonts.google.com/) importiert.

Erfahre mehr über [Schriftarten](custom/fonts) und  [Frontmatter konfigurationen](/custom/.html)

## Thema-Metadaten

### Farbschema

Standartmäßig geht Slidev davon aus, dass das Thema Dark- und Lightmode unterstützt. Wenn man möchte, dass das Thema in einem bestimmten Farbschema präsentiert wird, kann dies in der `package.json`Datei angegeben werden.

```json
// package.json
{
  "name": "slidev-theme-my-cool-theme",
  "keywords": [
    "slidev-theme",
    "slidev"
  ],
  "slidev": {
    "colorSchema": "light" // oder "dark" oder "both"
  }
}
```

Alle Styles, die Darkmode spezifisch sind, können in einer `dark` CSS Klasse untergebracht werden:

```css
/* Allgemeines css hier */

html:not(.dark) {
  /* Lightmode CSS hier */
}

html.dark {
  /* Darkmode CSS hier */
}
```

Slidev schaltet die `dark` CSS Klasse am `html` Element der Seite um, um das Farbschema zu wechseln.

### Highlighter

Syntaxhervorhebungs-Farben können auch im Thema bereit gestellt werden. Wir unterstützen sowohl [Prism](https://prismjs.com/) als auch [Shiki](https://github.com/shikijs/shiki). Weitere Informationen sind in den [Syntax-Highlighting Dokumentationen](custom/highlighters).

Dein Thema kann entweder einen der beide oder beide unterstützen. Konfigurationsbeispiele sind im Standard-Thema [`./styles/code.css`](https://github.com/slidevjs/slidev/blob/main/packages/create-theme/template/styles/code.css) / [`./setup/shiki.ts`](https://github.com/slidevjs/slidev/blob/main/packages/create-theme/template/setup/shiki.ts) zu finden. 

Denke auch daran, unterstützte Highlighters in der `package.json` Datei anzugeben.

```json
// package.json
{
  "slidev": {
    "highlighter": "shiki" // oder "prism" oder "all"
  }
}
```

### Slidev Version

Falls das Thema auf einer bestimmten Slidev Funktion basiert, kann die minimale Slidev Version angegeben werden:

```json
// package.json
{
  "engines": {
    "slidev": ">=0.19.3"
  }
}
```

Falls Benutzer eine ältere Version von Slidev nutzen, wird ein Fehler ausgeworfen.
<span style='float: footnote;'><a href="../index.html#toc">Go to TOC</a></span>
