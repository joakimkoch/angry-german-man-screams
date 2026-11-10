# Herr Ohr – decibelmätare

En webbapp i en enda fil (`index.html`), publicerad via GitHub Pages (grenen `gh-pages`).
Den mäter ljudnivån med mikrofonen. När man är för högljudd säger en tysk mansröst
"Thank you for screaming in my ear."

## Återstående uppgift: bygg in en riktig tysk mansröst

Talsyntes i webbläsaren ger oftast en kvinnoröst, så rösten ska bakas in som ljud:

1. Kolla vilka tyska röster som finns: `say -v '?' | grep de_`
   - Helst **Markus** eller **Yannick**. Saknas de: be användaren ladda ner en under
     Systeminställningar → Hjälpmedel → Talat innehåll → Systemröst → Hantera röster → Tyska.
2. Generera: `say -v Markus -r 165 "Thank you for screaming in my ear." -o /tmp/ohr.aiff`
3. Gör den lite djupare och komprimera:
   `ffmpeg -y -i /tmp/ohr.aiff -af "asetrate=22050*0.9,aresample=44100,atempo=1.08,loudnorm" -c:a aac -b:a 96k /tmp/ohr.m4a`
   (Justera asetrate-faktorn efter smak. Testa med `afplay /tmp/ohr.m4a`.)
4. Lägg in base64 i `index.html`: ersätt `const VOICE_B64 = "";` med innehållet från
   `base64 -i /tmp/ohr.m4a | tr -d '\n'`. Behåll `VOICE_MIME = "audio/mp4"`.
5. Committa och pusha både `main` och `gh-pages` (`git push origin main main:gh-pages`).

## Bra att veta
- Mikrofonen kräver https (fungerar på GitHub Pages, inte i Claudes artefaktvisare eller via file://).
- iOS: ljud och talsyntes måste "låsas upp" vid klick på Start – det görs redan i `start()`.
- Gräns, kalibrering och vald reservröst sparas i localStorage.
