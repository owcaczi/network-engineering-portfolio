# 🧪 Advanced Network & System Engineering Lab

![Status](https://img.shields.io/badge/Status-Under_Construction-orange)
![Philosophy](https://img.shields.io/badge/Philosophy-Break_to_Learn-red)
![Goal](https://img.shields.io/badge/Goal-CCNA_%2F_LPIC--3-blue)

> **"Im więcej się psuje, tym lepiej - bo więcej się uczę."**

Ten projekt to nie tylko domowe centrum multimedialne. To zaawansowany **poligon inżynierski** nastawiony na symulację środowiska Enterprise. Celowo komplikuję architekturę, mieszam vendorów (Cisco, Ubiquiti, Mikrotik, Sophos) i wdrażam nadmiarowe rozwiązania, aby zrozumieć, jak działają, jak się psują i jak je naprawić.

---

## 🏗️ Architektura i Sprzęt (Hardware)

Obecna baza sprzętowa, która ewoluuje w kierunku klastra HA (High Availability).

| Typ | Sprzęt | Rola / Planowane użycie |
| :--- | :--- | :--- |
| **WAN/Edge** | Orange FTTH 8/1 Gbps + LEOX ONT | XGS-PON Access |
| **Gateway** | Ubiquiti UCG Fiber ➡️ **Sophos XG Home** | Migracja na NGFW (Deep Packet Inspection / SSL Decrypt) |
| **Core Switch** | Ubiquiti USW-Pro-HD-24 | Zarządzanie VLANami, LACP |
| **Lab Network** | Mikrotik RB5009, Cisco 1921/3560 | Router-on-a-Stick, OSPF/EIGRP, Cisco CLI |
| **Compute Node 1** | Lenovo Tiny M720q | Proxmox VE (docelowo Node w klastrze) |
| **Compute Node 2** | *Planowany zakup (SFF)* | Drugi węzeł do HA / migracji maszyn |
| **Storage** | NAS / Shared Storage | iSCSI / ZFS dla klastra wirtualizacyjnego |

---

## 🗺️ Mapa Drogowa (Project Roadmap)

Poniżej znajduje się lista technologii i konfiguracji, które wdrażam (lub planuję wdrożyć).

### 1. 🛡️ Network Security & NGFW
Celem jest wyjście poza prosty NAT i wdrożenie inspekcji ruchu na poziomie aplikacji (L7).
- [ ] **Wdrożenie Sophos XG Home** na fizycznym sprzęcie (zastąpienie UCG Fiber jako głównej bramy).
- [ ] **SSL Inspection (DPI-SSL):** Instalacja własnego certyfikatu Root CA na urządzeniach końcowych, aby deszyfrować ruch HTTPS.
- [ ] **Zone-Based Firewall:** Konfiguracja stref (LAN, DMZ, IoT, Guest) zamiast prostych reguł in/out.
- [ ] **GeoIP Blocking:** Blokowanie ruchu z krajów wysokiego ryzyka.

### 2. 🕸️ Zaawansowany Networking (VLANs & Routing)
Segmentacja sieci i "utrudnianie sobie życia" routingiem między strefami.
- [ ] **VLAN Segmentation:**
    - `VLAN 1` (Mgmt) - tylko zarządzanie.
    - `VLAN 10` (User) - domownicy.
    - `VLAN 99` (IoT) - całkowita izolacja od Internetu (no WAN access).
    - `VLAN 666` (DMZ) - dla usług wystawionych na świat.
- [ ] **Router-on-a-Stick (RoS):** Konfiguracja na Cisco/Mikrotik i trunking do switcha Ubiquiti.
- [ ] **Bandwidth Control:** Limitowanie przepustowości między VLANami (QoS) - symulacja wąskich gardeł.
- [ ] **DHCP Server Migration:** Przeniesienie DHCP z routera na dedykowany serwer **ISC DHCP** (Linux) dla lepszej kontroli opcji (Option 43, TFTP boot).

### 3. ☁️ Private Cloud & High Availability (HA)
Budowa odpornego klastra wirtualizacyjnego.
- [ ] **Wirtualizacja - Ewolucja:**
    1. Proxmox VE (obecnie).
    2. Migracja do **XCP-ng** (nauka alternatyw Enterprise).
    3. Testy **VMware ESXi** (standard rynkowy).
- [ ] **Cluster HA:** Uruchomienie min. 2 węzłów fizycznych.
    - Symulacja awarii jednego węzła ("odcięcie prądu") i automatyczna migracja VM.
- [ ] **Storage Backend:**
    - Testy wydajności: iSCSI vs NFS vs Ceph.
    - ZFS: Deduplikacja i kompresja danych.
    - Agregacja łączy (LACP) vs SMB Multichannel dla Storage'u.

### 4. 🔐 Identity & Access Management
Bezpieczny dostęp do usług i zarządzanie tożsamością.
- [ ] **Vaultwarden (Bitwarden):** Self-hosted menedżer haseł.
- [ ] **Reverse Proxy:** Nginx Proxy Manager / Traefik.
    - Kierowanie ruchem po domenach (np. `hasla.mojadomena.pl`).
    - Automatyzacja certyfikatów **Let's Encrypt** (Wildcard DNS challenge).
- [ ] **VPN & Remote Access:**
    - WireGuard (szybki dostęp).
    - OpenVPN (TCP 443) - jako backup działający w restrykcyjnych sieciach.
    - **Cloudflare Tunnels** - dostęp do DMZ bez otwierania portów na routerze.

### 5. 📉 Monitoring & DNS
- [ ] **AdGuard Home High Availability:**
    - Dwie instancje (Primary/Secondary).
    - **AdGuardHome-Sync:** Automatyczna synchronizacja reguł między instancjami.
    - DNS Rewrites: Lokalne domeny bez edycji plików `/etc/hosts`.
- [ ] **Monitoring wydajności:**
    - `iperf3`: Testy wydajności sieci wewnątrz VLAN i między VLANami.
    - Wykrywanie "wąskich gardeł" przy wirtualizacji sieciowej (VirtIO).

---

## 📚 Cele Edukacyjne (Certification Path)
Ten lab jest bezpośrednim przygotowaniem do:
1.  **Cisco CCNA 200-301** (Routing, Switching, IP Services).
2.  **LPIC-3 (303 Security & 305/306 Virtualization/HA)** - stąd nacisk na OpenVPN, Certificates, Cluster HA i iSCSI.

---
*Dokumentacja żyje własnym życiem. Jeśli coś działa - prawdopodobnie jutro to zmienię, żeby sprawdzić inne rozwiązanie.*
