# HoneyPot-Ngnix-K8s
HoneyPot-Ngnix-K8s
# HoneyKube - Dokumentacja Projektu

bedzie to kontynuwoane beda testy jak to dziala 

## Opis projektu

HoneyKube to system honeypot bazujący na Nginx, wdrażany w klastrze Kubernetes za pomocą Kind-Kind, z wykorzystaniem narzędzia DevBox do standaryzacji środowiska oraz Taskfile do automatyzacji zadań. Projekt pozwala na tworzenie, monitorowanie i analizę ataków na celowo podatny serwer web.

## Struktura projektu

```
.
├── Taskfile.yml          # Plik z zadaniami do automatyzacji
├── devbox.json           # Konfiguracja środowiska DevBox
├── k8s/                  # Pliki konfiguracyjne Kubernetes
│   ├── honeypot/         # Konfiguracja dla honeypota
│   │   └── nginx-honeypot.yaml
│   └── monitoring/       # Konfiguracja dla systemu monitorowania
│       └── wazuh.yaml
├── config/               # Dodatkowe pliki konfiguracyjne
│   └── honeypot-rules.xml  # Reguły Wazuh specyficzne dla honeypota
└── logs/                 # Katalog na zapisane logi
```

## Komponenty systemu

### 1. Nginx Honeypot

Specjalnie skonfigurowany serwer Nginx z celowo wprowadzonymi "podatnościami":

- Włączone listowanie katalogów
- Fałszywy panel administracyjny
- Ujawnianie wersji serwera
- Dostęp do ścieżek PHP (fałszywy phpinfo)
- Możliwość upload'u plików

### 2. System monitorowania Wazuh

System HIDS (Host-based Intrusion Detection System) wdrażany jako:

- Wazuh Manager - centralny serwer zbierający i analizujący logi
- Wazuh Agent - agent monitorujący działający na każdym węźle klastra

### 3. Niestandardowe reguły detekcji

Specjalne reguły Wazuh skonfigurowane do wykrywania:

- Prób skanowania
- Prób dostępu do panelu administracyjnego
- Prób wgrania złośliwych plików
- Ataków typu SQL Injection, XSS, LFI/RFI
- Prób wykonania komend systemowych

## Jak korzystać z projektu

### Inicjalizacja środowiska

```bash
# Inicjalizacja DevBox
devbox init
devbox install

# Przygotowanie struktury projektu
task init
```

### Zarządzanie klastrem

```bash
# Tworzenie klastra
task cluster:create

# Tworzenie namespace'ów
task namespace:create

# Usuwanie klastra
task cluster:delete
```

### Wdrażanie komponentów

```bash
# Wdrażanie Nginx Honeypot
task honeypot:deploy

# Wdrażanie Wazuh
task wazuh:deploy

# Monitorowanie honeypota
task honeypot:expose
task honeypot:logs
```

### Testowanie i analiza

```bash
# Symulacja prostego skanowania
task test:scan

# Symulacja ataku
task test:attack

# Zbieranie logów
task logs:collect
```

## Bezpieczeństwo

**UWAGA**: Ten system jest przeznaczony wyłącznie do celów edukacyjnych i testowych. Nie należy go wdrażać w środowisku produkcyjnym ani łączyć z innymi systemami produkcyjnymi.

- Honeypoty powinny być izolowane od reszty infrastruktury
- Projekt jest zaprojektowany do uruchamiania w izolowanym klastrze Kind
- Dane zbierane przez system zawierają informacje o potencjalnych atakach i powinny być traktowane jako poufne

## Rozszerzanie projektu

Projekt można rozszerzyć o:

1. Dashboard wizualizacji ataków (np. ELK Stack)
2. Więcej typów honeypotów (bazy danych, SSH, itp.)
3. Automatyczne alerty i powiadomienia
4. System analizy behawioralnej atakujących
5. Funkcje automatycznego generowania raportów
