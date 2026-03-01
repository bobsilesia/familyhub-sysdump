Zasady projektu: Samsung Family Hub (HACS)

Wydania
- Używaj SemVer: patch dla poprawek, minor dla nowych funkcji, major dla stabilnej pełnej integracji
- Każde wydanie ma Release Notes w GitHub Release zgodne ze zmianami

Autoryzacja
- Preferuj SmartApp OAuth dla pełnych uprawnień; PAT używaj do szybkiej konfiguracji
- Tokeny przechowuj w konfiguracji integracji HA; odświeżanie przez refresh_token

Integracja
- Sensory mapuj dynamicznie po capabilities; kamera przez execute i odczyt statusu
- Upload mediów przez usługę HA; docelowo pełna publikacja na panelu Family Hub

CI
- Uruchamiaj lint i sprawdzenie składni przy każdym push/PR
