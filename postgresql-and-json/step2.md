__Hinweis:__
In diesem Tutorial verwenden wir den von PostgreSQL zur Verfügung gestellten Datentyp _jsonb_. 
Dieser ist im Gegensatz zu _json_ deutlich schneller und ermöglicht die Indexierung von Einträgen.
Einzige Nachteile sind, dass _jsonb_ keine Leerzeichen speichert, keine doppelten Schlüssel erlaubt und die Reihenfolge der Einträge varieren kann.

__Bevor du loslegst:__  
Bitte schaue rechts im Terminal, ob du `postgres=#` siehts. Falls ja, kannst du loslegen! Springe zu __Tabelle erzeugen__  
Falls du `root@28e8ad58ddf9:` oder ähnlich sehen solltest, führe bitte `psql -U postgres`{{exec}} aus. Funktioniert das nicht, lade die Seite bitte neu und warte ein paar Sekunden.


### Tabelle erzeugen
Wir wollen Rechnungen speichern, da eine Rechnung generell immer die Adresse und alle Artikel gleichzeitig listet, verwenden wir das verschachtelte Prinzip. (Siehe Tabelle von Schritt 1)  
Bevor wir Einträge hinzufügen können, benötigen wir zuerst eine Tabelle.

Erstelle eine neue Tabelle "rechnungen" mit dem Feld "rechnungsNr" (Schlüssel INT) und "details" (JSONB)

`CREATE TABLE rechnungen (rechnungsNr INT PRIMARY KEY, details JSONB);`{{execute}}

Überprüfe ob deine Tabelle erfolgreich erstellt wurde:

`\d rechnungen`{{exec}}

### Einträge hinzufügen
Wir wollen nun die folgende Rechnungen hinzufügen.
```
rechnungsNr = 23774
{ 
	"Name":"Hans Peter", 
	"Datum":"22-03-2021 23:32:00", 
	"KundenNr": "2465433",
	"Adresse": { "Strasse": "Silberalle 12", "PLZ": "73357","Ort": "München" },
	"Artikel":[
		{ "Name": "Pullover", "Preis": "35.00" },
		{ "Name": "Socken", "Preis": "7.95" },
		{ "Name": "Jeans", "Preis": "65.99" }
	]
}')
```

Wir benutzen das, wie von SQL gewohnte, INSERT Statement und geben die JSON-Daten als String mit:

`INSERT INTO rechnungen (rechnungsNr, details) VALUES (23774, '{ 
	"Name":"Hans Peter", 
	"Datum":"22-03-2021 23:32:00", 
	"KundenNr": "2465433",
	"Adresse": { "Strasse": "Silberalle 12", "PLZ": "73357","Ort": "München" },
	"Artikel":[
		{ "Name": "Pullover", "Preis": "35.00" },
		{ "Name": "Socken", "Preis": "7.95" },
		{ "Name": "Jeans", "Preis": "65.99" }
	]
}');`{{execute}}


Den kompletten JSON-Eintrag bekommen wir mit dem gewohnten Statement zurück:

`SELECT details FROM rechnungen WHERE rechnungsNr = 23774;`{{execute}}

Analog zu diesen Befehlen verhält sich das DELETE oder UPDATE Statement, was du aber sicherlich schon kennst. 
Spannender wird es im nächsten Schritt, hier arbeiten wir mit dem JSON-Eintrag direkt 🧐

Klicke am besten noch auf die Datei unten, damit du dir immer die Struktur unserer Rechnung vor Augen holen kannst, wenn du sie brauchst.

`StrukturRechnung.js`{{open}}
