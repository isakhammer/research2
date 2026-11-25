# Instruksjoner for AI i semesteroppgaven

## Kompilering med Workbox

- Bruk Workbox (`wb`) til å kompilere LaTeX i dette prosjektet. Brukeren har
  bekreftet at dette er ønsket arbeidsmåte.
- Gjenbruk en kjørende Workbox-container. Finn den med `docker ps`, og kontroller
  med `docker inspect` at prosjektmappen er montert i containeren. Ikke hardkod
  container-ID eller velg en container bare ut fra navnet.
- Ved kontrollen 25. november 2026 var denne prosjektmappen montert som
  `/root/code` i en container med bildet `isakhammer/workbox:latest`.
- Kjør fra prosjektmappen i containeren:

  ```bash
  docker exec -w /root/code <container-ID> latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
  ```

- Bruk `-g` i tillegg når en full ombygging er nødvendig. Vent til kommandoen
  er ferdig, og kontroller resultatet før du rapporterer vellykket kompilering.
- Unngå samtidige kompileringer til de samme utfilene. Ved feil i genererte
  hjelpefiler som `main.aux`, bygg en ren, midlertidig prosjektkopi i Workbox
  før du endrer LaTeX-kilden. Dette ble testet med vellykket resultat:
  kopier prosjektets `.tex`- og `.bib`-filer til en ny mappe laget med `mktemp -d`
  under `/tmp` i containeren, og kjør `latexmk` der. Ta med eventuelle bilder og
  andre avhengigheter hvis prosjektet senere får slike. Kopier ferdig `main.pdf`
  tilbake til prosjektmappen først når kompileringen har avsluttet uten feil.
- `wb` starter en interaktiv Workbox og tar ikke imot vilkårlige
  kompileringskommandoer som argumenter. Bruk `docker exec` for en kjørende
  container. Ikke stopp eller start brukerens container på nytt.
- Hvis Docker-tilgang krever utvidet verktøytilgang, bruk den vanlige
  godkjenningsmekanismen. Ikke endre systeminstallasjonen av LaTeX for å omgå dette.
- Hvis en annen kompileringsmetode feiler, prøv Workbox før du konkluderer med
  at dokumentet ikke kan kompileres. Skill mellom verktøyfeil og feil i koden.
- Kontroller den oppdaterte `main.pdf` etter layoutendringer. PDF-en som Workbox
  lager ligger i samme prosjektmappe og kan leses og gjengis uten ny kompilering.

## Dokumentet

- Rediger eksisterende LaTeX-filer. Undervisningsplanen skal være redigerbar
  LaTeX-kode i `vedlegg-undervisningsplan.tex`, ikke innlimte PDF-sider.
- Behold det avtalte oppsettet med hele undervisningsplanen samlet på én A4-side.
