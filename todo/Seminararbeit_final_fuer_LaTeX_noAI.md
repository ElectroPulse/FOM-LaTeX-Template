# Potenziale und Risiken des Einsatzes von KI-gestützten Code-Generatoren in der agilen Softwareentwicklung

## 1 Einleitung

Große Sprachmodelle sind in der Softwareentwicklung angekommen: 81 % der Befragten des DORA-Reports 2024 berichten eine Prioritätenverschiebung zugunsten von KI.[^1] Eine Mehrfallstudie in drei deutschen Organisationen zeigt den regulären Einsatz in agilen Teams bei häufig fehlenden Regelungen.[^2] Die Modelle treffen dort auf ein Vorgehensmodell aus kurzen Lieferzyklen, technischer Exzellenz und regelmäßiger Reflexion.[^3]

Die Befundlage ist widersprüchlich. Ein Laborexperiment weist eine um 55,8 % kürzere Bearbeitungszeit aus, ein randomisierter Versuch mit erfahrenen Entwicklern dagegen eine um 19 % längere.[^4] Der DORA-Report verbindet zudem eine um 25 % höhere KI-Adoption mit besserer Dokumentationsqualität, aber geringerer Auslieferungsstabilität.[^5] Ob Code-Generatoren agile Prozesse stärken oder aushöhlen, ist damit offen.

Die Arbeit fragt, welche Potenziale und Risiken der Einsatz KI-gestützter Code-Generatoren in agilen Softwareentwicklungsprozessen mit sich bringt und welche Gestaltungsempfehlungen daraus folgen. Ziel ist deren Systematisierung entlang agiler Praktiken.

Methodisch stützt sich die Arbeit auf eine literaturbasierte Studienauswertung; eigene Erhebungen, Werkzeugvergleiche, juristische Detailfragen und KI-Ethik bleiben ausgeklammert. Kapitel 2 klärt die Begriffe, Kapitel 3 und 4 stellen Potenziale und Risiken gegenüber, Kapitel 5 wägt ab, Kapitel 6 beantwortet die Forschungsfrage.

[^1]: Vgl. DORA, State of DevOps, 2024, S. 18.
[^2]: Vgl. Neumann, M. et al., GenAI Adoption, 2026, S. 297 f.
[^3]: Vgl. Beck, K. et al., Agile Prinzipien, 2001, o. S.
[^4]: Vgl. Peng, S. et al., Developer Productivity, 2023, S. 2; Becker, J. et al., AI Impact, 2025, S. 2.
[^5]: Vgl. DORA, State of DevOps, 2024, S. 37, S. 39.

---

## 2 Konzeptionelle Grundlagen

Die Analyse setzt zwei Klärungen voraus: was unter einem KI-gestützten Code-Generator zu verstehen ist und welche agilen Praktiken den Bezugsrahmen bilden, an dem die folgenden Kapitel die Wirkungen prüfen.

### 2.1 KI-gestützte Code-Generatoren: Funktionsweise und Einsatzformen

Als KI-gestützte Code-Generatoren gelten hier Werkzeuge auf Basis großer Sprachmodelle (Large Language Models, LLM), die auf umfangreichen Mengen quelloffenen Codes trainiert sind und aus dem Kontext — Kommentaren, Funktionsnamen, umgebendem Code — Vervollständigungen vorschlagen.[^6] Verbreitet sind mehrere Modellfamilien, darunter GPT-4o, Claude 3.5, Gemini 1.5, Codestral und Llama 3.[^7] Zu unterscheiden sind drei Einsatzformen: die dialogische Nutzung über eine Weboberfläche, die Integration in die Entwicklungsumgebung und agentische Werkzeuge, die dort selbstständig Dateien durchsuchen, ändern und Programme iterativ debuggen.[^8] Die Unterscheidung ist keine Formalie: Studien fixieren jeweils eine Form. Eine Qualitätsuntersuchung bildet dabei bewusst die kostenfreie Weboberfläche nach, wie sie besonders Berufseinsteiger nutzen.[^9] Ihre Befunde gelten folglich nicht ohne Weiteres für den in Entwicklungsumgebungen integrierten Einsatz.

### 2.2 Agile Softwareentwicklung als Einsatzkontext

