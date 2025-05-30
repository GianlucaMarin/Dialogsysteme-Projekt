# Dialogsysteme-Projekt

# 🥗 NutriBot – Projektzusammenfassung (Modul Dialogsysteme)

Im Rahmen des Moduls *Dialogsysteme* haben wir den Chatbot **NutriBot** konzipiert und in Voiceflow umgesetzt. Ziel des Projekts war es, ein Dialogsystem zu entwickeln, das Nutzer:innen dabei unterstützt, mit vorhandenen Zutaten gesunde Mahlzeiten zu planen – und dabei die Makronährstoffe (Proteine, Kohlenhydrate, Fette) im Blick zu behalten.

Zu Beginn des Projekts stand die **Definition des Use Cases** im Fokus. Wir stellten uns die Frage, welches konkrete Problem unser Bot lösen soll. Dabei identifizierten wir mehrere Herausforderungen: Viele Menschen wollen sich gesünder ernähren, wissen jedoch nicht, wie sie vorhandene Zutaten sinnvoll kombinieren können, haben kein Verständnis für Makronährstoffe oder verlieren bei der Alltagsplanung schnell den Überblick über ihre Nährstoffaufnahme. Aus diesen Überlegungen ergab sich unser Ziel: Ein digitaler Assistent, der sowohl **Essensvorschläge basierend auf verfügbaren Zutaten liefert** als auch **Makros transparent berechnet und mittrackt**.
<img width="354" alt="Definition des Use Cases für NutriBot" src="https://github.com/user-attachments/assets/6db77d54-3e93-4c48-a914-529c6b6fbdfb" />


Im nächsten Schritt entwickelten wir eine **User Persona**, um die Bedürfnisse und Erwartungen unserer Zielgruppe besser zu verstehen. Die Persona „Mans Hustermann“ steht dabei stellvertretend für gesundheitsbewusste, aktive Menschen, die Wert auf einfache, funktionale Ernährungslösungen legen, jedoch Unterstützung bei der Umsetzung brauchen – sei es bei der Makroberechnung, bei der Ideenfindung oder beim Tracken von Mahlzeiten.

Basierend darauf konzipierten wir eine passende **Bot-Persona**. NutriBot ist freundlich, verständlich und ermutigend im Ton – bewusst so gewählt, um einen niedrigschwelligen Einstieg zu ermöglichen. Der Bot vermeidet Fachjargon, gibt motivierende Rückmeldungen und unterstützt bei Ernährungsfragen ohne belehrend zu wirken.

Und zuletzt haben wir uns noch mit der Architekur des Chatbots beschäftigt. Für die technische Umsetzung von NutriBot haben wir eine modulare Architektur gewählt, die eine klare Trennung zwischen Dialogführung, Datenzugriff und Kanälen ermöglicht. Ziel war es, eine Lösung zu schaffen, die sowohl funktional als auch zukunftssicher ist.

**1. Datenbedarf:**  
Um relevante Empfehlungen zu geben und Makronährstoffe zu berechnen, benötigt der Bot Zugriff auf:
- Eine **Nährwertdatenbank** (z. B. OpenFoodFacts), um Makros und Inhaltsstoffe abzurufen.
- Eine potenzielle **Barcode-Scan-Funktion**, um gescannte Lebensmittel zu identifizieren.
- Persönliche Präferenzen wie Allergien oder Fitnessziele, um Vorschläge zu personalisieren.
- Eine **Rezeptdatenbank**, die auf Basis vorhandener Zutaten passende Gerichte vorschlägt.

**2. Kanäle & UI:**  
Für die erste Version wurde der Bot über eine Web-App nutzbar gemacht. Die Hauptidee war, strukturierte Gespräche über eine einfache Oberfläche zu ermöglichen.  
Geplante oder denkbare Kanäle sind:
- **Primär:** Webchat und mobile App – für klar geführte Nutzerinteraktionen
- **Optional:** WhatsApp oder Telegram – für spontane, alltagstaugliche Eingaben  
Die Kanalwahl wurde bewusst so getroffen, dass **Nutzerfreundlichkeit und Alltagstauglichkeit** im Vordergrund stehen.

**3. Framework:**  
Voiceflow wurde als Entwicklungsplattform gewählt, weil es eine intuitive Gestaltung von Konversationsflüssen erlaubt. Zusätzlich können externe APIs eingebunden, Variablen verwaltet und einfache JavaScript-Operationen durchgeführt werden. Damit liess sich die gesamte Logik – von der Benutzerführung bis zur Makroberechnung – **visuell umsetzen und dennoch flexibel steuern**.

