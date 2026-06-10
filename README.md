# ecc-skills

[English](#english-version) · [Polski](#wersja-polska)

A set of four community agent skills for Claude Code (and compatible harnesses) that turn brand discovery and competitive benchmarking from one-off conversations into a versioned, resumable method.

---

## English Version

### Brand discovery and competitive benchmarking, as repeatable method — not a one-off prompt

A great brand interview or competitive teardown isn't a single conversation — it's a multi-session process that accumulates knowledge. The problem: every new chat starts from zero. Whatever you elicited last time evaporates unless you manually paste it back, and a mega-prompt that asks fifty questions at once gets shallow answers and forgets them by the end.

These skills encode the method once. Each is a plain `SKILL.md` file — no engine, no runtime, no database. They work because they instruct the agent to **read persisted state from disk at the start of every session and write it back at the end**. Run `/brand-discovery` on Monday, stop after the positioning module, come back Thursday — the agent reads where you were, reports it, and resumes the exact thread. State management is the whole trick: without it, every conversation wastes all the knowledge the last one captured.

The skills are versioned, portable, and chain together: discover the brand, scope its competitors, score them, write the report.

### The four skills

| Skill | What it does |
|---|---|
| **`brand-discovery`** | Runs an adaptive, multi-session brand identity interview. Elicits purpose, positioning, audience, personality, voice, narrative, and founder-brand tension. Output: a master brandbook (`90_SYNTHESIS.md`) you can hand to designers and writers. |
| **`competitive-platform-analysis`** | Decides *who* to benchmark and *where* to find them. Categorizes candidates along generic axes, sorts them into Direct / Adjacent / Aspirational tiers, and pre-filters them down to a defensible set. |
| **`benchmark-methodology`** | Turns the scoped set into comparable, defensible scores across nine weighted dimensions, each with explicit 1–5 rubrics. Produces one uniform profile card per competitor. |
| **`competitive-report-structure`** | Assembles the scored cards into a decision-grade report: landscape map, tiers, benchmarking matrix, white-space analysis, and prioritized recommendations. |

**How they chain:**

```
brand-discovery  →  competitive-platform-analysis  →  benchmark-methodology  →  competitive-report-structure
   (who are we?)        (who do we compete with?)         (how do they score?)        (what's the report?)
```

The brand's positioning brief — produced by `brand-discovery` — is the spine of the whole competitive pipeline. Each of the three competitive skills starts by establishing it, because a competitor set scoped without the client's own lens is noise, not intelligence.

### Why this framework mix

No single branding framework is complete — each is a model, not a law. The interview deliberately hybridizes, and each author earns a place by filling a gap the others leave open:

- **Sinek (Golden Circle)** opens the *Why* — emotionally engaging, easy to lead a conversation with, but structurally shallow on its own.
- **Kapferer (Identity Prism)** and **Aaker (brand system)** supply the full identity skeleton — culture, relationship, self-image — the dimensions agencies routinely skip.
- **Neumeier (trueline / onlyness)** and **Dunford (*Obviously Awesome*)** sharpen differentiation and competitive context: *what game are we playing* determines everything downstream.
- **Mark & Pearson (12 archetypes)** plus **J. Aaker's 5 personality dimensions** give the brand a coherent character that tone of voice can flow from.
- **Baker (*The Business of Expertise*)** and **Enns (*Win Without Pitching*)** add the discipline classic product frameworks lack: niche specialization and the expert-over-generalist stance that decides whether a creative business stands out at all.
- **Lencioni (*The Advantage*)** handles multi-founder coherence — keeping individual reputations reinforcing the collective brand instead of cannibalizing it.

The point isn't to run six frameworks in parallel. It's that each module reaches for the one tool that elicits its layer best, then the synthesis reconciles them.

### The interview technique layer

Knowing *what* to ask is only half of it; *how* to ask is a separate discipline borrowed from qualitative research, and it's what surfaces tacit knowledge instead of rehearsed declarations. The skill instructs the agent to use **laddering** (means-end: repeated "why does that matter to you?" until a core value surfaces), **5 Whys** (push past surface claims to the root reason), and **projective techniques** that break plateaus — the brand obituary ("if you closed in five years, what would clients miss?"), "if the brand were a person, how would they walk into a room?", and competitive contrast ("name a peer you admire but would never want to become"). It asks one question at a time, paraphrases each answer, detects thin or jargon-heavy responses and demands a concrete example, and watches for the **saturation signal** — when two consecutive probes yield nothing new, it closes the module. These matter because founders rarely state their real convictions outright; surface answers are cheap, and the technique is what gets underneath them.

### Brand discovery module sequence

Eight modules, completed in order (jumps are allowed and recorded in state):

| File | Module | Frameworks |
|---|---|---|
| `10_purpose-why.md` | Purpose / Why | Sinek Golden Circle, Lencioni |
| `20_positioning.md` | Positioning | Dunford *Obviously Awesome*, Moore template |
| `30_audience-niche.md` | Audience & Niche | Baker *Business of Expertise*, ICP |
| `40_personality-archetype.md` | Personality & Archetype | Mark & Pearson 12 archetypes, J. Aaker 5 dimensions |
| `50_voice-tone.md` | Voice & Tone | Voice spectrum, tone-by-content matrix |
| `60_narrative-story.md` | Narrative / Story | Neumeier trueline, brand story arc |
| `70_founder-tension.md` | Founder vs Organisation Brand | Enns *Win Without Pitching* |
| `90_SYNTHESIS.md` | Master Brandbook | Kapferer prism, Aaker brand system |

Each module file has two sections: `## Raw` (verbatim quotes and examples — exact language, no paraphrase) and `## Synthesis` (interpretation, two-to-three candidate formulations, open questions, contradictions between participants). Templates for all eight ship in `brand-discovery/templates/`.

### Checkpoint management (self-managed)

The skills carry no engine, so you own the state files. The convention below is all you need — plain markdown the agent reads and writes.

**File layout**

```
/brand-identity/
  STATE.md                    # progress manifest
  00_setup.md                 # session scope, participants, procedural decisions
  10_purpose-why.md
  20_positioning.md
  30_audience-niche.md
  40_personality-archetype.md
  50_voice-tone.md
  60_narrative-story.md
  70_founder-tension.md
  90_SYNTHESIS.md             # master brandbook — grows over time
  /founders/                  # individual interviews, multi-stakeholder mode
    founder-a.md
    founder-b.md
    ...
```

**`STATE.md` format** — a manifest the agent reads first on every session. A status table plus a current-module pointer and a next-step note:

```markdown
# Brand Identity — interview state

Current module: 20_positioning (in_progress)

| Module | Status |
|--------|--------|
| 10 purpose | done |
| 20 positioning | in_progress |
| 30 audience-niche | not_started |
| ... | ... |

Next step: finish the value laddering with founder C.
```

Module statuses move through: `not_started` → `in_progress` → `saturated` → `reconciled` → `done`.

**Module file frontmatter** — every module file carries its own status block:

```yaml
---
module: positioning
status: in_progress
last_updated: 2026-06-10
participants: [a, b, c]
open_questions:
  - "Is the category 'product design studio' or 'fintech product partner'?"
---
```

**Resume handshake** — the instruction you give the skill at every session start, and the skill enforces it on itself:

1. Read `STATE.md` and the current module file before asking anything.
2. Report in two or three sentences: which module, what status, what's open.
3. Ask: "Continue here, or switch module?"

Do **not** let it proceed without reading state first — that's the line between a resumable method and starting over.

**End-of-session protocol**

1. Write `## Raw` and `## Synthesis` to the current module file.
2. Update the module frontmatter (`status`, `last_updated`, `open_questions`) and the `STATE.md` manifest.
3. Confirm explicitly: "Saved — next session resumes from [module]."

**Multi-stakeholder mode:** when more than one founder participates, each one's answers go to `founders/{participant}.md` instead of the shared module file — this avoids the anchoring and conformity effect of a group session. After all founders finish a module, a reconciliation pass summarizes convergences and divergences and flags the "productive tensions" for a group alignment workshop.

### Competitive pipeline

Once the brand is discovered, three skills run in sequence to produce a benchmarking report:

1. **`competitive-platform-analysis`** scopes the set. Starting from the positioning brief, it categorizes candidates along generic axes (positioning stance, specialization, size, engagement format, distinctiveness, evidence model, brand strength, reach), sorts them into **Direct / Adjacent / Aspirational** tiers, and pre-filters typically 10–18 candidates down to 8–12 worth profiling.
2. **`benchmark-methodology`** scores each survivor on nine weighted dimensions — positioning clarity (18%), brand voice (15%), visual craft (15%), offer packaging (12%), evidence (12%), enterprise-readiness (10%), thought leadership (8%), pricing transparency (5%), and the client's own strategic tension (scored as both poles, never averaged). Every score needs a one-line justification and a source. Output: one profile card per competitor.
3. **`competitive-report-structure`** assembles the cards into a decision-grade report — executive summary, landscape map, tiers, a competitors × dimensions heatmap matrix, deep dives, white-space and threats, prioritized recommendations, and a sources appendix that keeps the whole thing auditable.

Throughout: verify every competitor claim across at least two sources, and tag asserted vs. proven. Site copy is marketing, not fact.

### Quickstart

1. **Install the skills.** Drop the four `*.skill.md` files (and the `brand-discovery/templates/` folder) where Claude Code looks for skills — your project's `.claude/skills/` directory, or your personal skills folder.
2. **Create the state directory.** Make a `/brand-identity/` folder wherever you keep project notes (a repo, an Obsidian vault — any directory the agent can read and write).
3. **Run it.** Invoke `/brand-discovery`. On a fresh start it confirms the brand name, participants, and where to save files, then begins at module 10. On a resume it reads `STATE.md` first and picks up where you left off.

When the brandbook is complete, hand the positioning brief to `/competitive-platform-analysis` and run the competitive pipeline.

---

## Wersja Polska

### Brand discovery i benchmarking konkurencji jako powtarzalna metoda — nie jednorazowy prompt

Dobry wywiad brandowy albo rozbiórka konkurencji to nie pojedyncza rozmowa — to wielosesyjny proces, który kumuluje wiedzę. Problem w tym, że każdy nowy czat startuje od zera. To, co wydobyłeś poprzednim razem, paruje, o ile ręcznie nie wkleisz tego z powrotem, a mega-prompt zadający pięćdziesiąt pytań naraz dostaje płytkie odpowiedzi i zapomina je, zanim dojdzie do końca.

Te skille kodują metodę raz. Każdy to zwykły plik `SKILL.md` — bez silnika, bez runtime'u, bez bazy danych. Działają, bo instruują agenta, żeby **na starcie każdej sesji czytał zapisany stan z dysku, a na końcu zapisywał go z powrotem**. Odpal `/brand-discovery` w poniedziałek, przerwij po module pozycjonowania, wróć w czwartek — agent czyta, gdzie byłeś, raportuje to i wznawia dokładnie ten sam wątek. Cała sztuka tkwi w zarządzaniu stanem: bez niego każda rozmowa marnuje wiedzę, którą zebrała poprzednia.

Skille są wersjonowalne, przenośne i łączą się w łańcuch: odkryj markę, wyznacz jej konkurencję, oceń ją, napisz raport.

### Cztery skille

| Skill | Co robi |
|---|---|
| **`brand-discovery`** | Prowadzi adaptacyjny, wielosesyjny wywiad tożsamości marki. Wydobywa cel, pozycjonowanie, audytorium, osobowość, głos, narrację i napięcie między marką założyciela a marką firmy. Wynik: master-brandbook (`90_SYNTHESIS.md`), który oddasz projektantom i copywriterom. |
| **`competitive-platform-analysis`** | Decyduje, *kogo* benchmarkować i *gdzie* go znaleźć. Kategoryzuje kandydatów wzdłuż generycznych osi, sortuje ich na poziomy Bezpośredni / Sąsiedni / Aspiracyjny i wstępnie filtruje do bronionego zbioru. |
| **`benchmark-methodology`** | Zamienia wyznaczony zbiór w porównywalne, bronione oceny w dziewięciu ważonych wymiarach, każdy z jawną rubryką 1–5. Produkuje jedną jednolitą kartę profilu na konkurenta. |
| **`competitive-report-structure`** | Składa ocenione karty w raport gotowy do decyzji: mapę krajobrazu, poziomy, macierz benchmarkingową, analizę białej plamy i priorytetyzowane rekomendacje. |

**Jak się łączą:**

```
brand-discovery  →  competitive-platform-analysis  →  benchmark-methodology  →  competitive-report-structure
   (kim jesteśmy?)    (z kim konkurujemy?)              (jak się oceniają?)         (jaki jest raport?)
```

Brief pozycjonujący marki — wytwarzany przez `brand-discovery` — jest kręgosłupem całego pipeline'u konkurencyjnego. Każdy z trzech skilli konkurencyjnych zaczyna od jego ustalenia, bo zbiór konkurentów wyznaczony bez własnej soczewki klienta to szum, nie wywiad.

### Dlaczego taka mieszanka frameworków

Żaden pojedynczy framework brandingowy nie jest kompletny — każdy to model, nie prawo. Wywiad celowo hybrydyzuje, a każdy autor zasługuje na miejsce, bo wypełnia lukę, którą inni zostawiają otwartą:

- **Sinek (Golden Circle)** otwiera *Why* — emocjonalnie angażujący, łatwo na nim poprowadzić rozmowę, ale sam w sobie strukturalnie płytki.
- **Kapferer (Brand Identity Prism)** i **Aaker (system marki)** dają pełny szkielet tożsamości — kulturę, relację, self-image — wymiary, które agencje rutynowo pomijają.
- **Neumeier (trueline / onlyness)** i **Dunford (*Obviously Awesome*)** wyostrzają differentiation i kontekst konkurencyjny: *w jaką grę gramy* determinuje wszystko niżej.
- **Mark & Pearson (12 archetypów)** plus **pięć wymiarów osobowości J. Aaker** nadają marce spójny charakter, z którego może wypływać ton głosu.
- **Baker (*The Business of Expertise*)** i **Enns (*Win Without Pitching*)** dokładają dyscyplinę, której brakuje klasycznym frameworkom produktowym: specjalizację niszową i postawę eksperta zamiast generalisty, która rozstrzyga, czy firma kreatywna w ogóle się wyróżni.
- **Lencioni (*The Advantage*)** obsługuje spójność wielu założycieli — pilnuje, żeby indywidualne reputacje wzmacniały markę zbiorową, a nie ją kanibalizowały.

Nie chodzi o to, żeby puścić sześć frameworków równolegle. Chodzi o to, że każdy moduł sięga po jedno narzędzie, które najlepiej elicytuje jego warstwę, a synteza na końcu je rekoncyliuje.

### Warstwa techniki wywiadu

Wiedzieć, *o co* pytać, to dopiero połowa; *jak* pytać to osobna dyscyplina pożyczona z badań jakościowych — i to ona wydobywa wiedzę ukrytą (tacit) zamiast wyuczonych deklaracji. Skill instruuje agenta, żeby używał **ladderingu** (means-end: powtarzane „dlaczego to dla Ciebie ważne?", aż wypłynie wartość rdzeniowa), **5 Whys** (przepychanie poza deklaracje powierzchowne do prawdziwej przyczyny) oraz **technik projekcyjnych**, które przełamują plateau — brand obituary („gdybyście zniknęli za pięć lat, czego zabrakłoby klientom?"), „gdyby marka była osobą, jak weszłaby do pokoju?" i kontrast konkurencyjny („wymień firmę, którą podziwiasz, ale którą nigdy nie chciałbyś się stać"). Zadaje jedno pytanie naraz, parafrazuje każdą odpowiedź, wykrywa odpowiedzi cienkie albo żargonowe i wymusza konkretny przykład, a także pilnuje **sygnału nasycenia** — gdy dwie kolejne sondy nie wnoszą nic nowego, zamyka moduł. To ma znaczenie, bo założyciele rzadko wprost formułują swoje prawdziwe przekonania; powierzchowne odpowiedzi są tanie, a technika to właśnie to, co wchodzi pod nie.

