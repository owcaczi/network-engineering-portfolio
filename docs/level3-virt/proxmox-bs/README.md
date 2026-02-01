

### 2. ⬛ Proxmox Backup Server
**Ścieżka:** `level3-virt/proxmox-bs/README.md`

Tutaj kluczowe jest sparowanie obu serwerów.

```markdown
# ⬛ Proxmox Backup Server (PBS)

Dedykowany serwer do tworzenia kopii zapasowych z deduplikacją i kompresją. Fizycznie oddzielony od klastra PVE (inny sprzęt).

## 💾 Instalacja na nowym sprzęcie
1. Pobierz ISO Proxmox Backup Server.
2. Zainstaluj na dysku systemowym (SSD zalecany dla bazy danych indeksów).
3. **Datastore:** Sformatuj i zamontuj duże dyski HDD jako główny magazyn danych (np. ZFS Mirror).

## 🔗 Podpięcie pod Proxmox VE

Aby PVE mógł wysyłać backupy do PBS:

1. **W PBS:**
   * Wejdź w `Dashboard` -> `Show Fingerprint` (skopiuj kod SHA256).
   * Wejdź w `Access Control` -> `Add User API Token` (jeśli nie chcesz używać roota).
2. **W Proxmox VE:**
   * Przejdź do `Datacenter` -> `Storage` -> `Add` -> `Proxmox Backup Server`.
   * **ID:** `pbs-backup` (nazwa wyświetlana).
   * **Server:** Adres IP serwera PBS.
   * **Fingerprint:** Wklej skopiowany kod.
   * **Username/Password:** Dane logowania do PBS.

## 🕰️ Strategia Backupów (Prune Simulator)
Ustawienia retencji (jak długo trzymać kopie):
* **Keep Last:** 3 (ostatnie 3 backupy)
* **Keep Daily:** 7 (jeden z każdego dnia tygodnia)
* **Keep Weekly:** 4 (jeden z każdego tygodnia)
* **Keep Monthly:** 12 (jeden z każdego miesiąca)

## 📝 Notatki
* **Dedykowany Port:** 8007 (https://[IP-PBS]:8007)
* **Deduplikacja:** Oszczędność miejsca ~15:1 przy typowych VM systemowych.