**4. Externe Aktionen & APIs:**  
Für die Verarbeitung und Anreicherung der Nutzereingaben wurden externe Schnittstellen eingebunden oder vorbereitet:
- Die **OpenFoodFacts API** liefert Nährwerte und Produktinformationen.
- Eine zukünftige Integration mit **MyFitnessPal oder Apple Health** ermöglicht es, Kalorien- und Makrodaten automatisch zu erfassen und in die tägliche Bilanz einzurechnen.  
So kann NutriBot langfristig als umfassender Begleiter in der Ernährung eingesetzt werden.

Die gewählte Architektur ist bewusst offen gestaltet, um Erweiterungen wie Rezeptvorschläge per Spracheingabe, Nutzerfeedback-Loops oder Machine-Learning-basierte Empfehlungen zu ermöglichen – **ohne die bestehende Struktur zu beeinträchtigen**.


Für die Umsetzung entschieden wir uns für Voiceflow, da es eine visuelle, intuitive Entwicklung von Konversationslogiken erlaubt. Wir haben zwei zentrale Dialog-Flows erstellt:

1. **Onboarding-Flow**

Der Onboarding-Flow stellt den Einstiegspunkt für Nutzer:innen dar und ist in zwei Hauptpfade unterteilt: Login und Registrierung (Signup). Ziel ist es, neue Benutzer:innen zu erfassen, bestehende anzumelden und gleichzeitig die grundlegenden persönlichen Informationen einzuholen, die für spätere Empfehlungen benötigt werden.

Zu Beginn begrüsst der Bot die Nutzer:innen mit einer Nachricht, die erklärt, dass ein Konto erforderlich ist. Anschliessend wird ihnen die Wahl zwischen „Login“ und „Signup“ angeboten. Diese Entscheidung führt zu zwei separaten Pfaden mit jeweils eigener Logik und Validierung.

### Login-Pfad

Wählen die Nutzer:innen den Login-Pfad, wird zunächst ihre E-Mail-Adresse abgefragt. Diese wird über einen POST-Request an Make übermittelt, wo automatisiert geprüft wird, ob die E-Mail-Adresse bereits in der hinterlegten Datenbank vorhanden ist. Abhängig vom Rückgabestatus erfolgt die weitere Navigation: 

Wird die E-Mail erkannt (Status = true), fragt der Bot das Passwort ab. Auch dieses wird an Make übermittelt und dort validiert. Bei erfolgreicher Authentifizierung erhalten die Nutzer:innen eine Bestätigung („Login successful“) und können direkt in den weiteren Anwendungsbereich übergehen. Ist das Passwort hingegen falsch, wird eine entsprechende Fehlermeldung angezeigt, mit der Möglichkeit, es erneut einzugeben.

Wird die eingegebene E-Mail nicht erkannt (Status = false), fragt der Bot, ob ein neues Konto erstellt werden soll. Lehnt die Person dies ab, endet der Dialog mit einer Verabschiedung. Stimmt sie zu, wird nahtlos in den Signup-Pfad gewechselt.

### Signup-Pfad

Im Registrierungspfad wird ebenfalls zunächst die E-Mail-Adresse abgefragt und via Make geprüft. Falls diese bereits existiert, wird eine Warnung ausgegeben („E-Mail already exists – please try to login“) und der Nutzer zum Login zurückgeführt. Ist die Adresse neu, beginnt das eigentliche Onboarding.

Der Bot führt nun einen dialogischen Fragebogen durch, um die persönlichen Eckdaten der Nutzer:innen zu erfassen: Grösse, Gewicht, Geschlecht und individuelles Ziel (z. B. Muskelaufbau, Abnehmen oder Gewicht halten). Diese Eingaben werden als Variablen gespeichert und bilden die Grundlage für die personalisierte Betreuung durch NutriBot.

