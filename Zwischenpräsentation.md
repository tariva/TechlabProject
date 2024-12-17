# Guidelines Zwischenpräsentation II und Abschlusspräsentation

Folgende Aspekte sollten bei der Projektumsetzung und den Präsentationen dazu berücksichtig werden.

## Daten

- Welche Fragestellung wird im Projekt bearbeitet und was ist das Ziel des Projekts? Wie tragen die Daten zur Beantwortung der Fragestellung bzw. zur Erreichung der Zielsetzung bei?

Das Projekt untersucht, ob Websites grundlegende Sicherheitsstandards wie Security-Header (z. B. CSP, HSTS) und DNS-Sicherheitseinstellungen (z. B. SPF, DMARC) erfüllen. Ziel ist es, Website-Betreibern schnelle, verständliche Einblicke in potenzielle Sicherheitslücken zu geben und klare Handlungsempfehlungen bereitzustellen.

Die Daten aus der Sicherheits- und DNS-Analyse dienen als Grundlage, um die Sicherheitskonfigurationen der Websites zu überprüfen. Sie ermöglichen die automatisierte Bewertung der Sicherheitslage, die Generierung von KI-gestützten Empfehlungen und die Erstellung eines verständlichen Ampelberichts, der die Ergebnisse klar visualisiert und das Ziel des Projekts direkt unterstützt.

- Wie wurden die Daten gesammelt?

  - Referenzdaten:
    Eine vordefinierte Link-Liste wurde verwendet, z. B. Listen aller Bildungseinrichtungen, um Vergleichswerte zu erstellen. Crawling Manuel/Automatisiert
  - Webcheck API/Anwendung:
    - Sicherheits- und Konfigurationsdaten wurden über spezialisierte APIs und Tools gesammelt, darunter:
    - Technische Informationen:
    - IP-Info, SSL-Chain, DNS Records, Open Ports, Server Status, Server Info, Traceroute, Tech Stack, DNS Security Extensions.
    - Sicherheitsanalysen:
    - HTTP Security Features, TLS Cipher Suites, TLS Config, Firewall Detection, Malware & Phishing Detection, Cookies, Security.txt, HSTS.
    - Inhaltliche Analysen:
      - Crawl Rules, Redirect Chain, Listed Pages, Linked Pages, Archive History.
    - Ranking & Qualitätsmetriken:
      - Global Ranking, Quality Metrics, Carbon Footprint.
    - DNS & Domain-Spezifika:
         Whois Lookup, Domain Info, TXT Records, DNS Server, Email Configuration.
    - Inwieweit ist garantiert, dass sich es um eine zuverlässige Datenquelle handelt?
      - Technische Prüfung  
  - Best Practices und aktuelle Informationen:
    - Aktuelle Sicherheitsrichtlinien und -empfehlungen wurden in einer RAG-Datenbank (Retrieval-Augmented Generation) gespeichert, um KI-gestützte Empfehlungen zu erstellen und zu validieren.

## Explorative Analyse und Data Preprocessing

- Welche ersten Erkenntnisse können durch eine Explorative Analyse gewonnen werden?

Häufigkeit fehlender Sicherheitsfeatures, Geografische und technische Trends

- Welche Datenvorverarbeitungsschritte müssen angewendet werden? (Skalierung, Umgang mit kategorischen Daten, Umgang mit fehlenden Daten, etc.)Modellierung
  - Datenbereinigung
    - Fehlende Werte bei numerischen Feldern (z. B. offene Ports, Ladezeiten) werden mit dem Mittelwert oder Median gefüllt.
    - Bei kategorischen Feldern (z. B. „Ja/Nein“ zu HTTPS oder Firewall) wird ein Default-Wert wie „Unbekannt“ gesetzt.
    - Komplette Zeilen mit kritischen fehlenden Werten (z. B. keine DNS-Daten) werden entfernt.
  - Umgang mit kategorischen Daten
    - Binary Encoding für Ja/Nein-Daten (z. B. „HTTPS aktiviert“ → 1, „Nicht aktiviert“ → 0).
    - One-Hot-Encoding für nominale Daten (z. B. „Server-Standort“: EU, USA, Asien).
    - Ordinal-Encoding für ordinal skalierte Felder (z. B. Ampel-Logik: „Grün“ → 1, „Gelb“ → 2, „Rot“ → 3).
  - Feature Engineering -  Erstellung neuer Features basierend auf bestehenden Daten:
    - Security Score: Aggregation von Security-Headern, HTTPS-Status und DNS-Konfiguration zu einem Punktesystem.
    - Risk Index: Kombination von offenen Ports, fehlender Firewall, fehlenden DNS-Einträgen.
    - Server Regions: Gruppierung der Server-Standorte (z. B. EU, Nordamerika, Asien).