### Sekwencja modułów brand discovery

Osiem modułów, realizowanych po kolei (skoki są dozwolone i zapisywane w stanie):

| Plik | Moduł | Frameworki |
|---|---|---|
| `10_purpose-why.md` | Cel / Why | Sinek Golden Circle, Lencioni |
| `20_positioning.md` | Pozycjonowanie | Dunford *Obviously Awesome*, szablon Moore'a |
| `30_audience-niche.md` | Audytorium i nisza | Baker *Business of Expertise*, ICP |
| `40_personality-archetype.md` | Osobowość i archetyp | Mark & Pearson 12 archetypów, 5 wymiarów J. Aaker |
| `50_voice-tone.md` | Głos i ton | Spektrum głosu, macierz tonu wg typu treści |
| `60_narrative-story.md` | Narracja / Story | Neumeier trueline, łuk story marki |
| `70_founder-tension.md` | Marka założyciela vs marka firmy | Enns *Win Without Pitching* |
| `90_SYNTHESIS.md` | Master-brandbook | Pryzmat Kapferera, system marki Aakera |

Każdy plik modułu ma dwie sekcje: `## Raw` (cytaty i przykłady dosłownie — dokładny język, bez parafrazy) oraz `## Synthesis` (interpretacja, dwa-trzy kandydujące sformułowania, otwarte pytania, sprzeczności między uczestnikami). Szablony wszystkich ośmiu są w `brand-discovery/templates/`.

