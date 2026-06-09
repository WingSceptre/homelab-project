# Debian Homelab Infrastructure

Projekt domowego laboratorium (Homelab) opartego na systemie Debian Linux. Środowisko służy do hostowania usług sieciowych, automatyzacji procesów oraz zarządzania odizolowanymi kontenerami i instancjami aplikacji w bezpiecznej sieci lokalnej oraz VPN.

---

## 📌 Spis treści
- [🛠️ Architektura i Technologie](#architektura)
- [📊 Dashboardy i Usługi](#dashboardy)
  - [🐳 Panel Portainer (Docker)](#portainer)
  - [🎮 Panel AMP (Serwery Gier)](#amp)
  - [🛡️ Pi-hole (Serwer DNS)](#pihole)
- [📂 Repozytorium Konfiguracji](#repozytorium)
- [🗺️ Schemat Architektury Sieciowej (ASCII)](#schemat)

---

## <a id="architektura"></a>🛠️ Architektura i Technologie

- **System Operacyjny:** Debian Linux (Konfiguracja hybrydowa: CLI jako baza serwerowa + GUI GNOME do celów administracyjnych)
- **Konteneryzacja & Orkiestracja:** Docker, Containerd oraz Docker Compose (Zarządzanie środowiskiem poprzez panel Portainer)
- **Zarządzanie Serwerami Gier:** AMP (CubeCoders)
- **Sieć i Bezpieczeństwo:** NetBird (Mesh VPN), Playit.gg (Bezpieczne tunelowanie ruchu), Pi-hole (Lokalny DNS Sinkhole)
- **Dostęp zdalny:** Protokół SSH, GNOME Remote Desktop

---

## <a id="dashboardy"></a>📊 Dashboardy i Usługi

Poniżej znajdują się zrzuty ekranu prezentujące wdrożone i w pełni działające usługi wchodzące w skład infrastruktury laboratoryjnej.

### <a id="portainer"></a>🐳 Panel Portainer (Docker)

Główny panel administracyjny wykorzystywany do wdrażania, monitorowania oraz zarządzania cyklem życia mikroserwisów. W środowisku uruchomione są stosy aplikacyjne (Stacks) realizujące zadania m.in. z zakresu lokalnego AI (Ollama), cyfryzacji dokumentów (Paperless-ngx), hostingu multimediów (Jellyfin), routingu (Nginx Proxy Manager) oraz ciągłego monitoringu statusu usług (Uptime Kuma). Wszystkie kontenery pracują w odizolowanych sieciach wirtualnych Dockera.

<img width="1920" height="930" alt="image" src="https://github.com/user-attachments/assets/fcab2621-fd63-40cb-a525-14aee11fea84" />

---

### <a id="amp"></a>🎮 Panel AMP (Serwery Gier)

W ramach testów laboratoryjnych wdrożyłem platformę AMP. Służy ona jako prywatne środowisko stagingowe oraz serwer rozrywkowy, z którego korzystam wspólnie z przyjaciółmi. Projekt pozwala w praktyce testować alokację zasobów oraz stabilność usług pod zmiennym obciążeniem.

**Specyfikacja środowiska:**
- **Host:** System Debian 13 uruchomiony na procesorze AMD Ryzen 5 PRO 3400G z pulą 16 GB pamięci RAM.

**Konfiguracja instancji:**
- **ServerHeavyModded:** Prywatna, silnie zmodyfikowana instancja serwera Minecraft Java. Z uwagi na wymagające paczki modyfikacji, przydzieliłem jej dedykowane 8 GB pamięci RAM. Serwer utrzymuje stabilne 20/20 TPS przy jednoczesnej rozgrywce wieloosobowej, co potwierdza poprawną optymalizację środowiska uruchomieniowego Java (JVM).
- **ServerVanillaCopper:** Instancja pomocnicza, utrzymywana w trybie offline w celu optymalizacji i oszczędzania zasobów systemowych maszyny, gdy nie jest używana.

**Bezpieczny dostęp zewnętrzny (Playit.gg):**
Aby umożliwić stabilne i bezpieczne połączenie użytkownikowi spoza sieci LAN (WAN), wykorzystałem dedykowany tunel dla protokołu Minecraft Java w usłudze Playit.gg. Pozwoliło to na bezpieczne wystawienie usługi na świat bez konieczności ponoszenia kosztów publicznego adresu IP u dostawcy internetu oraz bez ryzykownego otwierania portów na domowym routerze.

<img width="1920" height="930" alt="image" src="https://github.com/user-attachments/assets/61765d12-2418-459b-b34c-256609b08b3f" />

---

### <a id="pihole"></a>🛡️ Pi-hole (Serwer DNS)

Wdrożyłem serwer Pi-hole działający jako lokalny serwer DNS oraz tzw. DNS Sinkhole. Narzędzie to automatycznie filtruje ruch sieciowy dla wszystkich urządzeń w sieci lokalnej oraz w sieci VPN, blokując telemetrię, zapytania śledzące oraz złośliwe domeny bezpośrednio na poziomie zapytań DNS.

**Kluczowe parametry wdrożenia:**
- **Wydajność sieciowa:** Przetwarzanie ponad 33 000 zapytań DNS w skali doby na potrzeby sieci domowej.
- **Efektywność blokowania:** Skuteczne odfiltrowanie około 6% ruchu (ponad 2 000 zablokowanych zapytań telemetrycznych i reklamowych).
- **Zarządzanie bazą filtrów:** Integracja list blokowania obejmujących ponad 83 000 znanych domen serwujących niechciany ruch.

Dzięki temu rozwiązaniu infrastruktura zyskała dodatkową warstwę bezpieczeństwa na brzegu sieci (Edge Security). Urządzenia końcowe zużywają less pasma sieciowego, a użytkownicy zostali skutecznie odcięci od uciążliwych reklam.

<img width="1920" height="930" alt="Screenshot From 2026-06-09 14-43-02" src="https://github.com/user-attachments/assets/ca2efa28-3377-4d7c-84ae-e72b4d5ea86c" />

---

## <a id="repozytorium"></a>📂 Repozytorium Konfiguracji

W folderze `docker-compose-templates/` w tym repozytorium umieściłem oczyszczone (pozbawione wrażliwych danych, haseł i tokenów) pliki konfiguracyjne `yaml`, których używam do wdrażania moich usług w środowisku Docker.

---

## <a id="schemat"></a>🗺️ Schemat Architektury Sieciowej (ASCII)

Poniższy graf przedstawia logiczny podział infrastruktury laboratoryjnej na pojedyncze mikroserwisy oraz wielokontenerowe stosy aplikacyjne (Stacks). Schemat ilustruje routing zapytań WWW przez Reverse Proxy (NPM), centralną filtrację zapytań na poziomie DNS (Pi-hole) oraz izolację poszczególnych warstw bazodanowych i aplikacyjnych wewnątrz sieci wirtualnych Dockera.

```text
                                  [ INTERNET / SIEĆ LAN ]
                                             │
             ┌───────────────────────────────┴───────────────────────────────┐
             ▼ (Zapytania DNS - Port 53)                                     ▼ (Ruch Web - Porty 80/443)
        ┌──────────┐                                                   ┌───────────┐
        │ Pi-hole  │ ──► [ Filtrowanie DNS / Blokowanie reklam ]       │   Nginx   │
        │ Port 8090│                                                   │   Proxy   │
        └──────────┘                                                   │  Manager  │
                                                                       └─────┬─────┘
                                                                             │ (Reverse Proxy)
         ┌──────────────┬──────────────┬──────────────┬──────────────┬───────┴──────┬──────────────┬──────────────┐
         ▼              ▼              ▼              ▼              ▼              ▼              ▼              ▼
    ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
    │ Heimdall │   │ Jellyfin │   │ Stirling │   │  Uptime  │   │Portainer │   │  Ollama  │   │Paperless │   │ Odysseus │
    │Dashboard │   │  Media   │   │   -PDF   │   │   Kuma   │   │  WebUI   │   │Local AI  │   │Webserver │   │   App    │
    │ Port 8052│   │ Port 8096│   │ Port 9900│   │ Port 3001│   │ Port 9000│   │Port 11434│   │ Port 8010│   │ Port 7000│
    └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────┬───┘   └──────┬───┘
                                                                                                     │              │
                                   ┌─────────────────────────────────────────────────────────────────┘              │
                                   ▼                                                                                ▼
                     ┌─────────────────────────────┐                                                 ┌─────────────────────────────┐
                     │    STACK: PAPERLESS-NGX     │                                                 │       STACK: ODYSSEUS       │
                     │  ┌────────────────────────┐ │                                                 │  ┌────────────────────────┐ │
                     │  │ paperless-webserver-1  │ │                                                 │  │  odysseus-odysseus-1   │ │
                     │  └───────────┬────────────┘ │                                                 │  └───────────┬────────────┘ │
                     │              │              │                                                 │              │              │
                     │      ┌───────┴───────┐      │                                                 │      ┌───────┴───────┐      │
                     │      ▼               ▼      │                                                 │      ▼               ▼      │
                     │ ┌─────────┐     ┌─────────┐ │                                                 │ ┌─────────┐     ┌─────────┐ │
                     │ │paperless│     │paperless│ │                                                 │ │odysseus-│     │odysseus-│ │
                     │ │ -db-1   │     │-broker-1│ │                                                 │ │chromadb1│     │searxng-1│ │
                     │ │(MariaDB)│     │ (Redis) │ │                                                 │ │(Pt 8100)│     │(Pt 8080)│ │
                     │ └─────────┘     └─────────┘ │                                                 │ └─────────┘     └─────────┘ │
                     │      ▼               ▼      │                                                 │              ▼              │
                     │ ┌─────────┐     ┌─────────┐ │                                                 │         ┌─────────┐         │
                     │ │paperless│     │paperless│ │                                                 │         │odysseus-│         │
                     │ │ -tika-1 │     │-gotenb-1│ │                                                 │         │ ntfy-1  │         │
                     │ │  (OCR)  │     │ (Conv.) │ │                                                 │         │(Port8091│         │
                     │ └─────────┘     └─────────┘ │                                                 │         └─────────┘         │
                     └─────────────────────────────┘                                                 └─────────────────────────────┘
