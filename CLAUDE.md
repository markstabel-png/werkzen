# CLAUDE.md – Werkzen Projektregeln

> Hinweis: Diese Datei ist für **Phase 3** (echte technische Umsetzung) gedacht. Sie kommt erst ins Code-Repo, sobald tatsächlich programmiert wird. Für Phase 0/1 dient sie als Referenz und wird bis dahin weiter verfeinert.

## Projektüberblick

Werkzen ist ein digitaler Marktplatz für lokale Dienstleistungen (Reinigung, Garten, Umzug, Montage, Maler – Start in Stuttgart). Kunden beschreiben ihr Anliegen in eigenen Worten, eine KI strukturiert daraus einen Auftrag, passende Anbieter werden vorgeschlagen, Kunde und Anbieter chatten, ein Angebot wird angenommen, der Auftrag abgeschlossen und bewertet.

Vollständiges Konzept: siehe `WERKZEN_KONZEPT.md`.

## Tech-Stack

- Frontend: Next.js, TypeScript, Tailwind CSS
- Backend: Supabase (PostgreSQL, Auth, Realtime, Storage)
- KI: Claude API (Auftragsstrukturierung, später Matching-Unterstützung)
- Hosting: Vercel
- Noch **kein** eigenes Payment-System, kein natives Mobile-App-Development im MVP

## Benutzerrollen

- `customer` – Auftrag erstellen, Angebote erhalten, chatten, annehmen, bewerten
- `provider` – Profil, Kategorien, Einsatzgebiet, Anfragen erhalten, Angebote erstellen, chatten
- `admin` – Nutzer-/Anbieterverwaltung, Verifizierung, Moderation, Statistiken

## Zielstruktur (Soll-Zustand ab Phase 3 – noch nicht angelegt)

```
werkzen/
├── app/
│   ├── (public)/
│   │   ├── page.tsx                  # Startseite
│   │   ├── ueber-uns/
│   │   ├── fuer-anbieter/
│   │   └── kategorien/[kategorie]/[ort]/   # SEO-Landingpages
│   ├── (auth)/
│   │   ├── login/
│   │   └── register/
│   ├── auftrag/
│   │   ├── neu/
│   │   └── [id]/
│   ├── anbieter/
│   │   ├── registrieren/
│   │   ├── dashboard/
│   │   └── [id]/
│   ├── chat/[conversationId]/
│   └── admin/
├── components/
├── lib/
│   ├── supabase/
│   └── ai/                            # Claude-API-Anbindung
├── supabase/
│   └── migrations/
├── CLAUDE.md
├── WERKZEN_KONZEPT.md
└── PROJECT_RULES.md                   # falls von CLAUDE.md getrennt geführt
```

## Harte Regeln

1. Sauberen, modularen TypeScript-Code schreiben, TypeScript strict.
2. Keine unnötigen Libraries installieren.
3. Bestehende Architektur nicht ohne ausdrücklichen Grund verändern.
4. Keine Features implementieren, die nicht angefordert wurden.
5. Vor größeren Änderungen zuerst die bestehende Struktur analysieren (nicht raten).
6. Sicherheitsrelevante Änderungen besonders sorgfältig behandeln.
7. Supabase Row Level Security konsequent und von Anfang an verwenden.
8. Niemals API Keys oder Secrets im Frontend verwenden.
9. Alle Datenbankänderungen über nachvollziehbare Migrationen durchführen.
10. Komponenten wiederverwenden statt duplizieren.
11. Mobile-first entwickeln.
12. UI einfach, modern, vertrauenswürdig – keine überladenen Oberflächen, keine unnötigen Animationen.
13. Fehlerzustände und Loading States immer mitdenken.
14. Formulare müssen validiert werden.
15. Nach jeder größeren Implementierung: Tests.
16. Keine Fake-Daten in Produktionslogik.
17. Bei unklaren Anforderungen: nachfragen bzw. Architektur/Konzept prüfen, nicht raten.

## MVP-Umfang

**Rein:**
- Registrierung/Login (Kunde, Anbieter)
- Auftrag erstellen inkl. Foto-Upload, Standort, Terminwunsch
- KI-Auftragsstrukturierung (Claude API, strukturierte JSON-Ausgabe, keine Blackbox-Entscheidungen)
- Regelbasiertes Matching (Kategorie, Entfernung, Verfügbarkeit, Bewertung – kein ML nötig am Start)
- Chat (Supabase Realtime)
- Angebote erstellen/annehmen
- Bewertungen
- Admin-Basisfunktionen (Verifizierung, Moderation)

**Bewusst NICHT im MVP:**
- Eigenes Payment-System (Stufe 2 des Angebotsmodells ist ein einfaches manuelles Abo, kein In-App-Payment nötig)
- Komplexes CRM, Rechnungssoftware, Kalender-Sync
- WhatsApp-Integration
- Über die Start-Kategorien hinausgehende Kategorien
- KI-Sprachassistent, automatische Preisberechnung
- Eigene Versicherung

## Arbeitsweise mit Claude – credit-effizient

**Welches Claude-Werkzeug wann:**
- **Chat (Sonnet, ohne Tools):** Konzept-, Positionierungs- und Textarbeit – am günstigsten, aktuell (Phase 0) der richtige Modus.
- **Cowork:** mehrschrittige, dateibasierte Arbeit ohne Code – Akquiseliste pflegen, Recherche, Landingpage-Text iterieren, KPI-Tracking über die 90 Tage.
- **Claude Code (Pro- oder Max-Plan):** ausschließlich für die eigentliche Implementierung ab Phase 3 – nicht vorher öffnen, das kostet unnötig Kontext/Zeit.

**Kleine Schritte statt „baue alles":**
Nie mit „implementiere die ganze App" starten. Stattdessen fester Ablauf pro Feature (spart Kontext = spart Credits):

1. **Analyse-Prompt:** „Analysiere die bestehende Architektur/Anforderung. Ändere noch nichts. Zeig mir Risiken und offene Fragen."
2. **Plan-Prompt:** „Erstelle einen Implementierungsplan in kleinen Schritten für ausschließlich [Feature X]. Warte auf meine Bestätigung."
3. **Implementierungs-Prompt:** „Implementiere ausschließlich Schritt [n] aus dem bestätigten Plan."
4. **Review-Prompt:** „Prüfe die Implementierung auf Sicherheitsprobleme und Übereinstimmung mit CLAUDE.md."

**Weitere Credit-Spartipps:**
- Für jedes neue Feature einen neuen Chat/eine neue Claude-Code-Session starten statt einen endlos wachsenden Thread weiterzuführen – lange Threads verarbeiten bei jedem Turn den gesamten bisherigen Kontext erneut.
- CLAUDE.md und WERKZEN_KONZEPT.md aktuell halten, damit Claude nicht bei jeder Session neu erklärt werden muss, was Werkzen ist.
- Erst nach bestätigtem Plan implementieren lassen, nie „auf Verdacht" große Codemengen erzeugen lassen.
