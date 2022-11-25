# Ein Thema Benutzen

Das Thema auswählen ist in Slidev echt einfach. Es genügt das Hinzufügen von `theme:` zum Beginn des Dokumentes:

```yaml
---
theme: seriph
---
```

Wenn man den Server startet, fragt Slidev einen automatisch, ob man das ausgewählte Thema installieren möchte:

<div class="language-md">
<pre>
<span class="token keyword">?</span> The Theme <span class="token string">"@slidev/theme-seriph"</span> was not found in your project, do you want to install it now? › (Y/n)
</pre>
</div>

Man kann aber auch ein Thema manuell installieren:

```bash
$ npm install @slidev/theme-seriph
```

Das wars! Viel spaß beim Benutzen deines neuen Themas!

Für mehr Informationen bezüglich des Benutzens eines Themas ist es am Besten, sich an das README des jeweiligen Themas zu richten.

Wollen Sie ihr eigenes Thema entwerfen? Dann können Sie [lernen, wie man ein Thema entwirft](themes/write-a-theme).

## Thema "Eject"-en

Sollten man die volle Kontrolle über das aktuelles Thema haben wollen, dann kann man das Thema mithilfe des folgendem Befehles in das lokales Dateisystem kopieren:

```bash
$ slidev theme eject
```

Jetzt befindet sich das Thema in `./theme`.

Die Referenz zum Thema, die sich in der Hauptdatei des Projektes befindet, wird automatisch aktualisiert.

```yaml
---
theme: ./theme
---
```

Dieses Feature kann besonders hilfreich sein, wenn man ein Thema entwerfen möchte, das auf einem Anderen basiert. In dem Fall aber nicht vergessen, dem Originalautor seine Bemühungen zuzuschreiben!

## Locales Thema

Wie der vorherige Abschnitt es verraten hat, kann man ein Thema im lokalen Dateisystem benutzen, indem man ein relativer Pfad im Referenzfeld benutzt:

```yaml
---
theme: ./pfad/zum/thema
---
```

Weitere details dazu befinden sich unter [wie man ein Thema entwirft](themes/write-a-theme).
<span style='float: footnote;'><a href="../index.html#toc">Go to TOC</a></span>
