# Integrierte Produktionsplanung und -steuerung in SAP S/4HANA – konzeptionelle Grundlagen und prozessuale Umsetzung am Beispiel der Modellfirma Global Bike

## Inhaltsübersicht (Gliederung)

1. Einleitung
2. Wissenschaftliche Grundlagen
3. Praxisbeispiel: Der integrierte PP-Prozess bei Global Bike
4. Fazit
- Literaturverzeichnis
- Kennzeichnung des KI-Einsatzes

---

## 1 Einleitung

### 1.1 Problemstellung und Relevanz

Fertigungsunternehmen stehen vor der Aufgabe, schwankende Marktnachfrage, begrenzte Kapazitäten und mehrstufige Materialstrukturen so aufeinander abzustimmen, dass das richtige Produkt in der richtigen Menge termin- und kostengerecht bereitsteht. Die Produktionsplanung und -steuerung (PPS) ist dabei ein zentraler Hebel der Wettbewerbsfähigkeit, weil sie unmittelbar auf Liefertreue, Bestandshöhe und Herstellkosten wirkt.[^pps-relevanz] Eine isolierte, manuelle Abstimmung dieses Spannungsfelds aus Nachfrage, Kapazität und Materialverfügbarkeit führt regelmäßig zu Überbeständen, Fehlmengen und intransparenten Abläufen.[^isoliert] Integrierte ERP-Systeme begegnen diesem Problem, indem sie Planungs-, Ausführungs- und Abrechnungsdaten auf einer gemeinsamen Datenbasis verknüpfen und so eine durchgängige, konsistente Steuerung ermöglichen.[^erp]

### 1.2 Zielsetzung und Forschungsfrage

Hieran knüpft die Zielsetzung der Arbeit an. Untersucht wird, **wie SAP S/4HANA den durchgängigen Planungs- und Steuerungsprozess abbildet und inwieweit die einzelnen Prozessschritte – von der Absatz- und Produktionsgrobplanung (SOP) über Programmplanung und Materialbedarfsplanung (MRP) bis zur Auftragsabrechnung – das beschriebene Abstimmungsproblem lösen.** Der Betrachtungsschwerpunkt liegt auf der Lagerfertigung (Make-to-Stock) als Form der diskreten Fertigung; zugrunde gelegt wird die Planungsstrategie „Planung mit Endmontage" (Strategie 40).[^strategie] Die Variantenkonfiguration bzw. Kundeneinzelfertigung wird nicht vertieft, sondern lediglich als Ausblick aufgegriffen.

### 1.3 Vorgehensweise und Aufbau der Arbeit

Die Arbeit ist zweigeteilt aufgebaut. Teil A leitet die betriebswirtschaftlich-theoretischen Grundlagen der PPS her: die Begriffsabgrenzung, die Planungshierarchie des Sukzessivplanungskonzepts, die Stammdaten, die Mechanik der Materialbedarfsplanung sowie die Fertigungsstrategien. Teil B überträgt diese Konzepte auf den integrierten Fertigungsprozess der Modellfirma Global Bike und analysiert ihn schrittweise am Beispiel des Deluxe Touring Bike – von der Stammdatenpflege bis zur controllingseitigen Auftragsabrechnung. Datengrundlage des Praxisteils ist das Curriculum „Einführung in S/4HANA mit Global Bike" des SAP University Competence Center Magdeburg.[^curriculum] Eine kritische Würdigung des Prozesses sowie ein zusammenfassendes Fazit mit Ausblick schließen die Arbeit ab.

---

## 2 Wissenschaftliche Grundlagen

### 2.1 Begriffliche Einordnung der Produktionsplanung und -steuerung

Die Produktionsplanung und -steuerung (PPS) umfasst die Gesamtheit der Aufgaben, die erforderlich sind, um die betriebliche Leistungserstellung mengen-, termin- und kapazitätsgerecht zu disponieren, zu veranlassen und zu überwachen.[^def-pps] In SAP S/4HANA bildet der Funktionsbereich Produktionsplanung (PP) diese Aufgaben ab; das System unterteilt die Produktion zunächst in Produktionsplanung und Produktionsdurchführung und unterscheidet je nach Fertigungstyp zwischen diskreter Fertigung, Serienfertigung, KANBAN und Prozessfertigung.[^sap-typen] Konzeptionell knüpft die PPS an das MRP-II-Konzept (Manufacturing Resource Planning) an, das die reine Materialbedarfsplanung um die Kapazitäts-, Termin- und Ressourcenplanung erweitert und die Teilpläne zu einem geschlossenen Planungs- und Steuerungskreislauf verbindet.[^mrp2] Innerhalb dieses Rahmens ist die *Planung* von der *Steuerung* abzugrenzen: Die Planung legt fest, welches Erzeugnis in welcher Menge zu welchem Termin herzustellen ist, während die Steuerung die geplanten Aufträge freigibt, ihre Durchführung veranlasst und über Rückmeldungen kontrolliert.[^plan-steuer]

### 2.2 Die Planungshierarchie im Sukzessivplanungskonzept

