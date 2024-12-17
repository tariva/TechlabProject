# Zweites Projekt: Einfacher "Security Header & Domain Check"

## Idee & Ziel

Das Projekt soll ein leichtgewichtiges, schnell umsetzbares Webtool sein, das grundlegende Sicherheitsaspekte einer Website überprüft. Der Fokus liegt darauf, wichtige Security-Header (z. B. Content Security Policy, X-Frame-Options, Strict-Transport-Security) sowie grundlegende Domain-/DNS-Sicherheitseinstellungen (z. B. HTTPS-Verfügbarkeit, SPF-/DMARC-Einträge) abzurufen, zu analysieren und die Ergebnisse verständlich an einen nicht-technischen Nutzer zu vermitteln.

## Was soll erreicht werden?

Mit minimalem Aufwand soll ein Tool entstehen, das Website-Administratoren oder kleine Unternehmen auf einfache Weise zeigt, ob ihre Seite grundlegende Sicherheitsstandards erfüllt. Die Ergebnisse werden in einer verständlichen Ampel-Logik (Grün/Gelb/Rot) ausgegeben, zusammen mit kurzen, klaren Handlungsempfehlungen.

## Welches Problem wird gelöst?

Viele kleine Unternehmen wissen nicht, ob ihre Webpräsenz die grundlegenden Sicherheitsanforderungen erfüllt. Das Tool gibt einen ersten Überblick, ohne dass tiefes Expertenwissen notwendig ist. Nutzer erfahren schnell, ob etwa ein Content Security Policy-Header fehlt oder ob ihre Domain keine sinnvolle SPF/DMARC-Konfiguration hat, was Sicherheit und Vertrauenswürdigkeit beeinträchtigen kann.

## Zielgruppe

Betreiber kleiner Websites und Online-Shops ohne großes IT-Know-how
Kleine Firmen, die rasch prüfen möchten, ob sie gängige Sicherheitsstandards einhalten
