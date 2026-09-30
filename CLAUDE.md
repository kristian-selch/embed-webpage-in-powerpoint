# Embed webpage in PowerPoint

PowerPoint content add-in (XML-manifest, ContentApp), der viser en live https-webside på en slide. Bruges af Kristian/Selch Media på Mac (PowerPoint, Office LTSC 2024).

## Filer
- `index.html` – hele add-in'et: HTML, CSS og JS i én fil. Ingen npm, ingen build. Office.js hentes fra https://appsforoffice.microsoft.com/lib/1/hosted/office.js
- `manifest.xml` – sideloades lokalt i PowerPoint. PowerPoint henter den ikke fra GitHub Pages.
- `icon-32.png`, `icon-64.png`, `icon-80.png` – ikoner, refereret fra manifestet.

## Hosting
GitHub Pages fra `main` / root: https://kristian-selch.github.io/embed-webpage-in-powerpoint/
Push til main = live efter 1–2 minutter. Repoet er OFFENTLIGT: aldrig nøgler, tokens eller .env.

## Regler der ikke må brydes
- `<Id>` i manifest.xml må ALDRIG ændres – så mister eksisterende præsentationer koblingen til add-in'et.
- `<Permissions>Restricted</Permissions>` må ikke hæves uden Kristians godkendelse.
- Settings-nøglerne `url` og `zoom` i `Office.context.document.settings` må ikke omdøbes – gemte præsentationer bruger dem.
- Kun https-URL'er må vises.
- Al UI-tekst er på dansk.

## Hvordan det virker
- Hvert indsat element er en selvstændig instans med egne settings.
- `body.presenting` skjuler værktøjslinjen under slideshow (`getActiveViewAsync` + `ActiveViewChanged`; "read" = slideshow, "edit" = redigering).
- Sider der sender `X-Frame-Options` / CSP `frame-ancestors` (fx dr.dk) kan ikke vises i iframen. Det skal ikke forsøges omgået med proxy.

## Test og verifikation
- Der findes ingen typecheck eller `npm run build` i dette projekt.
- `index.html` kan åbnes direkte i en browser (kører uden Office, men gemmer ikke settings).
- Ved ændringer i manifest.xml: kør `npx --yes office-addin-manifest validate manifest.xml` og hæv `<Version>`.
- Efter push: tjek `curl -sI https://kristian-selch.github.io/embed-webpage-in-powerpoint/index.html` giver 200.
- Endelig test sker i PowerPoint på Kristians Mac.

## Geninstallation af manifest (kun når manifest.xml ændres)
Med PowerPoint lukket:
cp manifest.xml ~/Library/Containers/com.microsoft.Powerpoint/Data/Documents/wef/embed-webpage-in-powerpoint.xml
og ryd ~/Library/Containers/com.microsoft.Powerpoint/Data/Library/Application Support/Microsoft/Office/16.0/Wef/

## Arbejdsgang
Start altid med at undersøge og rapportere tilbage, før der rettes noget. Commits opdeles efter emne. Afslut med push til origin/main.