Klassisch folgt die PPS dem Sukzessivplanungskonzept, das den komplexen Gesamtplanungsprozess in aufeinander aufbauende, hierarchisch geordnete Stufen zerlegt: Jede Stufe übernimmt die Vorgaben der übergeordneten Ebene und detailliert sie weiter (Top-down-Verfahren von der aggregierten zur dispositiven Planung).[^sukzessiv] In SAP S/4HANA konkretisiert sich diese Hierarchie in der Stufenfolge Prognose → Absatz- und Produktionsgrobplanung (SOP) → Programmplanung → Leitteileplanung (MPS) → Materialbedarfsplanung (MRP) → Produktionsdurchführung → Auftragsabrechnung.[^sap-stufen] Diese Schritte lassen sich drei Planungsebenen zuordnen – der strategischen (aggregierten) Planung, der Detailplanung und der Produktionssteuerung –, denen jeweils unterschiedliche Rollen im Unternehmen zugewiesen sind.[^ebenen]

Den Ausgangspunkt bildet die Absatzprognose. Sie stützt sich auf historische Verbrauchsdaten und nutzt Prognosemodelle für konstante, trend- oder saisonbedingte Verläufe sowie kombinierte Trend-Saison-Modelle, wobei die Modellwahl automatisch oder manuell erfolgen kann.[^prognose] Da Prognosen grundsätzlich fehlerbehaftet sind und sowohl Über- als auch Unterbestände unmittelbar zu Verlusten führen, ist ihre Güte für alle nachgelagerten Stufen erfolgskritisch.[^prognose-fehler] Auf der aggregierten Ebene plant die SOP Absatz, Produktion und Kapazität für Produktgruppen in Zeitfenstern; die Programmplanung bildet anschließend das Bindeglied zur Feinplanung, indem sie die aggregierten Vorgaben in konkrete Planprimärbedarfe – das Produktionsprogramm – überführt, das von MPS und MRP bis auf Komponentenebene aufgelöst wird.[^programm]

### 2.3 Stammdaten als Fundament der Planung

Stammdaten bilden die wiederverwendbare, abteilungsübergreifende Datenbasis, ohne die keine Planungsstufe rechnen kann; in SAP S/4HANA sind dies für die Produktion vor allem Material, Stückliste, Arbeitsplan, Arbeitsplatz und Produktgruppe.[^stammdaten] Der **Materialstammsatz** – in allgemeiner Terminologie der Teilestamm – ist das zentrale Stammdatenobjekt jedes produktionswirtschaftlichen Systems. In ihm sind zahlreiche Attribute hinterlegt, deren Umfang und Differenzierung davon abhängen, welche betrieblichen Bereiche einbezogen werden; planungsrelevant sind vor allem die dispositions- und beschaffungsbezogenen Angaben, nach denen ein Material geplant und disponiert wird.[^matstamm] Die **Stückliste** listet sämtliche Komponenten auf, aus denen ein Produkt oder eine Baugruppe besteht. Sie kann einstufig oder mehrstufig sein und bildet damit die mehrstufige Erzeugnisstruktur ab; aus dem Primärbedarf des Fertigerzeugnisses leiten sich über die Stücklistenauflösung die Sekundärbedarfe der Komponenten ab.[^stueckliste]

Der **Arbeitsplan** beschreibt die Folge der Vorgänge (was, wo, wann, wie), die zur Herstellung eines Materials erforderlich sind. Er dient als Vorlage für Fertigungsaufträge, als Grundlage der Terminierung und der Erzeugniskalkulation und enthält je Vorgang den ausführenden Arbeitsplatz, Vorgabewerte bzw. Zeitelemente sowie einen Steuerschlüssel.[^arbeitsplan] Der **Arbeitsplatz** ist der Ort der Wertschöpfung – Person, Maschine oder Fließband – und definiert die verfügbaren Kapazitäten. Über seine Kalkulationsdaten (Kostenstelle und Leistungsarten) liefert er die Verrechnungssätze zur Bewertung der Vorgänge, während seine Terminierungsdaten (u. a. Liege- und Transportzeiten, Formelschlüssel) in die Terminierung einfließen.[^arbeitsplatz] Die **Produktgruppe** schließlich fasst Materialien zu Produktfamilien zusammen und bildet die aggregierte Planungsebene der Absatz- und Produktionsgrobplanung.[^produktgruppe]

### 2.4 Kernmechanik der Materialbedarfsplanung (MRP)

Die Materialbedarfsplanung (MRP) bildet als plangesteuerte Disposition die detaillierte Planungsebene. Ausgehend von den Bedarfen aus Programmplanung bzw. Leitteileplanung sichert sie die Materialverfügbarkeit über alle Stücklistenebenen und überführt die Bedarfe in einen detaillierten Produktions- und Beschaffungsplan.[^mrp-zweck] SAP gliedert den MRP-Lauf hierfür in fünf logische Schritte. Im ersten Schritt, der **Nettobedarfsrechnung**, stellt das System je Material den verfügbaren Lagerbestand und die fest eingeplanten Zugänge den Anforderungen – etwa Planprimärbedarf, Reservierungen und Kundenaufträgen – sowie dem Sicherheitsbestand gegenüber; eine Unterdeckung löst einen Beschaffungsvorschlag aus.[^netto]