Agile Entwicklung liefert Software in kurzen Abständen und reflektiert regelmäßig das eigene Vorgehen.[^10] In Scrum verdichtet sich das zu Sprints von höchstens einem Monat, in denen die Qualität nicht abnehmen darf.[^11] Als Qualitätsmaßstab dient die Definition of Done, eine formale Beschreibung des Zustands, den ein Increment erreichen muss; konkretisiert wird sie über Kriterien wie fehlerfreie Unit-Tests, ersatzweise über das Vier-Augen-Prinzip.[^12] Zwei getrennte Schleifen prüfen das Erreichte: das Sprint Review das Produkt, die Retrospektive den Prozess.[^13] Verantwortlich ist das Team, das für das Increment einsteht und selbst entscheidet, wie Anforderungen umgesetzt werden.[^14] Hinzu treten Praktiken mit hohem Anteil sozialer Interaktion wie Pair Programming[^15] sowie Code Reviews, deren Geschwindigkeit als Prozessgröße erhoben wird.[^16] Diese sechs Elemente bilden das Raster für die Kapitel 3 und 4.

[^6]: Vgl. Pearce, H. et al., Asleep at the Keyboard, 2022, S. 754.
[^7]: Vgl. Kharma, M. F. et al., LLM-Generated Code, 2026, S. 7300.
[^8]: Vgl. Becker, J. et al., AI Impact, 2025, S. 6.
[^9]: Vgl. Swaraj, A., Kumar, S., Trust But Verify, 2026, S. 67.
[^10]: Vgl. Beck, K. et al., Agile Prinzipien, 2001, o. S.
[^11]: Vgl. Schwaber, K., Sutherland, J., Scrum Guide, 2020, S. 8.
[^12]: Vgl. ebd., S. 13; Preußig, J., Agiles Projektmanagement, 2020, S. 93.
[^13]: Vgl. Schwaber, K., Sutherland, J., Scrum Guide, 2020, S. 10 f.
[^14]: Vgl. ebd., S. 5, S. 9.
[^15]: Vgl. Neumann, M. et al., GenAI Adoption, 2026, S. 291.
[^16]: Vgl. DORA, State of DevOps, 2024, S. 37.

---

## 3 Potenziale des Einsatzes in agilen Entwicklungsprozessen

Die berichteten Vorteile liegen auf zwei Ebenen: in der Geschwindigkeit der Erstellung und in der Qualitätssicherung samt Wissensweitergabe. Beide werden am Raster aus Kapitel 2.2 geprüft.

### 3.1 Produktivitäts- und Geschwindigkeitseffekte

Ein kontrolliertes Experiment mit 95 rekrutierten Programmierern zeigt den Effekt unter Laborbedingungen: Bei der Implementierung eines HTTP-Servers in JavaScript arbeitete die Gruppe mit Assistenz 55,8 % schneller, bei einem Konfidenzintervall von 21 % bis 89 % und dem stärksten Effekt bei weniger erfahrenen Teilnehmern.[^17] Ein Feldexperiment in einem Technologiekonzern bestätigt die Richtung unter Praxisbedingungen: Nach Einführung eines internen Sprachmodells stieg der Code-Ausstoß um 55 %, bei Beschäftigten mit höchstens einem Jahr Erfahrung um 67 %, während der Effekt bei erfahreneren Beschäftigten nicht signifikant war.[^18] Gemessen wird dort die Menge produzierter Codezeilen, nicht deren Qualität; nur 11 bis 18 Prozentpunkte des Zuwachses entfallen auf unmittelbar übernommene Modellausgaben, die übrigen 36 bis 43 führen die Autoren auf Zeitersparnis zurück.[^19] Dem steht ein randomisierter Versuch entgegen, in dem 16 erfahrene Open-Source-Entwickler 246 Aufgaben in ihren eigenen Repositorien bearbeiteten: Sie benötigten mit Assistenz 19 % mehr Zeit, hatten vorab aber eine Beschleunigung um 24 % erwartet.[^20] Die Befunde widersprechen einander nur scheinbar, denn sie unterscheiden sich systematisch in Erfahrung und Vertrautheit mit der Codebasis. Daraus folgt für diese Arbeit: Der Zeitgewinn konzentriert sich dort, wo Kontextwissen fehlt. Ein pauschaler Zuwachs der Velocity, also der je Iteration fertiggestellten Story Points,[^21] ist nicht zu erwarten.

### 3.2 Qualitätssicherungs- und Wissenseffekte