### Zarządzanie checkpointami (self-managed)

Skille nie niosą żadnego silnika, więc pliki stanu są Twoje. Poniższa konwencja to wszystko, czego potrzebujesz — zwykły markdown, który agent czyta i zapisuje.

**Układ plików**

```
/brand-identity/
  STATE.md                    # manifest postępu
  00_setup.md                 # zakres sesji, uczestnicy, decyzje proceduralne
  10_purpose-why.md
  20_positioning.md
  30_audience-niche.md
  40_personality-archetype.md
  50_voice-tone.md
  60_narrative-story.md
  70_founder-tension.md
  90_SYNTHESIS.md             # master-brandbook — rośnie z czasem
  /founders/                  # wywiady indywidualne, tryb wielu interesariuszy
    founder-a.md
    founder-b.md
    ...
```

**Format `STATE.md`** — manifest, który agent czyta jako pierwszy w każdej sesji. Tabela statusów plus wskaźnik bieżącego modułu i notatka o następnym kroku:

```markdown
# Brand Identity — stan wywiadu

Bieżący moduł: 20_positioning (in_progress)

| Moduł | Status |
|-------|--------|
| 10 purpose | done |
| 20 positioning | in_progress |
| 30 audience-niche | not_started |
| ... | ... |

Następny krok: dokończyć laddering wartości z founderem C.
```