Die **Losgrößenberechnung** bestimmt anschließend die Beschaffungsmenge, wobei statische, periodische oder optimierende Losgrößenverfahren zum Einsatz kommen.[^losgroesse] Im Schritt **Beschaffungsart** entscheidet das System, ob ein Bedarf durch interne Beschaffung (Eigenfertigung) oder durch Fremdbeschaffung gedeckt wird; für die Eigenfertigung entsteht ein Planauftrag, für die Fremdbeschaffung eine Bestellanforderung.[^beschaffungsart] Die **Terminierung** ermittelt mehrstufig die Start- und Endtermine, sodass untergeordnete Baugruppen und Komponenten rechtzeitig zum übergeordneten Bedarfstermin bereitstehen.[^mrp-terminierung] Die **Stücklistenauflösung** schließlich leitet aus dem Bedarf des übergeordneten Materials die Sekundärbedarfe der Komponenten ab, die ihrerseits in die Nettobedarfsrechnung der nächsttieferen Stücklistenebene eingehen.[^stueckaufloesung] Ergebnis des MRP-Laufs ist damit für jeden eigengefertigten Bedarf ein **Planauftrag**. Dieser bildet das Bindeglied zwischen Planung und Steuerung: Er beschreibt einen künftigen Bedarf und wird später in einen Fertigungsauftrag (Eigenfertigung) oder eine Bestellung (Fremdbeschaffung) umgewandelt.[^planauftrag]

### 2.5 Vom Plan zur Ausführung: Auftragsarten und Fertigungssteuerung

Die drei zentralen Auftragsarten grenzen sich nach ihrer Funktion ab: Der Planauftrag gehört zur Planung und beschreibt einen künftigen Bedarf, während Fertigungsauftrag und Bestellung der Steuerung zuzuordnen sind.[^auftragsarten] Mit der Umwandlung des Planauftrags in einen Fertigungsauftrag (Eigenfertigung) bzw. eine Bestellung (Fremdbeschaffung) geht die Planung in die Ausführung über.[^umwandlung] Der Fertigungsauftrag steuert die Produktionsvorgänge und die damit verbundenen Kosten; er legt das zu fertigende Material, die Menge, das Werk, die Rahmenzeiten, die eingesetzten Ressourcen (Arbeitsplätze und Zeiten) sowie die Kosten fest.[^fertauftrag] Nach Terminierung und Verfügbarkeitsprüfung wird der Auftrag freigegeben (Status FREI); damit ist er ausführungsbereit, und Warenbewegungen sowie Rückmeldungen werden fortan mit ihm abgeglichen.[^freigabe]

Die Warenbewegungen werden über Bewegungsarten gesteuert und buchhalterisch erfasst. Beim **Warenausgang** werden die für den Auftrag reservierten Komponenten dem Bestand entnommen – über die Bewegungsart 261 (Verbrauch für Auftrag) –, wobei die entnommenen Materialien dem Auftrag als Ist-Kosten zugeordnet werden.[^warenausgang] Die **Rückmeldung** dokumentiert den Fortschritt eines Auftrags (u. a. Mengen, Zeiten) und ist Voraussetzung einer realistischen Produktionssteuerung.[^rueckmeldung] Beim **Wareneingang** gelangt die gefertigte Menge in den Bestand; Bestandsmenge und -wert werden fortgeschrieben, der Auftrag wird aktualisiert und es entstehen ein Material-, ein Finanz- und ein Kostenrechnungsbeleg.[^wareneingang] Den Abschluss bildet die **Auftragsabrechnung**: Die tatsächlichen Kosten des Auftrags werden an einen oder mehrere Empfänger – im Regelfall das Material bzw. den Bestand – abgerechnet; das Abrechnungsprofil legt dabei Empfänger und Aufteilungsregeln fest. Differenzen zwischen Be- und Entlastung werden auf ein Preisdifferenzkonto gebucht, und Plan-, Soll- sowie Istkosten lassen sich einander gegenüberstellen.[^abrechnung]

### 2.6 Fertigungsstrategien (theoretischer Rahmen für die Bewertung)

Fertigungsstrategien legen fest, ob und wie weit die Produktion bereits vor dem Eingang konkreter Kundenaufträge vorgeplant wird. SAP unterscheidet hierbei vor allem die Lagerfertigung (Make-to-Stock), die Kundeneinzelfertigung (Make-to-Order) sowie konfigurierbare Materialien im Sinne einer Mass Customization.[^strategien-arten] Bei der Lagerfertigung erfolgt die Planung über unabhängigen (Plan-)Bedarf und der Vertrieb aus dem Lagerbestand, während bei der Kundeneinzelfertigung die Produktion unmittelbar durch Kundenaufträge angestoßen wird.[^strategien-lager] Theoretisch lässt sich diese Abgrenzung über den Entkopplungspunkt (Order Penetration Point) fassen: Er markiert die Stelle in der Wertschöpfungskette, bis zu der kundenanonym auf Basis von Prognosen vorgeplant und ab der kundenauftragsbezogen gefertigt wird.[^opp]