Auf Prozessebene verbindet der DORA-Report eine um 25 % höhere KI-Adoption mit einer um 7,5 % besseren Dokumentationsqualität, einer um 3,4 % höheren Codequalität und um 3,1 % schnelleren Code Reviews.[^22] Diese Größen lassen sich dem Raster aus Kapitel 2.2 zuordnen: Die Review-Geschwindigkeit betrifft die Praktik Code Review unmittelbar, die Codequalität den Maßstab der Definition of Done. Eine Befragung in zwei großen agilen Organisationen stützt das aus Nutzersicht: Zwei Drittel berichten geringeren Aufwand für wiederkehrende Aufgaben, drei Fünftel weniger Zeit für die Informationssuche; häufiger im Arbeitsfluss bleibt jedoch nur knapp ein Drittel — deutlich weniger als in einer früheren Herstellerbefragung.[^23] In agilen Teams reichen die Anwendungen über das Programmieren hinaus: Von 21 dokumentierten Anwendungsfällen entfallen sieben auf Dokumentation und sechs auf kreative Aufgaben, darunter die Vorbereitung von Retrospektiven; zugleich ersetzt das Werkzeug die Recherche in Foren.[^24] Das senkt Einstiegshürden, bleibt aber an die einzelne Person gebunden: Dieselbe Untersuchung beschreibt die Werkzeuge als persönliche Produktivitätsassistenz, die einzelne Tätigkeiten unterstützt, ohne die Zusammenarbeit im Team zu verändern.[^25]

[^17]: Vgl. Peng, S. et al., Developer Productivity, 2023, S. 2 f.
[^18]: Vgl. Gambacorta, L. et al., Labour Productivity, 2024, S. 5.
[^19]: Vgl. ebd., S. 5.
[^20]: Vgl. Becker, J. et al., AI Impact, 2025, S. 2.
[^21]: Vgl. Preußig, J., Agiles Projektmanagement, 2020, S. 132.
[^22]: Vgl. DORA, State of DevOps, 2024, S. 37.
[^23]: Vgl. Wivestad, V. T., Ulfsnes, R., Journey Through SPACE, 2025, S. 46.
[^24]: Vgl. Neumann, M. et al., GenAI Adoption, 2026, S. 298–300.
[^25]: Vgl. ebd., S. 299.

---

## 4 Risiken und Herausforderungen

Den Potenzialen stehen Befunde gegenüber, die dieselben Praktiken betreffen. Sie ordnen sich in zwei Gruppen: Qualität und Sicherheit des erzeugten Codes sowie Wirkungen auf Kompetenz, Prozess und Recht.

### 4.1 Qualitäts- und Sicherheitsrisiken

Der erzeugte Code ist nicht durchgängig sicher. Eine Untersuchung von 1.689 durch einen Assistenten erzeugten Programmen wies rund 40 % als verwundbar aus.[^26] Eine Messstudie über 200 Aufgaben, vier Sprachen und fünf Modellfamilien zeigt zudem Sprachabhängigkeit: In C und C++ treten Speicherfehler, fest eingebettete Geheimnisse und kryptografische Fehlanwendungen häufiger auf, moderne Sicherheitsmerkmale bleiben oft ungenutzt.[^27] In einer Nutzerstudie mit 47 Teilnehmern schrieb die Gruppe mit Assistenz signifikant unsichereren Code und hielt ihn zugleich häufiger für sicher.[^28] Solches Übernehmen fehlerhafter Modellausgaben wird als Overreliance bezeichnet.[^29] Hinzu tritt der Korrekturaufwand für Vorschläge, die fast richtig sind: 66 % der Befragten einer Entwicklerumfrage nennen ihn als Hauptärgernis, 45 % das Debuggen KI-erzeugten Codes.[^30] Auf Prozessebene sinken Auslieferungsstabilität (−7,2 %) und Durchsatz (−1,5 %) je 25 % höherer KI-Adoption; als Erklärung vermutet der Report größere Änderungspakete und erinnert an das Prinzip kleiner Losgrößen.[^31] Daraus ergibt sich eine Rückkopplung: Code Review und Definition of Done sollen solche Fehler abfangen, setzen aber die prüfende Aufmerksamkeit voraus, die Overreliance gerade schwächt.

### 4.2 Kompetenz-, prozess- und rechtsbezogene Risiken