Im Anschluss folgt ein automatisierter Kalorienrechner – ein in Voiceflow eingebetteter JavaScript-Block –, der den ungefähren täglichen Kalorienbedarf der Person berechnet. Dieser Wert wird abschliessend in einem separaten Block ausgegeben („Your estimated daily calorie requirement is approximately 2'300 kcal“) und steht dem System für spätere Empfehlungen zur Verfügung.

### Technische Umsetzung und Besonderheiten

Die Umsetzung erfolgt vollständig in Voiceflow, wobei mehrere Blöcke mit denselben Namen („New Block 7“, „New Block 8“, etc.) verwendet werden – jedoch mit klarer Trennung nach Kontext (Login vs. Signup). Durch diese Struktur lässt sich der Flow visuell kompakt halten, ohne an Klarheit zu verlieren.

Externe Anfragen und Datenverarbeitung laufen über Webhooks zu **Make.com**, was eine einfache, aber leistungsstarke Möglichkeit darstellt, auf Benutzereingaben dynamisch zu reagieren, Daten zu speichern oder zu verifizieren.

Besonders hervorzuheben ist die durchdachte Fehlerbehandlung: Falsche Passworteingaben, doppelte E-Mails und nicht gefundene Nutzer:innen werden jeweils aufgefangen und mit passenden Alternativen gelöst. Diese Logik stärkt die Nutzerfreundlichkeit und sorgt für ein robustes Dialogerlebnis.

Abschliessend wird der Flow mit einer Übergabe an den zweiten Haupt-Workflow „meal and track“ abgeschlossen, in dem die Interaktion fortgeführt und vertieft wird.

2. **Meal and Track-Flow**

Der zweite Haupt-Flow in unserem Projekt ist der „meal and track“-Flow. Nachdem die Nutzer:innen sich registriert oder eingeloggt und ihre Kalorienziele im Onboarding-Flow definiert haben, beginnt hier die eigentliche Interaktion rund um Ernährung, Tracking und Vorschläge.

Nach dem Einstieg begrüsst NutriBot die Person und zeigt das aktuelle Ernährungsziel an – z. B. Muskelaufbau oder Fettabbau. Anschliessend bietet er drei Hauptoptionen an: Kalorien und Makronährstoffe tracken, Vorschläge für gesunde Mahlzeiten erhalten oder den Bot beenden. Die Entscheidung, dem Nutzer diese Auswahl zu überlassen, basiert auf dem Prinzip der Eigenverantwortung und Motivation: Der Bot agiert wie ein Coach, aber ohne Druck.

### 1. Kalorien-Tracking

Wenn Nutzer:innen sich entscheiden, ihre Kalorienaufnahme zu verwalten, führt NutriBot sie zunächst zu einem Auswahlmenü in „New Block 6“. Hier stehen zwei Optionen zur Verfügung: „Add Calories“ oder „View Daily Calories“. Diese Trennung wurde bewusst gewählt, um je nach Ziel eine gezielte Aktion zu ermöglichen.

**Wird die Option „Add Calories“ gewählt**, fragt der Bot, wie viele Kalorien der Nutzer aufgenommen hat. Diese Information wird gespeichert und via POST-Request an Make übertragen, wo die bisherigen Kalorienwerte für den aktuellen Tag abgerufen und aktualisiert werden. Anschliessend übernimmt „New Block 8“ die Rückmeldung: Ein JavaScript-Block berechnet die neue Gesamtsumme, die dem Nutzer zurückgemeldet wird, etwa mit der Nachricht „Your total calories are now: 1'850 kcal – database is updated :)“. Dies schafft Transparenz und motiviert zur Selbstkontrolle.

**Wird hingegen „View Daily Calories“ gewählt**, verzweigt der Flow zu einem zweiten „New Block 9“, in dem einfach die bisherige Tagesbilanz angezeigt wird. Auch hier erfolgt der Abruf über Make, jedoch ohne Änderung an der Datenbank – das System bleibt lesend und informativ.

Durch diese saubere Trennung zwischen Abfrage und Aktualisierung wird vermieden, dass Nutzer:innen versehentlich Daten überschreiben oder ungewollte Eingaben tätigen. Gleichzeitig bleibt die Interaktion effizient und leicht verständlich.

Abschliessend führt der Bot die Nutzer:innen über einen weiteren Block mit der Frage „What would you like to do now?“ zurück zum Hauptmenü des Flows. Dadurch können sie direkt weitere Funktionen nutzen – etwa neue Kalorien hinzufügen, bestehende Werte prüfen oder in die Rezeptvorschläge wechseln – ohne den Dialog verlassen zu müssen. Diese Rückführung trägt zu einem flüssigen Nutzungserlebnis bei und vermeidet unnötige Wiederholungen oder Neustarts des Flows.

### 2. Rezeptvorschläge

Alternativ können sich Nutzer:innen vom Bot Rezeptvorschläge anzeigen lassen. Auch hier gibt es zwei Wege:
- Entweder wird ein konkretes Gericht gewünscht („Ich will Pasta essen“),
- oder es wird mit vorhandenen Zutaten gearbeitet („Ich habe Hähnchen, Brokkoli und Reis“).

Im ersten Fall fragt der Bot nach dem gewünschten Gericht. Die Eingabe wird in einer Variable wie `food_wish` gespeichert und intern mit den eingebetteten Gerichten (KB) verglichen, die im Projekt als Datenquellen (in Textformat) hinterlegt sind. Passend zur Anfrage gibt der Bot dann einen Vorschlag zurück.

Im zweiten Fall fragt der Bot nach den verfügbaren Zutaten. Auch diese Eingabe wird mit der selben Tabelle abgeglichen. Der Bot sucht passende Rezeptkombinationen und schlägt dem Nutzer ein Gericht vor, das möglichst viele der vorhandenen Zutaten verwendet.

Nach jedem Rezeptvorschlag – unabhängig davon, ob dieser auf einer konkreten Wunschmahlzeit oder auf vorhandenen Zutaten basiert – fragt NutriBot gezielt: „Are you happy with the result?“. Wird die Frage mit „No“ beantwortet, folgt eine zweite Nachfrage: „Why not? Would you like to try again or go back to the main menu?“. Die Nutzer:innen können daraufhin entweder eine neue Auswahl treffen und einen weiteren Vorschlag erhalten oder direkt in das Hauptmenü zurückkehren. Diese doppelte Rückfrage sorgt dafür, dass die Nutzer:innen nicht in einer Sackgasse landen und jederzeit die Kontrolle über die Dialogführung behalten.

### 3. Technische Integration & Fehlerbehandlung

Im Gegensatz zum Onboarding-Flow, bei dem externe Dienste wie Make zur Anwendung kommen, ist die technische Logik im „meal and track“-Flow vollständig innerhalb von Voiceflow umgesetzt. Die Rezeptvorschläge – sowohl für konkrete Wunschmahlzeiten als auch für Kombinationen basierend auf vorhandenen Zutaten – basieren auf intern eingebundenen Textdateien. Zu jedem einzelnen Rezept wurde eine eigene Textdatei erstellt um Designprobleme zu einem späteren Zeitpunkt zu vermeiden. Zu Beginn haben wir mit Excel-Dateien gearbeitet, dies führte bei der Anzeige im Agent jedoch zu Designproblemen, entsprechend haben wir uns für einzelne Textdokumente entschieden. Diese wurde als Datenquelle im Projekt integriert und ermöglicht den Zugriff auf gespeicherte Rezepte, ohne auf externe APIs angewiesen zu sein. Die Eingaben der Nutzer:innen werden über Variablen (z. B. `food_wish`) gespeichert und gegen die lokal eingebettete Datensammlung geprüft.

Die Interaktionen im Kalorien-Tracking verlaufen ebenfalls vollständig innerhalb von Voiceflow. Die Nutzer:innen können Kalorienwerte hinzufügen oder ihren aktuellen Tagesstand abfragen. Auch hier wurde auf eine saubere Trennung zwischen Eingabe und Anzeige geachtet, um Fehler zu vermeiden. Die Ergebnisse werden direkt angezeigt, ohne dass Daten in einer externen Umgebung gespeichert werden müssen.

Besonderes Augenmerk wurde auf die Fehlerbehandlung und Rückführung gelegt. Nach jeder Rezeptempfehlung fragt NutriBot, ob der Vorschlag gefällt. Wird dieser abgelehnt, erscheint eine gezielte Nachfrage: „Would you like to try again or go back to the main menu?“. Diese zusätzliche Entscheidungsstufe verhindert Sackgassen im Dialogverlauf und erlaubt den Nutzer:innen, sich entweder einen neuen Vorschlag anzeigen zu lassen oder zum Hauptmenü zurückzukehren. Dadurch bleibt der Flow flexibel, nachvollziehbar und nutzerfreundlich.

Um Variablen, wie zum Beispiel die E-Mail, welche im Onboarding-Workflow gesetzt wird, auch Workflowübergreifend zu verwenden, haben wir mithilfe des "Set"-Logic eine Sessionvariable gesetzt. Nun kann die "temp_mail" in allen neuen oder bereits vorhanden Workflows verwendet werden. Hier das erwähnte Beispiel:

<img width="354" alt="Bildschirmfoto 2025-05-29 um 16 04 56" src="https://github.com/user-attachments/assets/891dd6c4-fae4-4ae3-a380-f9de0f7cb879" />

Ein weiterer Prompt wurde hinzegfügt um herauszufinden, mit welchen Zutaten der User genau kochen möchte. Da der User wie oben beschrieben, z.B. „Ich habe Hähnchen, Brokkoli und Reis“, als Anfrage senden kann, müssen wir die einzelnen Zutaten herausfiltern, um mit diesen die KB abzufragen. Dieser Prompt ist lediglich für die Extraktion von Nahrungsmittel aus der Nachricht des Users zuständig. Dieser sieht wie folgt aus:

<img width="746" alt="Bildschirmfoto 2025-05-29 um 16 09 28" src="https://github.com/user-attachments/assets/6db77d54-3e93-4c48-a914-529c6b6fbdfb" />

----------------------------------------

Insgesamt war das Projekt eine spannende Gelegenheit, theoretisches Wissen über dialogbasierte Systeme mit praktischer Umsetzung zu verbinden. Der iterative Aufbau über Persona-Definition, Use Case, Architektur und prototypische Dialoggestaltung hat uns geholfen, ein funktionales, zielgruppenorientiertes System zu entwickeln, das reale Mehrwerte liefert.
