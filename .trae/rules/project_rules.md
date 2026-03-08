Zasady projektu: Samsung Family Hub (HACS)

Wydania
- Używaj SemVer: patch dla poprawek, minor dla nowych funkcji, major dla stabilnej pełnej integracji
- Każde wydanie ma Release Notes w GitHub Release zgodne ze zmianami
 - Preferencja: po wydaniu patch następnym krokiem jest minor (np. 1.0.9 → 1.1.0)

Autoryzacja
- Preferuj SmartApp OAuth dla pełnych uprawnień; PAT używaj do szybkiej konfiguracji
- Tokeny przechowuj w konfiguracji integracji HA; odświeżanie przez refresh_token

Integracja
- Sensory mapuj dynamicznie po capabilities; kamera przez execute i odczyt statusu
- Upload mediów przez usługę HA; docelowo pełna publikacja na panelu Family Hub

CI
- Uruchamiaj lint i sprawdzenie składni przy każdym push/PR (flake8 + compileall) na wszystkich gałęziach
- Auto Fix na PR i push (z wyłączeniem main): ruff --fix, black (79), isort (79), auto-commit
 - Auto Fix na PR i push (z wyłączeniem main): ruff format + ruff --fix (I, itp.), auto-commit
- Sync wiki: workflow warunkowy (has_wiki), permissions contents: write, skip gdy brak uprawnień
- Przed wypchnięciem zawsze uruchom lokalnie: `flake8 custom_components/familyhub` oraz `python3 -m compileall -q custom_components/familyhub`
- Przed publikacją uruchom dodatkowo: `ruff check .` oraz `mypy custom_components/familyhub`

Rozwój i Jakość Kodu (CI/CD)
- Linting & Formatting: Kod musi przechodzić walidację `ruff` (linting i formatowanie) oraz `mypy` (typowanie).
- HACS & Hassfest: Każda zmiana w strukturze integracji musi być zgodna ze standardami HACS i przechodzić walidację `hassfest`.
- Wersjonowanie: Stosuj Semantic Versioning. Każde wydanie musi być odnotowane w `CHANGELOG.md` i w `manifest.json`.
- HACS PR: Podczas procesu zgłaszania do HACS Default, aktualizuj opis PR linkując do najnowszej wersji.

CI/CD Verification Checklist
Przed każdą publikacją (Release/Tag) wykonaj:
1.  [ ] Linting: `ruff check custom_components/familyhub` (musi być czysto).
2.  [ ] Formatting: `ruff format --check custom_components/familyhub` (musi być sformatowane).
3.  [ ] Typing: `mypy --ignore-missing-imports custom_components/familyhub` (brak błędów typów).
4.  [ ] Validation: `hassfest` (struktura i manifest).
5.  [ ] Commit: Upewnij się, że zmiany są zacommitowane przed tagowaniem.

Polling i RS232
- Domyślny polling: 30 s (DataUpdateCoordinator.update_interval) dla sensorów i kamery.
- Cel: unikanie przeciążenia interfejsów (RS232/API) zbyt częstymi zapytaniami.
- Zasady: limituj zapytania, grupuj odczyty, stosuj backoff na błędach/braku odpowiedzi.
- Snapshot kamery: wywołuj on‑demand; nie odpytywać co kilka sekund.

Odznaki i README
- Stosuj Shields.io: status CI (GitHub Actions), gwiazdki, issues, PR, last commit.
- Release badges: linkuj do `releases` lub `releases/latest` (unikaj bezpośrednich tagów mogących prowadzić do 404).
- Liczba pobrań: wykorzystuj endpoint GitHub downloads dla Releases.

Packaging (HACS)
- Buduj ZIP: `dist/familyhub-x.y.z.zip` zawierający wyłącznie `custom_components/familyhub`.
- Dołącz asset ZIP do GitHub Release tej samej wersji.
- README i odznaki renderowane w HACS po stronie repozytorium (ZIP nie musi zawierać README).

Auto Fix / Auto Branch Fix
- Kolejność kroków: `autoflake` → `ruff format` → `ruff --fix` (w tym sortowanie importów).
- PR/push poza main: automatyczny commit poprawek; na main tylko sprawdzanie.
- Autofix PR: włącz auto‑merge po zielonych checkach (preferuj squash).
