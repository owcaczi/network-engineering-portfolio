>#  Mapa VLAN (Production)

| VLAN ID | Nazwa | Adresacja | Przeznaczenie |
| :--- | :--- | :--- | :--- |
| **1** | NATIVE/MGMT | x.x.x.x/24 | Infrastruktura sieciowa (Switch, AP). |
| **10** | LAB | 10.0.10.0/24 | Proxmox, Zabbix, kontenery. |
| **20** | HOME | 10.0.20.0/24 | Zaufane urządzenia (PC, Laptop). |
| **30** | IOT | 10.0.30.0/24 | Smart Home (Izolacja). |
| **99** | GUEST | 10.0.99.0/24 | Izolowany ruch gościnny. |
