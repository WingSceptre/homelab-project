# Debian Homelab Infrastructure

Projekt mojego domowego laboratorium (Homelab) opartego na systemie Debian Linux. Środowisko służy do hostowania usług sieciowych, automatyzacji oraz zarządzania instancjami aplikacji w bezpiecznej, odizolowanej sieci lokalnej i VPN.

---

Architektura i Technologie:

- System Operacyjny: Debian Linux (Konfiguracja Hybrydowa: GUI + CLI)
- Konteneryzacja: Docker i Containerd (Zarządzanie przez Portainer)
- Orkiestracja Gier i Aplikacji: AMP (CubeCoders)
- Sieć i Bezpieczeństwo: NetBird (Mesh VPN), Playit.gg (Tunelowanie), Pi-hole (Filtrowanie DNS)
- Dostęp zdalny: SSH, GNOME Remote Desktop

---

Galeria i Dashboardy:

  Tutaj znajdują się zrzuty ekranu działających usług w moim laboratorium:
    
    Panel Portainer (Zarządzanie Dockerem)
    
      <img width="1920" height="930" alt="image" src="https://github.com/user-attachments/assets/fcab2621-fd63-40cb-a525-14aee11fea84" />

    
    Panel AMP (CubeCoders)

    <img width="1920" height="930" alt="image" src="https://github.com/user-attachments/assets/493ea3bd-fac8-4dc2-b7b5-a0393dd7fd74" />

    
    Statystyki Pi-hole


W folderze `docker-compose-templates/` w tym repozytorium umieściłem oczyszczone (pozbawione haseł i tokenów) pliki konfiguracyjne `yaml`, których używam do wdrażania moich usług.
