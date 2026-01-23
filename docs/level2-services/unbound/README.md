###  Konfiguracja Unbound
Utwórz nowy plik konfiguracyjny dedykowany dla Pi-hole:

```bash
nano /etc/unbound/unbound.conf.d/pi-hole.conf
server:
    # Logowanie: 0 to tylko błędy krytyczne
    verbosity: 0

    # Port nasłuchu (inny niż standardowy 53, aby nie gryzł się z Pi-hole)
    interface: 127.0.0.1
    port: 5335
    
    do-ip4: yes
    do-udp: yes
    do-tcp: yes

    # Obsługa IPv6 (zmień na 'no' jeśli nie masz publicznego IPv6)
    do-ip6: yes
    prefer-ip6: no

    # Ścieżka do pliku Root Hints
    root-hints: "/var/lib/unbound/root.hints"

    # Bezpieczeństwo DNSSEC
    harden-glue: yes
    harden-dnssec-stripped: yes
    use-caps-for-id: no

    # Zapobieganie fragmentacji pakietów
    edns-buffer-size: 1232

    # Optymalizacja (Prefetching) - odświeżanie popularnych wpisów przed wygaśnięciem
    prefetch: yes
    
    # Wydajność (dla małych maszyn 1 wątek jest wystarczający)
    num-threads: 1
    so-rcvbuf: 1m

    # Prywatność: Ukrywanie lokalnych podsieci przed zapytaniami na zewnątrz
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10
```

3. Restart i Weryfikacja
Po zapisaniu konfiguracji zrestartuj usługę i sprawdź, czy rozwiązuje nazwy.
# Restart usługi Unbound
sudo service unbound restart

# Test działania
# Powinien zwrócić: "status: NOERROR" oraz adres IP
dig pi-hole.net @127.0.0.1 -p 5335

4. Konfiguracja w GUI Pi-hole
Ostatni krok wykonujemy w przeglądarce.

Zaloguj się do panelu Pi-hole (np. http://<IP-PIHOLE>/admin).

Przejdź do zakładki Settings > DNS.

W sekcji Upstream DNS Servers:

Odznacz wszystkie predefiniowane serwery (Google, OpenDNS, Quad9).

W polu Custom 1 (IPv4) wpisz: 127.0.0.1#5335.

(Opcjonalnie) Odznacz Use DNSSEC, ponieważ Unbound robi to już sam.

Zjedź na sam dół i kliknij Save.