Statusy modułów przechodzą przez: `not_started` → `in_progress` → `saturated` → `reconciled` → `done`.

**Frontmatter pliku modułu** — każdy plik modułu niesie własny blok statusu:

```yaml
---
module: positioning
status: in_progress
last_updated: 2026-06-10
participants: [a, b, c]
open_questions:
  - "Czy kategoria to 'product design studio' czy 'fintech product partner'?"
---
```

**Resume handshake** — instrukcja, którą dajesz skillowi na starcie każdej sesji, a skill egzekwuje ją na sobie:

1. Przeczytaj `STATE.md` i plik bieżącego modułu, zanim o cokolwiek zapytasz.
2. Zreferuj w dwóch-trzech zdaniach: który moduł, jaki status, co otwarte.
3. Zapytaj: „Kontynuujemy tutaj czy zmieniamy moduł?"

**Nie** pozwól mu ruszyć dalej bez wcześniejszego przeczytania stanu — to granica między metodą wznawialną a zaczynaniem od nowa.

**Protokół końca sesji**

1. Zapisz `## Raw` i `## Synthesis` do bieżącego pliku modułu.
2. Zaktualizuj frontmatter modułu (`status`, `last_updated`, `open_questions`) i manifest `STATE.md`.
3. Potwierdź jawnie: „Zapisane — następna sesja wznawia od [moduł]."