In SAP konkretisieren sich die Strategien in Strategiegruppen; für die Lagerfertigung steht etwa die Strategie 40 (Planung mit Endmontage).[^strategien-lager] Der im Praxisteil betrachtete Global-Bike-Prozess nutzt eben diese Strategiegruppe 40 (Vorplanung mit Endmontage): Die Bedarfe werden über Planprimärbedarfe prognosebasiert vorgeplant, und auch die Endmontage wird vor dem Auftragseingang eingeplant; der Entkopplungspunkt liegt damit nahe am Fertigerzeugnis bzw. am Lager.[^gb-strategie40] Der untersuchte Prozess ist folglich der diskreten Make-to-Stock-Fertigung zuzuordnen – eine Einordnung, die den Bezugsrahmen für die spätere kritische Würdigung des Verfahrens bildet.

---

## 3 Praxisbeispiel: Der integrierte PP-Prozess bei Global Bike

### 3.1 Vorstellung der Modellfirma und des Szenarios

Global Bike ist das Modellunternehmen des Curriculums – ein internationaler Fahrradhersteller mit den Gesellschaften Global Bike Inc. (USA) und Global Bike Germany GmbH.[^gb-unternehmen] SAP S/4HANA bildet es hierarchisch ab: Der Mandant (Global Bike) ist die oberste Ebene, darunter der Buchungskreis als kleinste rechnungslegungspflichtige Einheit (US00 bzw. DE00); die Produktion erfolgt in Werken – hier Dallas (DL00) – mit Lagerorten für Roh-, Halb- und Fertigware.[^gb-orga]

Betrachtungsobjekt ist das Fertigerzeugnis Deluxe Touring Bike (DXTR, in Schwarz, Silber und Rot), das in Dallas in diskreter Fertigung montiert wird. Seine mehrstufige Produktstruktur umfasst einen farbspezifischen Rahmen, Räder und weitere Komponenten, die über Montagevorgänge zum Endprodukt zusammengeführt werden.[^gb-produkt] Für die Grobplanung sind die Fahrräder zu einer Produktgruppe zusammengefasst.[^gb-produktgruppe] Getragen wird der Prozess arbeitsteilig von mehreren Rollen – u. a. Fertigungsleiter (Jun Lee), Werksleiter (Hiro Abe) und Controller (Jamie Shamblin) – aus den Bereichen Produktionsplanung (PP) und Materialwirtschaft (MM).[^gb-rollen]

Das Szenario operationalisiert damit die Konzepte aus Kapitel 2: die Ebenen Mandant/Buchungskreis/Werk/Lagerort die Organisationsstruktur, die Produktgruppe die aggregierte SOP-Ebene (Kap. 2.2), der mehrstufige Aufbau des Fahrrads die Stückliste (Kap. 2.3) und seine Lagerfertigung die Strategie „Planung mit Endmontage" (Strategiegruppe 40, Kap. 2.6). Der folgende Abschnitt beginnt daher mit der Vorbereitung der Stammdaten.

### 3.2 Vorbereitung der Stammdaten

Bevor geplant werden kann, sind die Stammdaten des Deluxe Touring Bike um planungsrelevante Angaben zu erweitern. In der Rolle Fertigungsleiter wird der werksspezifische Materialstammsatz (DL00) angepasst: In der Dispositionssicht wird die Strategiegruppe 40 (Vorplanung mit Endmontage) gesetzt und damit die Make-to-Stock-Strategie aus Kap. 2.6 verankert; in der Prognosesicht werden zwölf Initialisierungsperioden hinterlegt, in den Steuerungsdaten der Optimierungsgrad „F (Fein)" mit Parameteroptimierung gewählt und die Glättungsfaktoren gesetzt: Alpha 0,20 (Grundwert), Beta 0,10 (Trend), Gamma 0,30 (Saison) und Delta 0,30 (MAD).[^matstamm-praxis] Diese Werte steuern die exponentielle Glättung der Absatzprognose (Kap. 2.2) und werden analog für die silberne und schwarze Variante gepflegt.

Anschließend wird der Arbeitsplan angepasst. Er ist über Arbeitsplangruppe und Plangruppenzähler definiert, referenziert das Material und enthält je Vorgang Vorgabewerte und Zeitelemente für die Terminierung.[^arbeitsplan-praxis] Kern der Anpassung ist die Zuordnung (Allokation) der Stücklistenkomponenten zu den Vorgängen: Rahmen und Sitz zu Vorgang 0020, Lenker zu 0030, Aluminiumrad und Kettenschaltung zu 0040, Kette (0050), Bremsanlage (0060), Pedale (0070) sowie Garantiedokument und Verpackung zu 0100. Damit ist festgelegt, welche Komponente in welchem Fertigungsschritt verbaut wird – ein abhängiger Prozess, bei dem jeder Vorgang auf dem vorhergehenden aufsetzt.[^komponenten-praxis]

Diese Vorbereitung bestätigt die in Kap. 2.3 hergeleitete Rolle der Stammdaten: Erst die Verknüpfung der Dispositions- und Prognosedaten des Materialstammsatzes mit Stückliste und Arbeitsplan schafft die konsistente Datenbasis, ohne die weder Bedarfs- noch Terminplanung rechnen kann.

### 3.3 Absatz- und Produktionsgrobplanung (SOP)

