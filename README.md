# Card Safari – Ein strategisches Kartenspiel mit Tieren 🃏🦁

## 🚀 Projektübersicht
Card Safari ist ein Kartenspiel, in dem Spieler Tierkarten sammeln und in strategischen Duellen gegeneinander antreten. Jeder Spieler vergleicht die Werte seiner Karten in mehreren Runden, wobei die Karte mit dem höheren Wert gewinnt.

## 🖥️ Hauptfunktionen
- Kartenduelle: Spieler vergleichen in mehreren Runden die Werte ihrer Karten.
- Tierfakten-Generator: Mit Hilfe einer ChatGPT-API werden zufällige Tierfakten generiert.
- Kartenübersicht: Eine Sammlung aller Karten mit einer Filterfunktion zur Suche nach Tierarten.

  
## 🏆 Besondere Herausforderungen & Lösungen
- Online-Gegner simulieren: Es wurde eine Simulation eines Matchmakings integriert, bei dem der Spieler gegen eine KI mit zufällig generierten Namen und Profilbildern antritt, um das Spiel realistischer wirken zu lassen. Für die zufällige Generierung von Gegnernamen wird die Randommer.io API verwendet. Diese API bietet eine einfache Möglichkeit, zufällige Namen zu erstellen, um die Spielwelt realistischer und dynamischer zu gestalten.
- Mobile Ansicht der Spielseite: Die Gestaltung der Spielseite für mobile Geräte stellt eine Herausforderung dar, da der begrenzte Platz es schwierig macht, alle Karten ordentlich darzustellen. Besonders bei der Anzeige mehrerer Karten auf kleinen Bildschirmen könnte es problematisch sein, eine benutzerfreundliche Ansicht zu schaffen. Dies ist ein Bereich, den man in Zukunft weiter optimieren könnte.

## 🎮 Spielregeln
1.  Jeder Spieler erhält 5 Karten mit einzigartigen Tieren und deren Werten. Jede Karte hat unterschiedliche Attribute wie Gewicht, Länge, maximale Geschwindigkeit und mehr, die in den Duellen verglichen werden.
2.  In jeder Runde wird zufällig einer der sechs Werte gewählt. Die verfügbaren Werte können z. B. Gewicht, Länge, maximale Geschwindigkeit, maximale Lebensdauer und andere interessante Attribute der Tiere sein.
Der Spieler, dessen Karte den höheren Wert in dieser Runde hat, gewinnt die Runde. Bei einem Gleichstand verschwinden die Karten.
3.  Jede gewonnene Runde bringt den Spieler dem Sieg näher. Der Spieler mit den meisten Siegen nach den festgelegten Runden gewinnt das Duell!

## 🌻 Could have

- Zeitlimit-Modus: Ein zusätzlicher Spielmodus, bei dem jede Runde ein festgelegtes Zeitlimit hat, um die Spielgeschwindigkeit zu erhöhen.
- Turniermodus: Spieler können sich in einem Turnierformat messen, mit mehreren Duellen und steigender Schwierigkeit.