- Welche Modellierungsansätze werden gewählt und weshalb?
  - Zielsetzung der Modellierung:
    - Klassifikation der Websites nach Sicherheitszustand (Ampel-Logik: Grün/Gelb/Rot).
    - Regression für numerische Vorhersagen, z. B. „Security Score“ oder Risikoindikator.
  - Modelle für die Klassifikation:
    - Random Forest oder Gradient Boosting: Robust bei unbalancierten und fehlenden Daten.
    - Logistische Regression: Für einfache Interpretierbarkeit der Ergebnisse.
    - Neural Networks (MLP): Für komplexe Feature-Interaktionen.
  - Hyperparameter-Tuning:
    - Verwendung von Grid Search oder Random Search für Optimierung.
  - Validierung der Modelle:
    - Cross-Validation zur Vermeidung von Overfitting.
    - Evaluation anhand von Metriken wie Accuracy, F1-Score und Confusion Matrix.

Die Nutzung von Agents automatisiert und optimiert den gesamten Analyseprozess, der bei manueller Durchführung zeitaufwendig, fehleranfällig und schwer skalierbar wäre. Die Agents übernehmen die Datenvorverarbeitung, erstellen abgeleitete Features wie Security Scores, generieren KI-gestützte Empfehlungen und validieren die Ergebnisse durch Cross-Verification mit mehreren Modellen (z. B. GPT-4 und Llama). Durch diese Automatisierung können große Datenmengen effizient verarbeitet, präzise Sicherheitsbewertungen erstellt und klare Handlungsempfehlungen in Echtzeit bereitgestellt werden. Manuelle Analysen hingegen erfordern erheblichen personellen Aufwand, sind langsamer und liefern oft inkonsistente Ergebnisse. Das agentenbasierte System bietet somit eine skalierbare, zuverlässige und kosteneffiziente Lösung, die auch für Nicht-Techniker verständlich aufbereitet wird.

- In welche Modellkategorien (z.B. Supervised Learning, Unsupervised Learning) fallen diese Modelle? Welche spezifischen Anforderungen gibt es für diese Modellkategorien?

Die Modelle wie GPT-4, Mistral und LLaMA fallen in die Kategorie des Unsupervised Learning während des Trainings, da sie ohne gelabelte Daten auf großen Textkorpora trainiert werden, um Muster und Zusammenhänge in der Sprache zu erkennen. Beim Einsatz als Generative Modelle im Projekt werden sie jedoch im Rahmen des Supervised Fine-Tuning oder Prompt Engineering verwendet, um gezielt Aufgaben wie Klassifikation, Empfehlungserstellung oder Textgenerierung zu lösen. Ein großer Vorteil liegt in ihrer Flexibilität: Sie können unstrukturierte Daten (wie Sicherheitsberichte) verarbeiten, Muster erkennen und präzise Handlungsempfehlungen in natürlicher Sprache generieren. Durch den Einsatz mehrerer Modelle wie GPT-4 für tiefgehende Analysen und Mistral für schnellere Validierung lässt sich eine Cross-Verification umsetzen, die Ergebnisse zuverlässiger macht, Inkonsistenzen minimiert und aktuelle Sicherheitsrichtlinien direkt integriert. Diese Kombination ermöglicht eine skalierbare, interpretierbare und adaptive Lösung für komplexe Sicherheitsanalysen.