Die zweite Gruppe betrifft Menschen und Prozesse. Ein randomisiertes Experiment zum Erlernen einer unbekannten Programmbibliothek — durchgeführt von einem Modellanbieter und bislang als Vorabfassung veröffentlicht — findet, dass KI-Unterstützung Verständnis, Code-Lesen und Debugging beeinträchtigt, ohne im Mittel Zeit zu sparen; wer die Aufgabe vollständig delegierte, gewann Produktivität auf Kosten des Lernens.[^32] Dazu passt, dass rund 20 % der befragten Entwickler geringeres Vertrauen in die eigenen Fähigkeiten berichten und nur 17 % eine bessere Zusammenarbeit im Team beobachten.[^33] Daraus folgt ein Risiko für die gemeinsame Verantwortlichkeit, eine der zwölf Vorgehensweisen des Extreme Programming:[^34] Sie setzt voraus, dass Teammitglieder fremden Code lesen und ändern können — genau die Fähigkeit, die nach diesen Befunden leidet. Hinzu kommt eine Wahrnehmungslücke: Dieselben Entwickler, die 19 % länger brauchten, schätzten sich im Nachhinein um 20 % schneller ein; Aufwandsschätzungen und Velocity-Werte dürften dadurch verzerrt werden.[^35] Rechtlich und organisatorisch bleibt die Vertraulichkeit offen. Fehlen verbindliche Richtlinien, entscheiden Beschäftigte nach eigenem Ermessen; Code mit Schlüsseln oder personenbezogenen Daten darf Werkzeugen ohne gesicherte Datenhoheit nicht übergeben werden.[^36]

[^26]: Vgl. Pearce, H. et al., Asleep at the Keyboard, 2022, S. 754.
[^27]: Vgl. Kharma, M. F. et al., LLM-Generated Code, 2026, S. 7300.
[^28]: Vgl. Perry, N. et al., Insecure Code, 2023, S. 2785.
[^29]: Vgl. Shen, J. H., Tamkin, A., Skill Formation, 2026, S. 4.
[^30]: Vgl. Swaraj, A., Kumar, S., Trust But Verify, 2026, S. 71.
[^31]: Vgl. DORA, State of DevOps, 2024, S. 39 f.
[^32]: Vgl. Shen, J. H., Tamkin, A., Skill Formation, 2026, S. 1.
[^33]: Vgl. Swaraj, A., Kumar, S., Trust But Verify, 2026, S. 71.
[^34]: Vgl. Preußig, J., Agiles Projektmanagement, 2020, S. 152 f.
[^35]: Vgl. Becker, J. et al., AI Impact, 2025, S. 2.
[^36]: Vgl. Neumann, M. et al., GenAI Adoption, 2026, S. 298.

---

## 5 Kritische Würdigung und Gestaltungsempfehlungen

Das Muster aus Kapitel 3.1 setzt sich fort: Die Befunde unterscheiden sich darin, worin die Werkzeuge eingebettet sind. Der DORA-Report zieht aus seinen eigenen gegenläufigen Zahlen den Schluss, dass ein verbesserter Entwicklungsprozess die Auslieferung nicht von selbst verbessert, solange Grundlagen wie kleine Änderungspakete und belastbare Testmechanismen fehlen.[^37] Die Mehrfallstudie stützt das von der Teamseite: Die Werkzeuge wirken als persönliche Assistenz, ohne die Zusammenarbeit zu verändern.[^38] Daraus ergibt sich die These dieser Arbeit: Über Nutzen und Schaden entscheidet nicht das Werkzeug, sondern die Praktik, die es umgibt. Review, Definition of Done und Retrospektive sind zugleich Verstärker der Potenziale und Schutz vor den Risiken.

Fünf Empfehlungen folgen daraus. Erstens ist die Definition of Done um Kriterien für generierten Code zu erweitern; angesichts der Verwundbarkeitsquoten aus Kapitel 4.1 gehört eine automatisierte Sicherheitsprüfung zur Mindestanforderung, und im Review ist generierter Code als solcher auszuweisen. Zweitens sind Nutzungsrichtlinien verbindlich zu fassen, da Beschäftigte sonst nach eigenem Ermessen entscheiden.[^39] Drittens ist der Einsatz nach Erfahrungsstand zu differenzieren: Weil die Gewinne bei Berufseinsteigern auftreten und bei erfahrenen Entwicklern ausbleiben oder ins Gegenteil umschlagen,[^40] liegt deren Beitrag eher in Architektur und Review als in generierungsnaher Arbeit. Viertens erhalten drei von sechs beobachteten Interaktionsmustern das Lernen auch unter KI-Nutzung;[^41] maßgeblich ist demnach die kognitive Beteiligung, nicht der Verzicht. Fünftens lässt sich der Wahrnehmungslücke aus Kapitel 4.2 mit dem vorhandenen Instrumentarium begegnen: Die Retrospektive ist die Praktik, die die Selbsteinschätzung korrigiert — allerdings nur, wenn ihr gemessene Durchlaufzeiten zugrunde liegen und nicht Eindrücke. Das entspricht dem Zweck der Größe: Team Velocity dient realistischen Vorhersagen, nicht der Maximierung der Arbeitsgeschwindigkeit.[^42]

