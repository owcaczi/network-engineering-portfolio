Poniższa procedura zakłada, że kontener LXC ma już ustawiony **statyczny adres IP** (Static Lease) na routerze lub w konfiguracji sieci Proxmox.

### 1. Instalacja Pi-hole i Unbound
Wykonaj poniższe komendy w konsoli kontenera (jako root).

```bash
# 1. Aktualizacja systemu
apt update && apt upgrade -y

# 2. Instalacja Pi-hole (Postępuj zgodnie z kreatorem)
curl -sSL [https://install.pi-hole.net](https://install.pi-hole.net) | bash

# 3. Instalacja Unbound
apt install unbound -y

# 4. Pobranie aktualnej listy serwerów głównych (Root Hints)
wget [https://www.internic.net/domain/named.root](https://www.internic.net/domain/named.root) -qO- | tee /var/lib/unbound/root.hints

```