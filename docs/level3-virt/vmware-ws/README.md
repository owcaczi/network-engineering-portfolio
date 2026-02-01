# 🟦 VMware Workstation Pro & GNS3

Hypervisor typu 2 (Desktop) używany jako backend dla symulatora sieci GNS3. Pozwala na uruchamianie cięższych obrazów (Cisco IOS-XR, Nexus, Windows Server) wewnątrz topologii sieciowej.

## 🛠️ Instalacja i Opis
VMware Workstation Pro jest obecnie **darmowy do użytku osobistego** (wymagane konto Broadcom).
Działa jako "silnik" dla GNS3 VM, zapewniając lepszą wydajność niż VirtualBox dzięki wsparciu dla KVM (Nested Virtualization).

## 🌐 Integracja z GNS3 (Setup)

### 1. Przygotowanie VMware
* Zainstaluj VMware Workstation Pro.
* W `Virtual Network Editor` zresetuj ustawienia do domyślnych ("Restore Defaults").

### 2. GNS3 VM
1. Pobierz **GNS3 VM.ova** ze strony gns3.com (wersja musi się zgadzać z wersją klienta GNS3!).
2. Zaimportuj plik OVA do VMware.
3. **Kluczowe ustawienie:** Edytuj maszynę -> `Processors`:
   * Zaznacz: `Virtualize Intel VT-x/EPT or AMD-V/RVI`.
   * To pozwala na zagnieżdżoną wirtualizację (KVM wewnątrz VM), co jest niezbędne dla szybkości.

### 3. Połączenie z GUI
1. Włącz GNS3 (aplikację desktopową).
2. Uruchomi się kreator "Setup Wizard".
3. Wybierz: **Run appliances in a virtual machine**.
4. Virtualization software: **VMware Workstation**.
5. GNS3 sam wykryje i uruchomi maszynę w tle.

## ⚠️ Ważne Ustawienia
* **Pamięć RAM:** Przypisz min. 8GB-16GB dla GNS3 VM (routery i firewalle są pazerne na RAM).
* **Wyłączanie:** Zawsze wyłączaj najpierw aplikację GNS3, ona automatycznie wyśle sygnał shutdown do maszyny VMware.