Die Grenzen der Arbeit sind zu benennen. Die Literaturbasis ist jung und teils vorläufig: Zwei tragende Studien liegen als Vorabfassung vor, eine stammt vom Anbieter selbst, eine weitere umfasst sechzehn Personen. Laborbefunde und Selbstauskünfte lassen sich nicht ohne Weiteres auf agile Teams übertragen, und der Ursachenzusammenhang hinter den Prozesskennzahlen ist im Report selbst nur eine Vermutung. Die Empfehlungen sind daher begründete Ableitungen, keine gesicherten Wirkungsaussagen.

[^37]: Vgl. DORA, State of DevOps, 2024, S. 40.
[^38]: Vgl. Neumann, M. et al., GenAI Adoption, 2026, S. 299.
[^39]: Vgl. ebd., S. 298.
[^40]: Vgl. Gambacorta, L. et al., Labour Productivity, 2024, S. 5; Becker, J. et al., AI Impact, 2025, S. 2.
[^41]: Vgl. Shen, J. H., Tamkin, A., Skill Formation, 2026, S. 1.
[^42]: Vgl. Preußig, J., Agiles Projektmanagement, 2020, S. 133.

---

## 6 Fazit und Ausblick

Die Forschungsfrage lässt sich verdichtet beantworten. Die Potenziale liegen in beschleunigter Erstellung, besserer Dokumentation und niedrigeren Einstiegshürden, die Risiken in unsicherem Code, Overreliance, nachlassender Kompetenz und verzerrter Selbsteinschätzung. Beide Seiten treffen dieselben Praktiken. Ob Nutzen oder Schaden überwiegt, entscheidet sich daher nicht an der Werkzeugwahl, sondern daran, ob Review, Definition of Done und Retrospektive an generierten Code angepasst werden; die Empfehlungen aus Kapitel 5 setzen dort an.

Offen bleiben drei Fragen. Erstens die Langzeitwirkung auf die Kompetenzentwicklung, da die vorliegenden Untersuchungen kurze Zeiträume abbilden. Zweitens die Wirkung agentischer Werkzeuge: Sie ändern nach Kapitel 2.1 selbstständig Dateien, sodass die menschliche Prüfung erst nach mehreren Arbeitsschritten ansetzt. Drittens, ob sich die Rolle der Entwicklung dauerhaft vom Ausführen zum Überwachen verschiebt — eine Verschiebung, die für frühere Automatisierungswellen bereits beschrieben ist.[^43]

[^43]: Vgl. Shen, J. H., Tamkin, A., Skill Formation, 2026, S. 1.

---

# Literaturverzeichnis

Becker, Joel, Rush, Nate, Barnes, Beth, Rein, David (AI Impact, 2025): Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity, arXiv:2507.09089, 2025, \<https://arxiv.org/abs/2507.09089\> [Zugriff 2026-07-26]

Gambacorta, Leonardo, Qiu, Han, Shan, Shuo, Rees, Daniel M. (Labour Productivity, 2024): Generative AI and Labour Productivity: A Field Experiment on Coding, BIS Working Papers Nr. 1208, Basel: Bank for International Settlements, 2024

Goldman, Alfredo, Marczak, Sabrina, Tonin, Graziela Simone, Bassi, Dairton, Silva da Silva, Tiago, Schön, Eva-Maria, Neumann, Michael, Pinna, Andrea (Hrsg.) (XP 2026, 2026): Agile Processes in Software Engineering and Extreme Programming, LNBIP 578, Cham: Springer, 2026

Kharma, Mohammed F., Choi, Soohyeon, Alkhanafseh, Mohammad, Mohaisen, David (LLM-Generated Code, 2026): Security and Quality in LLM-Generated Code: A Multi-Language, Multi-Model Analysis, in: IEEE Transactions on Dependable and Secure Computing, 23. Jg. (2026), Heft 3, S. 7300–7314

Marchesi, Lodovica, Goldman, Alfredo, Lunesu, Maria Ilaria, Przybyłek, Adam, Aguiar, Ademar, Morgan, Lorraine, Wang, Xiaofeng, Pinna, Andrea (Hrsg.) (XP 2024 Workshops, 2025): Agile Processes in Software Engineering and Extreme Programming – Workshops, LNBIP 524, Cham: Springer, 2025

