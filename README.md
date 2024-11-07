# Crashy Bird
## ~avatar avatar @unplugged

Der ganze Spaß des Spiels "Flappy Bird" jetzt auf deinem @boardname@ als "Crashy Bird"!

## ~ @unplugged

Dies ist eine einfache Variante des Spiels Flappy Bird für den @boardname@.
Das Ziel ist es, einen fliegenden Vogel, der sich kontinuierlich nach rechts bewegt, durch eine Menge Hindernisse zu steuern.
Wenn der Spieler ein Hindernis berührt, hat er verloren.
Der Zweck dieser Anleitung ist es, die Grundlagen von Sprites, Arrays und Schleifen zu vermitteln.

## Schritt 1: Füge den Vogel zum Spiel hinzu

Als erstes werden wir einen Sprite für den Vogel aus dem Spiel-Menü hinzufügen und ihn blinken lassen.
```blocks
let vogel: game.LedSprite = null
vogel = game.createSprite(0, 2)
vogel.set(LedSpriteProperty.Blink, 300)
```
## Schritt 2: Lass den Vogel fliegen

Vor der Erstellung des Codes für die Spielaktionen, sollten wir zuerst eine Möglichkeit der Steuerung hinzufügen, damit wir den Vogel bewegen können.
Wir werden den Vogel steuern, indem wir Knopf A drücken, um ihn nach oben zu bewegen oder den Knopf B drücken, damit er sich nach unten bewegt.
```blocks
let vogel: game.LedSprite = null

input.onButtonPressed(Button.A, () => {
    vogel.change(LedSpriteProperty.Y, -1)
})
input.onButtonPressed(Button.B, () => {
    vogel.change(LedSpriteProperty.Y, 1)
})
```
## Schritt 3: Hindernisse erstellen

Hier beginnt es nun interessant zu werden.
Wir werden zufällige Hindernisse schaffen.
Wir werden alle Hindernisse in einem Array speichern.
Alle Hindernisse werden ein einziges Loch für den Vogel haben, durch das er fliegen kann.

Erstelle zuerst ein Array namens `hindernisse` für die Hindernisse welches alle Hindernis-Sprites speichern wird.

```blocks
let hindernisse: game.LedSprite[] = []
```
## Schritt 4: Weitere Hindernisse erstellen
Erzeuge nun vertikale Hindernisse aus 4 Sprites und 1 zufälligen Loch.
Erstelle eine neue Variable namens `leeresHindernisY`.
Verwende ``||math:wähle eine zufällige Zahl||``, erstelle eine Zufallszahl von 0 bis 4 und speichere diese in leeresHindernisY.

Verwende ein ``||loops:für||``-Schleife und zähle von 0 bis 4.
Erstelle für jede Koordinate die nicht gleich `leeresHindernisY` ist Hindernis-Sprites und füge diese am Ende des hindernisse-Arrays hinzu.

```blocks
let leeresHindernisY = 0
let hindernisse: game.LedSprite[] = []

leeresHindernisY = Math.randomRange(0, 4)
for (let index = 0; index <= 4; index++) {
    if (index != leeresHindernisY) {
        hindernisse.push(game.createSprite(4, index))
    }
}
```

## Schritt 5: 
Jetzt solltest du bei jedem Neustart deines @boardname@ unterschiedliche, automatisch erstellte vertikale Hindernisse sehen.

Bevor es weitergeht solltest du sicherstellen, dass die Hindernisse zufällig erstellt werden und dass sich der Vogel hoch- und runterbewegt.

```blocks
let leeresHindernisY = 0
let hindernisse: game.LedSprite[] = []
let vogel: game.LedSprite = null

vogel = game.createSprite(0, 2)
vogel.set(LedSpriteProperty.Blink, 300)

leeresHindernisY = Math.randomRange(0, 4)
for (let index = 0; index <= 4; index++) {
    if (index != leeresHindernisY) {
        hindernisse.push(game.createSprite(4, index))
    }
}

input.onButtonPressed(Button.A, () => {
    vogel.change(LedSpriteProperty.Y, -1)
})

input.onButtonPressed(Button.B, () => {
    vogel.change(LedSpriteProperty.Y, 1)
})
```

## Schritt 6: Hindernisse bewegen

Greife auf jedes Hindernis mit einer ``||loops:für element||``-Schleife zu (durchlaufe das gesamte hindernisse-Array) und verringere die `hindernisse X-Koordinate` um 1.
Klicke mit der rechten Maustaste auf den ``||variables:value||``-Block und nenne ihn um in ``||variables:hindernis||`` ; Ziehe dann den ``||variables:hindernis||``-Block oberhalb von ``||game:sprite||`` in den ``||game:ändere x um||``-Block.

```blocks
let hindernisse: game.LedSprite[] = []

basic.forever(() => {
    for (let hindernis of hindernisse) {
        hindernis.change(LedSpriteProperty.X, -1)
    }
    basic.pause(1000)
})
```
Die Hindernisse sollten sich jede Sekunde weiter nach links bewegen.

## Schritt 7: Hindernisse verschwinden lassen

Lasse die Hindernisse verschwinden nachdem sie die linke Ecke erreicht haben.
Durchlaufe alle Hindernisse, lösche das Hindernis-Sprite bei dem die X-Koordinate 0 ist und entferne sie aus dem hindernisse-Array.
```blocks
let hindernisse: game.LedSprite[] = []

basic.forever(() => {
    while (hindernisse.length > 0 && hindernisse[0].get(LedSpriteProperty.X) == 0) {
        hindernisse.removeAt(0).delete()
    }

    for (let hindernis of hindernisse) {
        hindernis.change(LedSpriteProperty.X, -1)
    }
    basic.pause(1000)
})
```

