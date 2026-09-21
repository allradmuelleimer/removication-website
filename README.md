# Removication — Website

Die öffentliche Datenschutzerklärung, die Google Play verlangt, dazu Impressum und eine
kleine Startseite. Deutsch und englisch.

Reines HTML und ein Stylesheet. **Kein Build-Schritt, kein Framework, keine Abhängigkeiten.**

**Live:** <https://allradmuelleimer.github.io/removication-website/>

```
index.html              Startseite DE
datenschutz/            Datenschutzerklärung DE   <- diese URL geht zu Google Play
impressum/              Impressum DE (§ 5 DDG)
privacy/                Privacy policy EN
imprint/                Imprint EN
404.html
css/site.css            Das einzige Stylesheet, Palette wie in der App
.nojekyll               Verhindert, dass GitHub Pages die Dateien durch Jekyll schickt
robots.txt
```

---

## Gehostet auf GitHub Pages

Kein dritter Dienst: Das Repository *ist* der Server. Pages liefert aus, was im Branch `main`
im Wurzelverzeichnis liegt — ein `git push` genügt, eine Minute später ist die Änderung online.

Einmalig eingerichtet wurde:

```bash
gh api -X POST repos/allradmuelleimer/removication-website/pages \
  -f 'source[branch]=main' -f 'source[path]=/'
```

Status jederzeit nachsehen:

```bash
gh api repos/allradmuelleimer/removication-website/pages --jq '.status, .html_url'
```

---

## Wichtig: die Pfade hängen am Repository-Namen

Pages liefert Projektseiten unter `/<repo-name>/` aus, nicht unter `/`. Deshalb stehen alle
internen Verweise als `/removication-website/…` im Quelltext. **Wird das Repository umbenannt,
bricht jede CSS- und Navigationsverknüpfung**, bis alles mitgezogen ist:

```bash
grep -rl "removication-website" . ../app/src/main/java --exclude-dir=.git \
  | xargs sed -i 's|removication-website|NEUER-NAME|g'
```

Dieselbe Adresse steht in `PRIVACY_POLICY_URL`
(`app/src/main/java/de/gio/removication/ui/settings/SettingsScreen.kt`) und wird bei Google
Play im Feld „Datenschutzerklärung" eingetragen.

---

## Änderungen an der Datenschutzerklärung

Die Quelle ist `../docs/datenschutzerklaerung.md`. Wer sie ändert, muss
`datenschutz/index.html` **und** `privacy/index.html` nachziehen und oben das Datum
hochsetzen — sonst weichen App-Link, Store-Angabe und Wahrheit voneinander ab.

---

## Nicht in Suchmaschinen

Auf jeder Seite steht Gideons Name (Impressum, Verantwortlicher). Deshalb tragen **alle Seiten**
`<meta name="robots" content="noindex,follow">` — seit 22.09.2026 auch Start- und
Datenschutzseite. Google Play braucht die Datenschutzseite nur **erreichbar**, nicht auffindbar.

- **Nicht über `robots.txt` sperren.** Dann liest Google das `noindex` nie und zeigt bereits
  bekannte Adressen trotzdem weiter an. Außerdem wertet Google eine `robots.txt` nur ganz oben auf
  der Domain aus (`allradmuelleimer.github.io/robots.txt`), die hier liegende ist wirkungslos.
- **Keine Sitemap** — sie würde Google genau die Seiten anbieten, die nicht erscheinen sollen.
- **Neue Seiten** bekommen dieselbe Zeile, sonst taucht der Name über sie wieder auf.
