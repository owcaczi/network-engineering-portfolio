# 🧪 Advanced Network & System Engineering Lab

![Status](https://img.shields.io/badge/Status-Under_Construction-orange)
![Philosophy](https://img.shields.io/badge/Philosophy-Break_to_Learn-red)
![Goal](https://img.shields.io/badge/Goal-CCNA_%2F_LPIC--3-blue)

> **"Im więcej się psuje, tym lepiej - bo więcej się uczę."**

Ten projekt to nie . To zaawansowany **poligon inżynierski** nastawiony na symulację środowiska Enterprise. Celowo komplikuję architekturę, mieszam vendorów (Cisco, Ubiquiti, Mikrotik) i wdrażam nadmiarowe rozwiązania, aby zrozumieć, jak działają, jak się psują i jak je naprawić.

---

## 🏗️ Architektura i Sprzęt (Hardware)

Obecna baza sprzętowa, która ewoluuje w kierunku klastra HA (High Availability).

| Typ | Sprzęt | Rola / Planowane użycie |
| :--- | :--- | :--- |
| **WAN/Edge** | Orange FTTH 8/1 Gbps + LEOX ONT | XGS-PON Access |
| **Gateway** | Ubiquiti UCG Fiber | IDS/IPS |
| **Core Switch** | Ubiquiti USW-Pro-HD-24 | Zarządzanie VLANami, LACP |
| **Lab Network** | Mikrotik RB5009, Cisco 1921/3560 | Router-on-a-Stick, OSPF/EIGRP, Cisco CLI |
| **Compute Node 1** | Lenovo Tiny M720q | Proxmox VE (docelowo Node w klastrze) |
| **Compute Node 2** | *Planowany zakup (SFF)* | Drugi węzeł do HA / migracji maszyn |
| **Storage** | *NAS In-progress*| iSCSI / ZFS dla klastra wirtualizacyjnego |

---

## 🗺️ Project Roadmap

Poniżej znajduje się lista technologii i konfiguracji, które wdrażam (lub planuję wdrożyć).

> **Cel:** Komplikować życie, mieszać vendory, unikać gotowców, budować od zera.

---

<details>
<summary><b>🏆 Level 1: Networking & Hardcore Firewalling (Kliknij, aby rozwinąć)</b></summary>

<br>

*Celem jest zrozumienie, jak naprawdę działa sieć, wychodząc poza prosty router od dostawcy. Mieszu w vendorach.*

- [ ] **Next-Gen Firewall (NGFW)**
  - [ ] Wdrożenie **Sophos XG Home** (poznanie mechanizmów kontroli SSL/DPI).
  - [ ] Analiza porównawcza: Dlaczego **UniFi Express** jest "gorszy" (brak głębokiej inspekcji SSL) vs Sophos.
  - [ ] Alternatywa/Testy: OPNsense lub PfSense na terminalu (np. Lenovo M720q).
- [ ] **Router-on-a-Stick (RoS) - "Vendor Hell"**
  - [ ] Konfiguracja RoS na mieszanym sprzęcie: Ubiquiti + MikroTik + Sophos.
  - [ ] Celowe wymuszanie routingu między urządzeniami różnych producentów.
- [ ] **Segmentacja sieci (VLANs & Security Zones)**
  - [ ] Utworzenie minimum 5 VLAN-ów:
    - `GUEST` (izolowany całkowicie)
    - `IoT` (izolacja "niebezpiecznych" urządzeń)
    - `HOME INFRA` (zaufane urządzenia)
    - `CAM` (CCTV - odcięcie od Internetu)
    - `DMZ` (dla usług wystawionych na świat, np. Nextcloud)
  - [ ] **Polityki Firewall:** Blokada ruchu między VLAN-ami (zasada *Default Deny*).
  - [ ] Konfiguracja "Zone-Based Firewall".
  - [ ] Ograniczanie przepustowości (QoS/Limiters) między VLAN-ami.

</details>

<details>
<summary><b>🏗️ Level 2: Core Infrastructure Services (Self-Hosted)</b></summary>

<br>

*Przestajemy polegać na routerze w kwestii usług. Wszystko hostujemy sami na serwerach.*

- [ ] **DHCP Server**
  - [ ] Wyniesienie DHCP z routera na dedykowany serwer (Linux/Windows Server).
- [ ] **DNS & AdBlocking**
  - [ ] **AdGuard Home:** Instalacja dwóch instancji (Primary/Secondary) dla High Availability.
  - [ ] **AdGuard Home Sync:** Konfiguracja synchronizacji między instancjami.
  - [ ] **DNS Rewrite:** Lokalne domeny (np. `serwer.lan`) bez wychodzenia do publicznego DNS.
- [ ] **Zarządzanie hasłami & Bezpieczeństwo**
  - [ ] **Vaultwarden (Bitwarden):** Wdrożenie wersji Self-hosted.
  - [ ] Wymóg krytyczny: Wymuszenie HTTPS (szyfrowana transmisja).
- [ ] **Reverse Proxy**
  - [ ] Nauka narzędzi: **Nginx Proxy Manager**, **Traefik** lub **Caddy**.
  - [ ] Cel: Wystawienie usług pod własną domeną (np. `bitwarden.mojadomena.pl`).
- [ ] **Certyfikaty SSL (PKI)**
  - [ ] Let's Encrypt (automatyzacja).
  - [ ] **Hard Mode (LPIC-303):** Własne CA (Certificate Authority), generowanie kluczy, instalacja Root CA na urządzeniach końcowych.

</details>

<details>
<summary><b>☁️ Level 3: Virtualization & Storage (Home Data Center)</b></summary>

<br>

*Budowa wydajnego klastra obliczeniowego i walka z wydajnością I/O.*

- [ ] **Hypervisory - Przegląd rynku**
  - [ ] **Proxmox VE:** Podstawa (minimum pół roku pracy w klastrze).
  - [ ] **XCP-ng + Xen Orchestra:** Alternatywa Open Source.
  - [ ] **VMware ESXi:** (Opcjonalnie, dla znajomości standardu legacy).
- [ ] **High Availability (HA) Cluster**
  - [ ] Minimum 2-3 węzły (PC/SFF, Intel/AMD).
  - [ ] Symulacja awarii: Fizyczne odłączenie węzła ("pull the plug") i test migracji maszyn.
- [ ] **Storage & NAS**
  - [ ] Systemy: **TrueNAS Scale** lub **OpenMediaVault**.
  - [ ] **ZFS:** Zrozumienie pooli, datasetów, snapshotów, ZIL/SLOG.
  - [ ] Protokóły: iSCSI vs NFS dla wirtualizacji.
  - [ ] **Stress Test:** Symulacja pracy 100 użytkowników (generowanie obciążenia I/O).
- [ ] **Networking w wirtualizacji**
  - [ ] Rozwiązanie problemu "wąskiego gardła" 1Gbit.
  - [ ] **Agregacja łączy:** LACP (L2) vs SMB Multichannel (L7).
  - [ ] Instalacja kart 4x1Gb lub 10GbE SFP+ i mapowanie ich do maszyn wirtualnych.
- [ ] **Konteneryzacja**
  - [ ] **LXC:** Lekkie kontenery systemowe (Proxmox).
  - [ ] **Docker & Portainer:** Zarządzanie mikroserwisami.

</details>

<details>
<summary><b>🔐 Level 4: Secure Remote Access & VPN</b></summary>

<br>

*Dostęp do domu z każdego miejsca na ziemi, ale bezpiecznie.*

- [ ] **VPN Tradycyjny**
  - [ ] OpenVPN (TCP 443 - trudny do zablokowania w hotelach/pracy).
  - [ ] WireGuard (szybki UDP).
- [ ] **Mesh VPN (SD-WAN)**
  - [ ] **Tailscale / Netbird:** Omijanie braku publicznego IP (CGNAT).
- [ ] **Tunele**
  - [ ] **Cloudflare Tunnel:** Bez otwierania portów na routerze.
  - [ ] **Pangolin:** Alternatywa Self-hosted dla Cloudflare.

</details>

<details>
<summary><b>🌍 Level 5: VPS & "Exit to Cloud"</b></summary>

<br>

*Wychodzimy z Home Labu na serwery publiczne. Nauka prawdziwego świata.*

- [ ] **Infrastruktura na VPS**
  - [ ] Wynajem VPS (OVH, Hetzner, Oracle).
  - [ ] **Netbird (Self-hosted):** Własny kontroler sieci Mesh na VPS.
  - [ ] **Nextcloud na VPS:** Odciążenie łącza domowego.
  - [ ] **Mail Server (Hard Mode):** Postawienie poczty od zera (Postfix, Dovecot, SPF, DKIM, DMARC) - *zakaz używania gotowców na start*.
- [ ] **Hardening VPS (Security)**
  - [ ] SSH: Zmiana portów, klucze RSA/Ed25519, brak haseł.
  - [ ] **CrowdSec:** Nowoczesny IPS/IDS (analiza behawioralna).
  - [ ] **Wazuh:** SIEM - zbieranie i analiza logów bezpieczeństwa.

</details>

<details>
<summary><b>🆔 Level 6: Identity Management (SSO) & Enterprise</b></summary>

<br>

*Jeden login by wszystkimi rządzić.*

- [ ] **Identity Provider (IdP)**
  - [ ] **Authentik** lub **Keycloak**.
  - [ ] Integracja usług (Proxmox, Portainer, Wiki) przez **OAuth2 / OIDC**.
- [ ] **Active Directory**
  - [ ] Postawienie Windows Server DC.
  - [ ] Integracja usług Linuxowych z AD (LDAP/Kerberos).
- [ ] **MFA / 2FA**
  - [ ] Wymuszenie 2FA wszędzie.
  - [ ] Implementacja kluczy sprzętowych (YubiKey) lub Passkeys.

</details>

<details>
<summary><b>🤖 Level 7: DevOps, Automation & IaC (The Endgame)</b></summary>

<br>

*Koniec z "klikaniem". Wszystko jako kod.*

- [ ] **Ansible (Configuration Management)**
  - [ ] Automatyzacja konfiguracji serwerów (aktualizacje, pakiety).
  - [ ] Tworzenie Playbooków zastępujących ręczną konfigurację.
- [ ] **Terraform (Provisioning)**
  - [ ] Powoływanie maszyn na Proxmoxie/VPS kodem.
- [ ] **Git & CI/CD**
  - [ ] **Gitea:** Własne repozytorium kodu.
  - [ ] **Jenkins / GitHub Actions:** Potoki wdrażania (Pipeline).
  - [ ] Scenariusz: *Zmiana w kodzie -> Terraform stawia VM -> Ansible konfiguruje -> Testy.*
- [ ] **Low-Code Automation**
  - [ ] **n8n:** Automatyzacja powiadomień i przepływów pracy.

</details>

<br>

## 📚 Cele Edukacyjne (Certification Path)

## 🎯 Cele i Certyfikacja

<details>
<summary><b>⏳ Short-term Goals: Cisco CCNA (Kliknij, aby rozwinąć)</b></summary>

<br>

**Status certyfikacji sieciowej (Cisco):**

- [x] **1. CCNA: Introduction to Networks** *Status: DONE ✅* *(Tu możesz wkleić link do swojego badge'a na Credly jako zwykły link, np.: [Zobacz Badge](https://www.credly.com/...))*

- [ ] **2. CCNA: Switching, Routing, and Wireless Essentials** *Status: In Progress 🔄*

- [ ] **3. CCNA: Enterprise Networking, Security, and Automation** *Status: In Progress 🔄*

- [ ] **4. Cisco CCNA 200-301 (Egzamin końcowy)** *Zakres: Routing, Switching, IP Services*

</details>

<details>
<summary><b>🚀 Long-term Goals: Linux Professional Institute (LPIC)</b></summary>

<br>

**Ścieżka administracji systemami Linux (LPI):**

- [ ] **1. LPIC 1-101** *Fundamenty systemu Linux + sieć i storage (baza pod HA).*

- [ ] **2. LPIC-1 102** *Usługi, bezpieczeństwo i automatyzacja podstawowa.*

- [ ] **3. LPIC-2** *Administracja zaawansowana + zarządzanie środowiskami produkcyjnymi.*

- [ ] **4. LPIC 3-305/306** *High Availability (HA), klastry i wirtualizacja (infrastruktura krytyczna).*

- [ ] **5. LPIC 3-303** *Bezpieczeństwo infrastruktury i usług krytycznych.*

</details>

---
*Dokumentacja żyje własnym życiem. Jeśli coś działa - prawdopodobnie jutro to zmienię, żeby sprawdzić inne rozwiązanie.*