## Schritt 8: Erstelle weitere Hindernisse

Im Augenblick erstellt unser Code nur ein vertikales Hindernis.
Wir müssen den Code, der die Hindernisse erstellt, in die ``||basic:dauerhaft||``-Schleife platzieren, sodass er immer mehr Hindernisse erstellt.
```blocks
let leeresHindernisY = 0
let hindernisse: game.LedSprite[] = []

basic.forever(() => {
    while (hindernisse.length > 0 && hindernisse[0].get(LedSpriteProperty.X) == 0) {
        hindernisse.removeAt(0).delete()
    }

    for (let hindernis of hindernisse) {
        hindernis.change(LedSpriteProperty.X, -1)
    }
    leeresHindernisY = Math.randomRange(0, 4)
    for (let index = 0; index <= 4; index++) {
        if (index != leeresHindernisY) {
            hindernisse.push(game.createSprite(4, index))
        }
    }
    basic.pause(1000)
})
```

## Schritt 9: Durchläufe zählen
Jetzt ist unser Bildschirm voller beweglicher Hindernisse.
Erstelle etwas freien Raum zwischen den erzeugten Hindernissen.
Lasst uns dazu eine `ticks`-Variable einführen um zu zählen, wie viele Durchläufe die ``||basic:dauerhaft||``-Schleife schon gemacht hat und die Hindernisse nur dann erzeugen zu lassen, wenn `ticks` durch 3 teilbar ist.
```blocks
let ticks = 0
let leeresHindernisY = 0
let hindernisse: game.LedSprite[] = []

basic.forever(() => {
    while (hindernisse.length > 0 && hindernisse[0].get(LedSpriteProperty.X) == 0) {
        hindernisse.removeAt(0).delete()
    }

    for (let hindernis of hindernisse) {
        hindernis.change(LedSpriteProperty.X, -1)
    }
    if (ticks % 3 == 0) {
        leeresHindernisY = Math.randomRange(0, 4)
        for (let index = 0; index <= 4; index++) {
            if (index != leeresHindernisY) {
                hindernisse.push(game.createSprite(4, index))
            }
        }
    }
    ticks += 1
    basic.pause(1000)
})
```
## Schritt 10: Game Over

Momentan passiert nichts, wenn der Vogel von einem Hindernis getroffen wird.
Behebe das, indem du das `hindernisse`-Array durchläufst und kontrollierst, ob die Koordinaten eines Hindernis-Sprites identisch sind mit der Koordinate des Vogels.
```blocks
let vogel: game.LedSprite = null
let ticks = 0
let leeresHindernisY = 0
let hindernisse: game.LedSprite[] = []

basic.forever(() => {
    while (hindernisse.length > 0 && hindernisse[0].get(LedSpriteProperty.X) == 0) {
        hindernisse.removeAt(0).delete()
    }

    for (let hindernis of hindernisse) {
        hindernis.change(LedSpriteProperty.X, -1)
    }
    if (ticks % 3 == 0) {
        leeresHindernisY = Math.randomRange(0, 4)
        for (let index = 0; index <= 4; index++) {
            if (index != leeresHindernisY) {
                hindernisse.push(game.createSprite(4, index))
            }
        }
    }

    for (let hindernis of hindernisse) {
        if (hindernis.get(LedSpriteProperty.X) == vogel.get(LedSpriteProperty.X) && hindernis.get(LedSpriteProperty.Y) == vogel.get(LedSpriteProperty.Y)) {
            game.gameOver()
        }
    }

    ticks += 1
    basic.pause(1000)
})
```
## Schritt 11: Finalversion 

Der endgültige Code

(Klicke auf das Lämpchen)

```blocks
let vogel: game.LedSprite = null
let ticks = 0
let index = 0
let leeresHindernisY = 0
let hindernisse: game.LedSprite[] = []

input.onButtonPressed(Button.A, function ()  {
    vogel.change(LedSpriteProperty.Y, -1)
})

input.onButtonPressed(Button.B, function ()  {
    vogel.change(LedSpriteProperty.Y, 1)
})

index = 0
hindernisse = []
vogel = game.createSprite(0, 2)
vogel.set(LedSpriteProperty.Blink, 300)

basic.forever(function () {
    while (hindernisse.length > 0 && hindernisse[0].get(LedSpriteProperty.X) == 0) {
        hindernisse.removeAt(0).delete()
    }

    for (let hindernis of hindernisse) {
        hindernis.change(LedSpriteProperty.X, -1)
    }
    if (ticks % 3 == 0) {
        leeresHindernisY = Math.randomRange(0, 4)
        for (let index = 0; index <= 4; index++) {
            if (index != leeresHindernisY) {
                hindernisse.push(game.createSprite(4, index))
            }
        }
    }

    for (let hindernis of hindernisse) {
        if (hindernis.get(LedSpriteProperty.X) == vogel.get(LedSpriteProperty.X) && hindernis.get(LedSpriteProperty.Y) == vogel.get(LedSpriteProperty.Y)) {
            game.gameOver()
        }
    }

    ticks += 1
    basic.pause(1000)
})
```
## Übungen

Hier sind einige zusätzliche Funktionen, die du dem Spiel hinzufügen kannst:

    1. Zähle und zeige den Crashy Bird Spielstand.
    2. Mache die Hindernisse jedes Mal schneller, wenn ein Hindernis passiert wurde.

## Über die Autoren

Dieses Projekt wurde von Karolis Vycius erstellt.

Das ursprüngliche Flappy Bird-Spiel wurde von Dong Nguyen entwickelt.

Ins Deutsche übertragen von Michael Klein