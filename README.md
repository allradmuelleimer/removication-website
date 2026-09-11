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
robots.txt  sitemap.xml
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
