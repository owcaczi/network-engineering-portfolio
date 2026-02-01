# 🟧 Proxmox Virtual Environment (PVE)

Główny klaster wirtualizacji (Hypervisor Type-1). Służy do hostowania serwerów aplikacyjnych, kontenerów LXC oraz routerów wirtualnych.

## 🚀 Instalacja (Bare Metal)
1. Pobierz ISO ze strony oficjalnej.
2. Wypal na USB (używając Etcher/Ventoy).
3. Boot -> Wybierz dysk docelowy (ZFS RAID1 zalecany dla redundancji).
4. Ustaw statyczne IP, Gateway i DNS (wskazujący na Pi-hole/Unbound).

## ⚙️ Konfiguracja Post-Install (Shell)

### 1. Repozytoria "No-Subscription"
Domyślne repozytoria wymagają płatnej licencji. Aby aktualizować system za darmo:

```bash
# Wyłącz repozytorium Enterprise
sed -i 's/^/#/' /etc/apt/sources.list.d/pve-enterprise.list

# Dodaj repozytorium No-Subscription
echo "deb [http://download.proxmox.com/debian/pve](http://download.proxmox.com/debian/pve) bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list

# Aktualizacja systemu
apt update && apt dist-upgrade -y