Neumann, Michael, Bischof, Lasse, Hinz, Nic Elias, Altun, Abdullah, Stockmann, Luca, Schrader, Dennis, Ahaus, Ana Carolina, Demirci, Erim Can, Gabel, Benjamin, Rauschenberger, Maria, Diebold, Philipp, Fritzemeier, Henning, Przybyłek, Adam (GenAI Adoption, 2026): Between Policy and Practice: GenAI Adoption in Agile Software Development Teams, in: Goldman, Alfredo, Marczak, Sabrina, Tonin, Graziela Simone, Bassi, Dairton, Silva da Silva, Tiago, Schön, Eva-Maria, Neumann, Michael, Pinna, Andrea (Hrsg.), Agile Processes in Software Engineering and Extreme Programming, LNBIP 578, Cham: Springer, 2026, S. 290–308

Pearce, Hammond, Ahmad, Baleegh, Tan, Benjamin, Dolan-Gavitt, Brendan, Karri, Ramesh (Asleep at the Keyboard, 2022): Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions, in: 2022 IEEE Symposium on Security and Privacy (SP), Piscataway: IEEE, 2022, S. 754–768

Peng, Sida, Kalliamvakou, Eirini, Cihon, Peter, Demirer, Mert (Developer Productivity, 2023): The Impact of AI on Developer Productivity: Evidence from GitHub Copilot, arXiv:2302.06590, 2023, \<https://arxiv.org/abs/2302.06590\> [Zugriff 2026-07-26]

Perry, Neil, Srivastava, Megha, Kumar, Deepak, Boneh, Dan (Insecure Code, 2023): Do Users Write More Insecure Code with AI Assistants?, in: Proceedings of the 2023 ACM SIGSAC Conference on Computer and Communications Security (CCS '23), New York: ACM, 2023, S. 2785–2799

Preußig, Jörg (Agiles Projektmanagement, 2020): Agiles Projektmanagement. Agilität und Scrum im klassischen Projektumfeld, 2. Aufl., Freiburg: Haufe-Lexware, 2020

Shen, Judy Hanwen, Tamkin, Alex (Skill Formation, 2026): How AI Impacts Skill Formation, arXiv:2601.20245, 2026, \<https://arxiv.org/abs/2601.20245\> [Zugriff 2026-07-26]

Swaraj, Aman, Kumar, Sandeep (Trust But Verify, 2026): Trust But Verify: Analyzing the Tradeoffs of AI-Generated Code, in: IEEE Software, 43. Jg. (2026), Heft 4, S. 66–75

Wivestad, Viggo Tellefsen, Ulfsnes, Rasmus (Journey Through SPACE, 2025): A Journey Through SPACE: Unpacking the Perceived Productivity of GitHub Copilot, in: Marchesi, Lodovica, Goldman, Alfredo, Lunesu, Maria Ilaria, Przybyłek, Adam, Aguiar, Ademar, Morgan, Lorraine, Wang, Xiaofeng, Pinna, Andrea (Hrsg.), Agile Processes in Software Engineering and Extreme Programming – Workshops, LNBIP 524, Cham: Springer, 2025, S. 42–50

**Internetquellen**

Beck, Kent, Beedle, Mike, van Bennekum, Arie, Cockburn, Alistair, Cunningham, Ward, Fowler, Martin, Grenning, James, Highsmith, Jim, Hunt, Andrew, Jeffries, Ron, Kern, Jon, Marick, Brian, Martin, Robert C., Mellor, Steve, Schwaber, Ken, Sutherland, Jeff, Thomas, Dave (Agile Prinzipien, 2001): Prinzipien hinter dem Agilen Manifest, \<https://agilemanifesto.org/iso/de/principles.html\> [Zugriff 2026-07-26]

DORA / Google Cloud (State of DevOps, 2024): Accelerate State of DevOps Report 2024, \<https://dora.dev/research/2024/dora-report/\> [Zugriff 2026-07-26]

Schwaber, Ken, Sutherland, Jeff (Scrum Guide, 2020): Der Scrum Guide. Der gültige Leitfaden für Scrum: Die Spielregeln, November 2020, \<https://scrumguides.org\> [Zugriff 2026-07-26]

---

# KI-Hilfsmittelverzeichnis

ClClaude, Version Opus 4.8 (Anthropic) — Themenfindung und Strukturentwurf; eingesetzt am 28.06.2026.