Im vierten Schritt legt der Fertigungsleiter für die Produktgruppe (PG-DXTR, DL00) einen zwölfmonatigen Absatz- und Produktionsgrobplan (SOP) an, der Plandaten konsolidiert und künftige Mengen prognostiziert. Grundlage ist der historische Verbrauch – in der Fallstudie vorgegebene Vergangenheitswerte (05.2017 bis 03.2021) –; über die automatische Modellauswahl erkennt das System Trend und Saison und wendet ein Saison-Trend-Modell an, womit die Prognosemodelle aus Kap. 2.2 praktisch umgesetzt werden.[^sop-prognose] Aus dem Absatzplan wird ein absatzsynchroner Produktionsplan abgeleitet und um eine Zielreichweite (fünf Perioden) ergänzt, sodass die Produktionsmengen den Absatz decken und den gewünschten Lagerbestand sichern.[^sop-produktionsplan]

Da der SOP langfristig und aggregiert plant, enthält er noch keine diskreten Materialbedarfe; diese entstehen erst mit der Übergabe an die Programmplanung, die die Produktgruppenplanung auf die einzelnen Materialien herunterbricht (Übergabestrategie „Produktionsplan Material(ien) als Anteil PG").[^uebergabe] Dabei entstehen Planprimärbedarfe für die drei Fahrräder, aufgeteilt nach hinterlegtem Anteil – DXTR1 40 %, DXTR2 30 %, DXTR3 30 %.[^planprimaerbedarf] Damit ist der Übergang von der Grobplanung (Kap. 2.2) zur materialgenauen Feinplanung vollzogen; der Planprimärbedarf ist Ausgangspunkt der Bedarfsplanung.

### 3.4 Programmplanung und Bedarfsplanung (MPS/MRP)

Vor dem Planungslauf prüft der Werksleiter in der Programmplanung, ob für die drei Fahrräder Planprimärbedarfe (geplante unabhängige Bedarfe) vorliegen.[^programmplanung-anzeige] Anschließend startet der Fertigungsleiter den Lauf aus Leitteileplanung (MPS) und Materialbedarfsplanung (MRP): Die MPS erzeugt Planaufträge nach den Vorgaben aus SOP und Programmplanung, parallel entstehen über die Stücklistenauflösung Planaufträge für die Sekundärbedarfe. Gesteuert wird der Lauf über Parameter, maßgeblich den Verarbeitungsschlüssel NETCH (Net-Change im gesamten Horizont), der nur geänderte Materialien neu plant, ergänzt um Vorgaben zu Bestellanforderung, Dispositionsliste, Planungsmodus und Eckterminierung.[^mrp-lauf]

Inhaltlich führt das System die in Kap. 2.4 beschriebene Nettobedarfsrechnung durch: Bestand und feste Zugänge werden dem Sicherheitsbestand und den Bedarfen gegenübergestellt; ergibt sich eine Unterdeckung (dispositiv verfügbare Menge kleiner als null), legt die MRP Beschaffungsvorschläge – Planaufträge bzw. Bestellanforderungen – in der durch das Losgrößenverfahren bestimmten Menge an.[^mrp-netto]

Das Ergebnis zeigt die dynamische Bedarfs-/Bestandsliste: Für das rote Fahrrad weist sie zunächst keinen Bestand und keine frei verfügbare Menge aus; die Einträge lassen sich zu Periodensummen aus Planprimärbedarfen, geplanten Zugängen und ATP-Mengen verdichten.[^bedarfsliste] Über den Bedarfsverursacher wird sichtbar, dass der erste Planauftrag den Sicherheitsbestand und den ersten Planprimärbedarf deckt. Damit ist die Kette von der Grobplanung bis zum konkreten Planauftrag (Kap. 2.4) geschlossen.[^bedarfsverursacher]

### 3.5 Fertigungsausführung

Zur Ausführung wird der Planauftrag aus der Bedarfs-/Bestandsliste in einen Fertigungsauftrag umgewandelt. Dabei terminiert das System, prüft die Verfügbarkeit, reserviert die Komponenten laut Stückliste, gibt den Auftrag frei und berechnet die Plankosten.[^umwandlung-praxis] Damit die Fertigung starten kann, werden die zuvor leeren Komponentenbestände über einen Wareneingang ins Lager aufgefüllt – in der Fallstudie vereinfacht ohne den vorgelagerten Beschaffungsprozess.[^wareneingang-lager]

Anschließend bucht der Lagerarbeiter den Warenausgang der Komponenten mit Bezug auf den Fertigungsauftrag über die Bewegungsart 261 (Verbrauch für Auftrag): Die reservierten Materialien werden aus den Lagerorten entnommen – das Aluminiumrad aus dem Halbfabrikatelager (SF00), die übrigen aus dem Rohstofflager (RM00) –, dem Auftrag als Ist-Kosten zugeordnet und über einen Material-, Buchhaltungs- und Kostenrechnungsbeleg fortgeschrieben.[^warenausgang-261] Nach der Montage meldet der Fertigungsarbeiter die Fertigstellung zurück (Endrückmeldung mit Reservierungsausbuchung und Gutmenge); das System berechnet die Fertigungskosten, und der Auftragsstatus wechselt von „freigegeben" zu „rückgemeldet".[^rueckmeldung-praxis]

Den Abschluss bildet der Wareneingang des fertigen Fahrrads in das Fertigerzeugnislager (FG00), mit dem der Wert des hergestellten Materials in den Auftrag fortgeschrieben wird.[^wareneingang-fe] Der Ablauf entspricht damit exakt der in Kap. 2.5 hergeleiteten Logik aus Auftragsumwandlung, bewegungsartengesteuerten Warenbewegungen, Rückmeldung und Belegerzeugung.

### 3.6 Controllingseitiger Abschluss

Nach der Ausführung prüft der Controller in der Fertigungskostenanalyse die dem Auftrag zugeordneten Kosten; die Übersicht stellt summierte Soll- und Ist-Kosten gegenüber und weist Abweichungen aus (die Gemeinkostenzuschläge erscheinen nur in den Soll-Kosten).[^kosten-anzeige] Im letzten Schritt rechnet der Controller den Fertigungsauftrag ab (Istabrechnung im Kostenrechnungskreis NA00, Periode gleich laufender Monat): Die zunächst nur temporär erfassten Kosten werden einem Kostenobjekt zugewiesen und der Auftrag entlastet.[^abrechnung-praxis] Der Lauf erfolgt zunächst als Testlauf mit dem Bericht „Ist/Plan/Abweichung", bevor er als Echtlauf gebucht wird.[^abrechnung-lauf]

Damit ist der in Kap. 2.5 beschriebene controllingseitige Abschluss vollzogen: Die Ist-Kosten werden abgerechnet und über den Soll-Ist- bzw. Plan-Ist-Vergleich transparent gemacht, sodass Abweichungen erkennbar werden und der Kreis von der Planung über die Ausführung bis zur kostenrechnerischen Bewertung geschlossen ist.

### 3.7 Kritische Würdigung des Prozesses

Der Prozess weist deutliche Stärken auf. Zentral ist die durchgängige Integration: Planungs-, Ausführungs- und Abrechnungsdaten liegen auf einer gemeinsamen Datenbasis, sodass jede Warenbewegung automatisch einen Material-, Finanz- und Kostenrechnungsbeleg erzeugt und die Datenkonsistenz sichert.[^wuerdigung-integration] Die Bedarfsableitung ist zudem weitgehend automatisiert – aus dem Planprimärbedarf entstehen über Stücklistenauflösung und Nettobedarfsrechnung ohne manuelle Zwischenschritte Planaufträge (Kap. 3.4) –, und der Ablauf bleibt nachvollziehbar, da sich jeder Bedarf bis zum Verursacher zurückverfolgen lässt.

Diesen Stärken stehen Grenzen gegenüber. Erstens beruht die Planung auf umfangreichen, korrekt zu pflegenden Stammdaten und Parametern – Strategiegruppe, Glättungsfaktoren, MRP-Steuerungsparameter (Kap. 3.2 und 3.4) –, was hohen Pflegeaufwand und Parametrisierungskomplexität bedeutet. Zweitens folgt der Ablauf einer Sukzessivplanung, deren Teilpläne auf Annahmen – etwa über den künftigen Primärbedarf – beruhen, die durch ungeplante Ereignisse verletzt werden können.[^wuerdigung-sukzessiv] Drittens ist die Prognose modellabhängig und grundsätzlich fehlerbehaftet, sodass Über- oder Unterdeckungen möglich bleiben.[^wuerdigung-prognose]

Bezogen auf die Problemstellung aus Kapitel 1 löst der integrierte Ansatz das Abstimmungsproblem aus Nachfrage, Kapazität und Materialverfügbarkeit weitgehend: Er überführt eine Absatzprognose konsistent in konkrete Fertigungs- und Beschaffungsvorschläge und reduziert Intransparenz und isolierte Einzelplanung. Die Güte dieser Lösung hängt jedoch maßgeblich von Datenpflege und Planungsannahmen ab; der Prozess verlagert das Abstimmungsproblem damit von der manuellen Koordination hin zu einer daten- und parametergetriebenen Steuerung.

---

## 4 Fazit

### 4.1 Zusammenfassung der Ergebnisse

Die Arbeit ist der Frage nachgegangen, wie SAP S/4HANA den durchgängigen Planungs- und Steuerungsprozess abbildet und inwieweit dessen einzelne Schritte das Abstimmungsproblem aus Nachfrage, Kapazität und Materialverfügbarkeit lösen. Der theoretische Teil hat die PPS als sukzessive Planungshierarchie auf gemeinsamer Stammdatenbasis hergeleitet – von der Absatz- und Produktionsgrobplanung über Programmplanung und Materialbedarfsplanung bis zu Ausführung und Auftragsabrechnung. Der Praxisteil hat gezeigt, dass die Modellfirma Global Bike genau diese Hierarchie durchläuft: von der prognosebasierten SOP über die materialgenaue Bedarfsplanung und die bewegungsartengesteuerte Fertigung bis zur kostenrechnerischen Abrechnung. Jede Prozessstufe des Systems ließ sich dabei einem theoretischen Konzept zuordnen – etwa die Nettobedarfsrechnung, der Planauftrag als Bindeglied zwischen Planung und Steuerung oder die Strategie „Planung mit Endmontage". Damit überführt der integrierte Ansatz eine Absatzprognose konsistent in Fertigungs- und Beschaffungsentscheidungen und beantwortet die Forschungsfrage im Kern positiv: Die Prozessschritte lösen das Abstimmungsproblem weitgehend – in Abhängigkeit von Datenqualität und Planungsannahmen.

### 4.2 Kritische Reflexion und Ausblick

Die Untersuchung stützt sich auf ein didaktisch vereinfachtes Curriculum-Szenario mit einem Werk, einem Produkt, vorgegebenen Vergangenheitsdaten und einer vereinfachten Beschaffung. Reale Unternehmen sehen sich größerer Datenkomplexität, mehrstufigeren Lieferketten sowie höherem Pflege- und Abstimmungsaufwand gegenüber; die Grundlogik des Prozesses bleibt jedoch übertragbar. Ebenso wurden Kapazitäts- und Verfügbarkeitsprüfungen nur am Rande betrachtet, und der Fokus lag bewusst auf der Make-to-Stock-Fertigung. Eine naheliegende Erweiterung ist daher die Variantenkonfiguration bzw. Kundeneinzelfertigung, die im Curriculum als eigene Fallstudie vorliegt und den Entkopplungspunkt in Richtung Kundenauftrag verschiebt.[^ausblick-vc] Darüber hinaus dürften S/4HANA-Funktionalitäten wie Echtzeitauswertungen (Embedded Analytics) und die In-Memory-Verarbeitung Potenzial eröffnen, Planung und Steuerung weiter zu beschleunigen und transparenter zu gestalten. Künftige Arbeiten könnten den beschriebenen Prozess um diese Aspekte erweitern und an einem komplexeren, realitätsnäheren Szenario validieren.

---

## Literaturverzeichnis

- Boldau, Michael / Wagner, Bret / Weidner, Stefan (2023): Global Bike. Fallstudie Produktionsplanung und -steuerung (PP), Version 4.2, Magdeburg: SAP UCC.
- Kessler, Alexander / Vogt, Josua / Stephan, Max / Himburg, Marcel (2023): Global Bike. Fallstudie Variantenkonfiguration (VC), Version 4.2, Magdeburg: SAP UCC.
- Kurbel, Karl (2021): ERP und SCM. Enterprise Resource Planning und Supply Chain Management in der Industrie, 9. Aufl., Berlin/Boston: De Gruyter Oldenbourg. ISBN 978-3-11-070118-0; DOI 10.1515/9783110701203.
- SAP UCC Magdeburg (2023): Einführung in S/4HANA mit Global Bike. Foliensatz Produktionsplanung und -steuerung (PP), Version 4.2, Magdeburg.

---

## Kennzeichnung des KI-Einsatzes

Zur Strukturierung und sprachlichen Ausformulierung von Textentwürfen wurde ein KI-gestütztes Sprachmodell als Hilfsmittel eingesetzt. Die inhaltliche Verantwortung, die Auswahl und Prüfung der Fachquellen sowie die fachliche Richtigkeit liegen vollständig bei der/dem Verfasser/in. Die KI-Unterstützung ersetzt keine Fachquelle.

---

### Fußnoten

[^pps-relevanz]: Vgl. Kurbel (2021), S. 22 ff.

[^isoliert]: Vgl. Kurbel (2021), S. 22 ff.; ergänzend SAP UCC Magdeburg (2023), Folie 27.

[^erp]: Vgl. Kurbel (2021), S. 212 ff.

[^strategie]: Vgl. SAP UCC Magdeburg (2023), Folie 32 f.; Boldau/Wagner/Weidner (2023), S. 1.

[^curriculum]: Vgl. Boldau/Wagner/Weidner (2023); SAP UCC Magdeburg (2023).

[^def-pps]: Vgl. Kurbel (2021), S. 16 ff.

[^sap-typen]: Vgl. SAP UCC Magdeburg (2023), Folie 5.

[^mrp2]: Vgl. Kurbel (2021), S. 101 ff.

[^plan-steuer]: Vgl. Kurbel (2021), S. 16 ff.; ergänzend SAP UCC Magdeburg (2023), Folie 25 f.

[^sukzessiv]: Vgl. Kurbel (2021), S. 20 f.

[^sap-stufen]: Vgl. SAP UCC Magdeburg (2023), Folie 24 f.

[^ebenen]: Vgl. SAP UCC Magdeburg (2023), Folie 25 f.

[^prognose]: Vgl. SAP UCC Magdeburg (2023), Folie 27 f.

[^prognose-fehler]: Vgl. SAP UCC Magdeburg (2023), Folie 27.

[^programm]: Vgl. SAP UCC Magdeburg (2023), Folie 29 ff.

[^stammdaten]: Vgl. Kurbel (2021), S. 43 ff.; ergänzend SAP UCC Magdeburg (2023), Folie 11.

[^matstamm]: Vgl. Kurbel (2021), S. 44 ff.; SAP UCC Magdeburg (2023), Folie 12.

[^stueckliste]: Vgl. SAP UCC Magdeburg (2023), Folie 13 ff.; Kurbel (2021), S. 45 ff.

[^arbeitsplan]: Vgl. SAP UCC Magdeburg (2023), Folie 18 f.; Boldau/Wagner/Weidner (2023), S. 9.

[^arbeitsplatz]: Vgl. SAP UCC Magdeburg (2023), Folie 7 u. 20 f.

[^produktgruppe]: Vgl. SAP UCC Magdeburg (2023), Folie 22.

[^mrp-zweck]: Vgl. SAP UCC Magdeburg (2023), Folie 35 u. 37; ergänzend Kurbel (2021), S. 42 ff.

[^netto]: Vgl. SAP UCC Magdeburg (2023), Folie 38.

[^losgroesse]: Vgl. SAP UCC Magdeburg (2023), Folie 39.

[^beschaffungsart]: Vgl. SAP UCC Magdeburg (2023), Folie 40.

[^mrp-terminierung]: Vgl. SAP UCC Magdeburg (2023), Folie 41.

[^stueckaufloesung]: Vgl. SAP UCC Magdeburg (2023), Folie 35 u. 37; Kurbel (2021), S. 88 ff.

[^planauftrag]: Vgl. SAP UCC Magdeburg (2023), Folie 44 f.

[^auftragsarten]: Vgl. SAP UCC Magdeburg (2023), Folie 45.

[^umwandlung]: Vgl. SAP UCC Magdeburg (2023), Folie 44 f.

[^fertauftrag]: Vgl. SAP UCC Magdeburg (2023), Folie 47 f.

[^freigabe]: Vgl. SAP UCC Magdeburg (2023), Folie 49 f. u. 52.

[^warenausgang]: Vgl. SAP UCC Magdeburg (2023), Folie 54; Boldau/Wagner/Weidner (2023), S. 34 f. (Bewegungsart 261).

[^rueckmeldung]: Vgl. SAP UCC Magdeburg (2023), Folie 55.

[^wareneingang]: Vgl. SAP UCC Magdeburg (2023), Folie 56.

[^abrechnung]: Vgl. SAP UCC Magdeburg (2023), Folie 57 ff.

[^strategien-arten]: Vgl. SAP UCC Magdeburg (2023), Folie 32.

[^strategien-lager]: Vgl. SAP UCC Magdeburg (2023), Folie 33.

[^opp]: Vgl. Kurbel (2021), S. 35 ff. u. 173 ff.

[^gb-strategie40]: Vgl. SAP UCC Magdeburg (2023), Folie 33; Boldau/Wagner/Weidner (2023), S. 6.

[^gb-unternehmen]: Vgl. SAP UCC Magdeburg (2023), Folie 8; Boldau/Wagner/Weidner (2023), S. 2.

[^gb-orga]: Vgl. SAP UCC Magdeburg (2023), Folie 7 ff.

[^gb-produkt]: Vgl. Boldau/Wagner/Weidner (2023), S. 4; SAP UCC Magdeburg (2023), Folie 13 ff.

[^gb-produktgruppe]: Vgl. Boldau/Wagner/Weidner (2023), S. 12.

[^gb-rollen]: Vgl. Boldau/Wagner/Weidner (2023), S. 2.

[^matstamm-praxis]: Vgl. Boldau/Wagner/Weidner (2023), S. 6.

[^arbeitsplan-praxis]: Vgl. Boldau/Wagner/Weidner (2023), S. 8 f.

[^komponenten-praxis]: Vgl. Boldau/Wagner/Weidner (2023), S. 9 f.

[^sop-prognose]: Vgl. Boldau/Wagner/Weidner (2023), S. 14 ff.

[^sop-produktionsplan]: Vgl. Boldau/Wagner/Weidner (2023), S. 16 f.

[^uebergabe]: Vgl. Boldau/Wagner/Weidner (2023), S. 18.

[^planprimaerbedarf]: Vgl. Boldau/Wagner/Weidner (2023), S. 19.

[^programmplanung-anzeige]: Vgl. Boldau/Wagner/Weidner (2023), S. 20 f.

[^mrp-lauf]: Vgl. Boldau/Wagner/Weidner (2023), S. 22.

[^mrp-netto]: Vgl. Boldau/Wagner/Weidner (2023), S. 23.

[^bedarfsliste]: Vgl. Boldau/Wagner/Weidner (2023), S. 25 f.

[^bedarfsverursacher]: Vgl. Boldau/Wagner/Weidner (2023), S. 27.

[^umwandlung-praxis]: Vgl. Boldau/Wagner/Weidner (2023), S. 28 f.

[^wareneingang-lager]: Vgl. Boldau/Wagner/Weidner (2023), S. 31.

[^warenausgang-261]: Vgl. Boldau/Wagner/Weidner (2023), S. 34 f.

[^rueckmeldung-praxis]: Vgl. Boldau/Wagner/Weidner (2023), S. 40 ff.

[^wareneingang-fe]: Vgl. Boldau/Wagner/Weidner (2023), S. 44 f.

[^kosten-anzeige]: Vgl. Boldau/Wagner/Weidner (2023), S. 46 f.

[^abrechnung-praxis]: Vgl. Boldau/Wagner/Weidner (2023), S. 48.

[^abrechnung-lauf]: Vgl. Boldau/Wagner/Weidner (2023), S. 49 ff.

[^wuerdigung-integration]: Vgl. Kurbel (2021), S. 212 ff.; SAP UCC Magdeburg (2023), Folie 56.

[^wuerdigung-sukzessiv]: Vgl. Kurbel (2021), S. 33.

[^wuerdigung-prognose]: Vgl. SAP UCC Magdeburg (2023), Folie 27.

[^ausblick-vc]: Vgl. Kessler/Vogt/Stephan/Himburg (2023).