**Tryb wielu interesariuszy:** gdy uczestniczy więcej niż jeden założyciel, odpowiedzi każdego trafiają do `founders/{participant}.md` zamiast do wspólnego pliku modułu — to unika efektu zakotwiczenia i konformizmu sesji grupowej. Po tym, jak wszyscy założyciele skończą moduł, etap rekoncyliacji streszcza zbieżności i rozbieżności oraz oznacza „produktywne napięcia" do warsztatu uzgodnienia w grupie.

### Pipeline konkurencyjny

Gdy marka jest już odkryta, trzy skille jadą sekwencyjnie, żeby wytworzyć raport benchmarkingowy:

1. **`competitive-platform-analysis`** wyznacza zbiór. Startując od briefu pozycjonującego, kategoryzuje kandydatów wzdłuż generycznych osi (stanowisko pozycjonujące, specjalizacja, wielkość, format współpracy, distinctiveness, model dowodu, siła marki, zasięg), sortuje ich na poziomy **Bezpośredni / Sąsiedni / Aspiracyjny** i wstępnie filtruje zwykle 10–18 kandydatów do 8–12 wartych profilowania.
2. **`benchmark-methodology`** ocenia każdego, kto przeszedł, w dziewięciu ważonych wymiarach — klarowność pozycjonowania (18%), głos marki (15%), craft wizualny (15%), pakowanie oferty (12%), dowody (12%), enterprise-readiness (10%), thought leadership (8%), transparentność cen (5%) oraz własne napięcie strategiczne klienta (oceniane jako oba bieguny, nigdy uśrednione). Każda ocena potrzebuje jednolinijkowego uzasadnienia i źródła. Wynik: jedna karta profilu na konkurenta.
3. **`competitive-report-structure`** składa karty w raport gotowy do decyzji — executive summary, mapę krajobrazu, poziomy, macierz heatmap konkurenci × wymiary, deep dive'y, białą plamę i zagrożenia, priorytetyzowane rekomendacje oraz załącznik źródeł, który utrzymuje całość audytowalną.

W całym przebiegu: weryfikuj każde twierdzenie o konkurencie w co najmniej dwóch źródłach i oznaczaj asserted vs proven. Copy ze strony to marketing, nie fakt.

### Quickstart

1. **Zainstaluj skille.** Wrzuć cztery pliki `*.skill.md` (i folder `brand-discovery/templates/`) tam, gdzie Claude Code szuka skilli — do katalogu `.claude/skills/` Twojego projektu albo do osobistego folderu skilli.
2. **Utwórz katalog stanu.** Zrób folder `/brand-identity/` tam, gdzie trzymasz notatki projektu (repo, vault Obsidian — dowolny katalog, który agent może czytać i zapisywać).
3. **Odpal.** Wywołaj `/brand-discovery`. Na świeżym starcie potwierdzi nazwę marki, uczestników i gdzie zapisywać pliki, po czym zacznie od modułu 10. Przy wznawianiu najpierw czyta `STATE.md` i podejmuje wątek tam, gdzie skończyłeś.

Gdy brandbook jest gotowy, oddaj brief pozycjonujący do `/competitive-platform-analysis` i odpal pipeline konkurencyjny.
