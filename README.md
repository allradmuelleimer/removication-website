# Removication — Website

Die öffentliche Datenschutzerklärung, die Google Play verlangt, dazu Impressum und eine
kleine Startseite. Deutsch und englisch.

Reines HTML und ein Stylesheet. **Kein Build-Schritt, kein Framework, keine Abhängigkeiten.**
Was in diesem Ordner liegt, wird eins zu eins ausgeliefert.

```
index.html              Startseite DE
datenschutz/            Datenschutzerklärung DE   <- diese URL geht zu Google Play
impressum/              Impressum DE (§ 5 DDG)
privacy/                Privacy policy EN
imprint/                Imprint EN
404.html
css/site.css            Das einzige Stylesheet, Palette wie in der App
netlify.toml            Sicherheits-Header und CSP, kein Bauschritt
robots.txt  sitemap.xml
```

---

## Veröffentlichen über GitHub + Netlify

Einmal einrichten, danach genügt ein `git push` — Netlify baut automatisch nach.

**1. GitHub CLI anmelden** (nur beim allerersten Mal):

```bash
gh auth login
```

`GitHub.com` → `HTTPS` → `Y` → `Login with a web browser`, dann den angezeigten Code im
Browser eintippen.

**2. Repository anlegen und hochladen** — aus diesem Ordner heraus:

```bash
git init -b main
git add -A
git commit -m "Datenschutzerklärung und Impressum"
gh repo create removication-website --private --source=. --push
```

**3. Netlify mit dem Repo verbinden** (der einzige Klick-Teil, weil GitHub die Erlaubnis
selbst erteilen muss):

- [app.netlify.com](https://app.netlify.com) → **Add new site → Import an existing project → GitHub**
- Zugriff erlauben, dabei genügt „Only select repositories" mit `removication-website`
- **Build command leer lassen, Publish directory `.`** → Deploy
- **Site configuration → Change site name** → `removication`

Danach ist die Seite unter `https://removication.netlify.app/datenschutz/` erreichbar.

**Alternative ohne GitHub:** [app.netlify.com/drop](https://app.netlify.com/drop) öffnen und
diesen Ordner hineinziehen. Schneller beim ersten Mal, aber jede spätere Änderung heißt wieder
Ordner ziehen — und man landet leicht versehentlich auf einer zweiten Site statt auf der alten.

---

## Wenn der Site-Name nicht `removication` sein kann

Die Adresse steht fest in den `canonical`- und `hreflang`-Angaben, in `sitemap.xml`,
`robots.txt` und in `PRIVACY_POLICY_URL` der App. Läuft die Seite unter einem anderen Namen,
alles auf einmal umstellen:

```bash
grep -rl "removication.netlify.app" . ../app/src/main/java | xargs sed -i 's|removication\.netlify\.app|NEUE-ADRESSE|g'
```

---

## Änderungen an der Datenschutzerklärung

Die Quelle ist `../docs/datenschutzerklaerung.md`. Wer sie ändert, muss
`datenschutz/index.html` **und** `privacy/index.html` nachziehen und oben das Datum
hochsetzen — sonst weichen App-Link, Store-Angabe und Wahrheit voneinander ab.
