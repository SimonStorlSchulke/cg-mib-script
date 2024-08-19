---
title: 3. Signals
---

Oft brauchen wir bei der Gestalltung von 3D Szenen und Spielen Möglichkeiten zur Interaktion zwischen verschiedenen Objekten in der Szene. Hier kommen [Signals](https://docs.godotengine.org/en/stable/getting_started/step_by_step/signals.html) ins Spiel. Das sind Nachrichten, die Nodes aussenden, wenn etwas Bestimmtes mit ihnen passiert, z.B. wenn ein Button gedrückt wird oder ein Objekt mit einem Collider kollidiert. Andere Nodes können sich mit diesem Signal verbinden und eine Funktion aufrufen, wenn das Ereignis eintritt.

{{<info>}}
**Warum Signals statt einfach Methoden auf anderen Objekten direkt aufrufen?**  
Signals bieten den Vorteil, dass ihnen erstmal egal ist, wer der Empfänger des Signals ist - oder obe es überhaupt einen gibt. Objelte die mit Signalen arbeiten sind somit wesentlich flexibler und modularer einsetzbar als "hardvertratete" Objekte. In Godot gibt es das Prinzip "Call up, Signal down". Bedeutet: Hierarchisch übergeordnete Objekte sollten ihren "Kindern" Signale senden, auf die diese dann reagieren. z.B. sollte ein "Game Manager" Objekt nicht direkt Methoden in Spielcharakteren aufrufen um nicht technisch von ihnen abhängig zu sein. Umgekehrt könnte Z.b. könnte ein Spielercharakter (Child) seine Position an den Game Manager (Parent) melden, indem er eine Funktion im Game Manager aufruft. Diese Art der Abhängigkeit ist akzeptabel, da Spielcharaktere in diesem Beispiel ohne Gamemanager sowieso nicht funktionieren können sollten.
{{</info>}}

## UI mit Signals bauen

Wir wollen für unseren Terraingenerator nun eine Benutzeroberfläche bauen, in die wir die Parameter des Generators eingeben können und daraufhin ein Signal an unser Terrain schicken, um es neu zu generieren.

{{<todo>}}
**Baue zunächst die grafische Benutzeroberfläche für den Terraingenerator.**  
Nutze z.B. das "SpinBox" Element, um ein Zahleneingabefeld zu erzeugen. Erstelle Eingabemasken für alle wichtigen parameter des Terraingenerators und beschrifte sie (Labels). Verwendet "HBox" und "VBox" Nodes, um die Elemente anzuordnen. Mehr Infos zu UI findet ihr in der [Dokumentation](https://docs.godotengine.org/en/stable/tutorials/ui/index.html).
{{</todo>}}

## Ressourcen & Tutorials zum Thema

| Art/Länge | Titel | Beschreibung | Quelle |
|---|---|---|---|

https://gdscript.com/solutions/signals-godot/