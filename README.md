# 🌐 HomeLab & Network Engineering Portfolio

![Status](https://img.shields.io/badge/Status-Active-success)
![Focus](https://img.shields.io/badge/Focus-Network%20Engineering%20%2F%20CCNA-blue)
![Uptime](https://img.shields.io/badge/Uptime-99.9%25-green)

Witaj w dokumentacji mojego HomeLaba. Projekt ten służy jako poligon doświadczalny do nauki zaawansowanych zagadnień sieciowych, przygotowania do certyfikacji **Cisco CCNA** oraz hostowania prywatnych usług w bezpiecznym środowisku.

## 🎯 Cele Projektu
* **Edukacja:** Praktyczna nauka routingu i switchingu (Cisco/Mikrotik) oraz przygotowanie do egzaminu CCNA.
* **Wydajność:** Wykorzystanie łącza światłowodowego 8 Gbps (XGS-PON).
* **Bezpieczeństwo:** Implementacja segmentacji sieci (VLANs), IDS/IPS oraz bezpiecznego dostępu zdalnego (VPN).
* **Prywatność:** Własne serwery DNS i blokowanie śledzenia.

---

## 🗺️ Infrastruktura Sieciowa (Core Network)

Sercem sieci jest infrastruktura oparta o standard **XGS-PON**, zapewniająca przepustowość WAN na poziomie 8/1 Gbit.

| Rola | Urządzenie | Szczegóły |
| :--- | :--- | :--- |
| **ISP** | Orange Fiber | FTTH 8/1 Gbit |
| **ONT** | LEOX LXE-010X-A | Konfiguracja pod XGS-PON |
| **Router / Gateway** | Ubiquiti UCG Fiber | Zarządzanie siecią, IDS/IPS, WireGuard Server |
| **Core Switch** | Ubiquiti USW-Pro-HD-24 | L2/L3 Switching (Non-POE) |
| **Access Point** | Ubiquiti U7 Pro XGS | Wi-Fi 7 Ready |

---

## 🎓 Cisco/Mikrotik Lab (CCNA Study)

Wydzielona sekcja fizyczna służąca do symulacji topologii sieciowych i nauki CLI.

* **Router:** Mikrotik RB5009 UPR (Lab Core)
* **Routery Cisco:** 2x Cisco 1921 (ISR G2)
* **Switche Cisco:** 2x Cisco 3560 (Layer 3)

---

## 🖥️ Serwery i Obliczenia (Compute)

Środowisko do wirtualizacji i konteneryzacji usług.

| Urządzenie | Specyfikacja | OS / Hypervisor | Rola Główna |
| :--- | :--- | :--- | :--- |
| **Lenovo Tiny M720q** | Intel Core i5, RAM rozbudowany | **Proxmox VE** | Wirtualizacja, GNS3 Server, LXC |
| **Raspberry Pi 4B** | ARM64 | **Debian 13** | Usługi lekkie, DNS Backup |

---

## ⚙️ Konfiguracja Logiczna i Bezpieczeństwo

### Segmentacja Sieci (VLANs)
Sieć została podzielona na odseparowane strefy w celu zwiększenia bezpieczeństwa i kontroli ruchu.

| VLAN ID | Nazwa | Opis | Dostęp do Internetu |
| :---: | :--- | :--- | :---: |
| **1** | Native / LAN | Zaufane urządzenia domowe | ✅ Tak |
| **99** | IoT | Izolowana sieć dla urządzeń Smart Home | ❌ Nie (blokada WAN) |
| **100** | Management | Dostęp do interfejsów administracyjnych | ✅ Tak (Restricted) |

### Usługi i Oprogramowanie
* **Symulacja Sieci:** GNS3 Server (uruchomiony na Proxmox) do emulacji złożonych topologii.
* **DNS & Privacy:**
    * *Primary:* AdGuard Home
    * *Secondary/Backup:* Pi-hole (LXC na Proxmox)
* **VPN:** WireGuard (hostowany na UCG Fiber) dla bezpiecznego dostępu do LAN z zewnątrz.

---

## 🔮 Plany Rozwoju (Roadmap)
- [ ] Zdanie egzaminu Cisco CCNA.
- [ ] Implementacja bardziej zaawansowanych reguł Firewall na Mikrotiku.
- [ ] Automatyzacja konfiguracji sieci (Ansible/Python).
- [ ] Rozbudowa monitoringu (Grafana/Prometheus).

---
*Dokumentacja aktualizowana na bieżąco. Ostatnia aktualizacja: 2024.*
