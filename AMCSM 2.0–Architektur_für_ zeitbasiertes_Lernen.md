AMCSM 2.0 – Architektur für zeitbasiertes, erfahrungsorientiertes Lernen in KI-Systemen
Ein duales Modell aus AMCSM‑CORE und AMCSM‑SB mit MIDI‑2.0‑Synchronisation und Cache‑basiertem Echtzeitgedächtnis

Abstract

Konventionelle KI‑Modelle generieren Output, ohne ihn im Kontext ihrer eigenen Handlungen zu bewerten. Ihnen fehlt ein Mechanismus, der eigenes Verhalten mit externem Feedback entlang einer gemeinsamen Zeitbasis vergleicht und daraus korrigierende Muster ableitet.

Dieses Paper stellt AMCSM 2.0 vor — eine neue KI‑Architektur, die:
    • AMCSM‑CORE (ein MIDI‑2.0‑generatives Modell),
    • AMCSM‑SB (ein kleines, online‑trainierbares Auditor‑Modell),
    • Output‑ und Input‑Caches (zeitlich synchronisierte Ringbuffer),
    • MIDI‑2.0 als globale Zeitbasis,
kombiniert, um zeitlich verankertes Feedback‑Lernen zu ermöglichen.
Das System trennt Erzeugung, Bewertung und Konsolidierung in zwei Modelle und eine Echtzeit‑Cache‑Schicht. Damit entsteht ein neuer Ansatz für KI‑Systeme, die aus Interaktion lernen.

1. Einleitung

Heutige KI‑Modelle:
    • generieren Output ohne Rückbezug,
    • besitzen kein funktionales Kurzzeitgedächtnis,
    • können ihr Verhalten nicht mit externem Feedback abgleichen,
    • haben keine interne Mechanik für Kausalitätslernen,
    • sind nicht in Echtzeit adaptiv.

AMCSM 2.0 adressiert diese Lücken durch:
    • eine zeitlich synchronisierte Cache‑Schicht,
    • ein zweistufiges Lernsystem,
    • MIDI‑2.0 als semantisches Alphabet und Zeitquelle,
    • und eine klare Trennung zwischen Online‑Analyse und Offline‑Konsolidierung.

2. Zeitbasis und Synchronisation

2.1 Die DAW als Master‑Clock

Die DAW ist die einzige Zeitquelle des Systems:
    • MIDI‑Clock,
    • Transport‑Signale,
    • Session‑Start,
    • hochauflösende Zeitstempel.
Das Script/Cache‑System ist kein Clock‑Generator, sondern ein Clock‑Follower.

2.2 Zeitstempelvergabe

Beim ersten Output des AMCSM‑CORE:
    1. Das Script fragt die aktuelle MIDI‑Zeit ab.
    2. Diese Zeit wird als Session‑Start übernommen.
    3. Alle folgenden Events erhalten diesen Zeitstempel.

Damit teilen Output und Rückkanal dieselbe Zeitachse.

2.3 Warum das LongBrain keine Zeit benötigt

AMCSM‑CORE erhält:
    • bereits synchronisierte Sequenzen,
    • bereits gelabelte Ereignisse,
    • bereits kausal geordnete Daten.

Es muss keine Zeit interpretieren. Es lernt aus strukturierten Erfahrungen, nicht aus Roh‑Zeitdaten.

3. Cache‑Schicht

3.1 Output‑Cache (32k Ringbuffer)

Funktionen:
    • Abfangen des CORE‑Outputs,
    • Abfrage der MIDI‑Zeit,
    • Vergabe des Zeitstempels,
    • sofortiges Weiterleiten (keine Latenz),
    • Speicherung im Ringbuffer.

3.2 Input‑Cache (32k Ringbuffer)

Funktionen:
    • Abfangen des Rückkanals,
    • Übernahme der MIDI‑Zeit,
    • sofortiges Weiterleiten,
    • Speicherung im Ringbuffer.

3.3 Rückkanal‑Definition