- Nach welchen Kriterien wurden die Modelle ausgewählt (z.B. Interpretierbarkeit des Modells, Komplexität des Modells)?

  - Leistungsfähigkeit: Modelle wie GPT-4 und Mistral bieten hohe Genauigkeit bei der Generierung von Erkenntnissen und Empfehlungen.
  - Flexibilität: Sie können unstrukturierte Daten verarbeiten und unterschiedliche Aufgaben wie Klassifikation, Textgenerierung und Analyse lösen.
  - Schnelligkeit: Leichtgewichtigere Modelle wie Mistral ermöglichen schnelle Validierungen und geringere Rechenkosten.
  - Cross-Verification: Die Kombination mehrerer Modelle erhöht die Zuverlässigkeit durch Abgleich und Minimierung von Inkonsistenzen.
  - Interpretierbarkeit: Die generierten Empfehlungen sind verständlich formuliert und lassen sich klar an die Nutzer kommunizieren.
  - Komplexität: Die Modelle sind vortrainiert und erfordern keine umfangreiche Anpassung, was die Implementierung vereinfacht.

## Evaluierung und Visualisierung

- Mit welchen Metriken werden die Modellresultate evaluiert? (z.B. Accuracy, RMSE, etc.)
  - Textgenerierungsmetriken (bei GPT-4, Mistral und LLaMA):
    - Semantic Consistency: Manuelle oder automatische Bewertung der inhaltlichen Übereinstimmung der Empfehlungen.
    - BLEU-Score: Bewertung der Übereinstimmung zwischen generierten Texten und Referenzempfehlungen.

  - DSPy.ai
    - Task-Specific Accuracy: Bewertung, wie oft das Modell die gewünschte Struktur oder Antwort liefert (z. B. korrekt klassifizierte Sicherheitsbewertungen).
    - Validation Loss: Minimierung der Fehler bei optimierten Prompts durch iterative Anpassungen.
    - Constraint Adherence: Prüfung, ob das Modell strukturierte Antworten liefert (z. B. JSON, Markdown-Format).
    - Pass@k: Evaluierung, wie viele der Top-k Outputs die gewünschte Lösung enthalten. Dies ist besonders relevant bei der Verifikation durch mehrere Modelle
    - Semantic Validation: DSPy.ai nutzt programmgesteuerte Tests, um die Semantik der generierten Antworten sicherzustellen und logische Fehler zu vermeiden.

- Welche Möglichkeiten der Visualisierung bestehen im Hinblick auf die Modellierung und Evaluierung?
  - Modellleistung:
    - Confusion Matrix: Darstellung korrekter und falscher Klassifikationen (Ampel-Logik: Grün/Gelb/Rot).
    - ROC-/AUC-Kurve: Bewertung der Klassifikationsgüte und Trade-offs zwischen Sensitivität und Spezifität.
  - Feature-Analyse:
    - Feature-Importanz: Balkendiagramme zur Identifikation der wichtigsten Einflussfaktoren (z. B. TLS, DNS, Header).
    - Korrelationsmatrix: Heatmap zur Darstellung von Zusammenhängen zwischen Sicherheitsmerkmalen.
  - Ergebnisvisualisierung:
    - Ampel-Logik: Farbkodierte Bewertungen (Grün/Gelb/Rot) zur schnellen Übersicht.
    - Markdown-Reports: Strukturierte, textuelle Darstellung der Sicherheitsbewertungen und Empfehlungen.

## Implementierung

- Wie und wo werden die Datenquellen verwaltet? Local (Prototyp)
- Erfolgt das Deployment On-Premise oder Off-Premise? Wie gut skaliert die Lösung bei großen Datenmengen? (autogen production rdy ?)
- Wie werden die Modelle aktualisiert, wenn sich die Datenbasis ändert? (Prototyp manuel)
- Welche Programmiersprachen bzw. welche Module werden für die Datenauswertung benötigt? (Python )
- Wie wird der Code dokumentiert bzw. zugänglich gemacht? (Github invite only )