Der Rückkanal umfasst:
    • MIDI‑2.0 Parameteränderungen,
    • DAW‑Automationen,
    • User‑Interaktionen (Fader, Regler, Controller),
    • optional emotionale Parameter (Valenz, Arousal, Tension).

Damit bildet der Rückkanal die Reaktion der Außenwelt auf den KI‑Output ab.

4. AMCSM‑SB (ShortBrain)

Online‑Auditor für zeitlich verankertes Feedback‑Lernen
Das ShortBrain beginnt zu arbeiten, wenn beide Caches ~50% gefüllt sind.

Es führt aus:
    • Zeitstempel‑Alignment,
    • Vergleich von Output(t) und Input(t),
    • Erkennung von Abweichungen,
    • Markierung von Fehlern,
    • Erzeugung von Labels,
    • Schreiben ins Event‑Log.

4.1 Online‑Learning‑Mechanismus

Das ShortBrain ist klein (1–10 Mio Parameter) und nutzt:
    • Mini‑Batch‑Updates,
    • Sliding‑Window‑Training,
    • Replay‑Buffer,
    • Regularisierung,
    • Gewichtsstabilisierung (z. B. EWC).

Damit bleibt das Modell stabil, obwohl es online lernt.

5. AMCSM‑CORE (LongBrain)
Offline‑Konsolidierung und Kausalitätslernen

Das LongBrain:
    • generiert Musik,
    • versteht MIDI‑2.0‑Semantik,
    • lernt Kausalität zwischen Aktion und Reaktion,
    • bildet musikalische Identität,
    • integriert Feedback in langfristige Muster.

Es wird offline trainiert — ausschließlich auf:
    • gelabelten Sequenzen,
    • synchronisierten Ereignissen,
    • ShortBrain‑Fehlerindikatoren,
    • Zustandsverläufen.

6. Zeitlich verankertes Feedback‑Lernen (funktionale Selbstreflexion)

Wir vermeiden den Begriff „Selbstreflexion“ im philosophischen Sinn. Stattdessen definieren wir:
Funktionale Selbstreflexion = die Fähigkeit eines Systems, eigenes Verhalten entlang einer gemeinsamen Zeitbasis mit externem Feedback zu vergleichen und daraus korrigierende Muster abzuleiten.

Der Prozess:
    1. Aktion – CORE erzeugt Output.
    2. Reaktion – DAW/User erzeugen Rückkanal.
    3. Synchronisation – Caches ordnen beide Ströme zeitlich.
    4. Analyse – ShortBrain erkennt Abweichungen.
    5. Labeling – Fehler werden markiert.
    6. Konsolidierung – LongBrain lernt daraus.

Das System lernt:
„Diese Aktion → führte zu dieser Reaktion → führte zu dieser Zustandsänderung.“
Das ist zeitlich verankertes Feedback‑Lernen.

7. Ein neuer Forschungszweig

AMCSM 2.0 ist nicht:
    • ein Musikmodell,
    • ein LLM,
    • ein Reinforcement‑Learner,
    • ein Audio‑Synthesizer.

Es ist:
Ein duales KI‑System, das Erfahrung strukturiert, Fehler erkennt und aus Interaktion lernt.

⭐ Temporal Experience Learning (TEL)
oder
⭐ Experience‑Aligned Machine Learning (EAML)
oder
⭐ Dual‑Brain AI Architecture
Ein Ansatz, der über klassische KI‑Paradigmen hinausgeht.

8. Schlussfolgerung

AMCSM 2.0 definiert eine neue KI‑Architektur:
    • MIDI‑2.0‑nativ,
    • zeitbasiert,
    • cache‑gestützt,
    • dual‑modelliert,
    • erfahrungsorientiert.

Es trennt:
    • Erleben (ShortBrain)
    • Verstehen (LongBrain)

und verbindet sie über:
    • eine gemeinsame Zeitbasis,
    • strukturierte Erfahrung,
    • kausale Sequenzen.

Damit entsteht ein System, das:
    • eigenes Verhalten analysiert,
    • Fehler erkennt,
    • Erfahrung strukturiert,
    • aus Interaktion lernt.

Das ist kein inkrementelles Modell. 
Das könnte die Zukunft werden.
