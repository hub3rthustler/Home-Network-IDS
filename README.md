# Home-Network-IDS
**Projekt homelabowy z zakresu cyberbezpieczeństwa, monitorowania sieci i wykrywania zagrożeń.**

Praktyczny projekt monitorowania bezpieczeństwa sieci domowej z wykorzystaniem serwera Proxmox, przełącznika TP-Link TL-SG108E oraz systemu Suricata IDS.

Celem projektu jest przechwytywanie i analizowanie ruchu sieciowego, wykrywanie podejrzanych zdarzeń oraz prezentowanie alertów w przejrzystym panelu analitycznym.

Projekt realizowany jako część portfolio Cybersecurity / SOC Analyst.

## Technologie

| Technologia | Zastosowanie |
|---|---|
| Proxmox VE | Wirtualizacja |
| Ubuntu Server 24.04 LTS | System sensora IDS |
| TP-Link TL-SG108E | Port Mirroring |
| Suricata 8.0.8 | Network Intrusion Detection System |
| EveBox 0.32.0 | Dashboard i analiza alertów |
| SQLite | Przechowywanie zdarzeń EveBox |
| AdGuard Home | DNS Filtering & Statistics |
| Nmap / Zenmap | Symulacja aktywności sieciowej |
| tcpdump / jq | Analiza pakietów i logów JSON |
| Tailscale | Zdalny dostęp VPN |

## Architektura

**Funbox 6 → TL-SG108E (Port Mirroring) → Suricata IDS → EVE JSON → EveBox Dashboard**

Środowisko działa na serwerze Dell OptiPlex 3040 SFF z Proxmox VE.

- VM 104 – Ubuntu Server, Suricata i EveBox
- CT 100 – AdGuard Home
- CT 101 – Uptime Kuma
- CT 102 – Tailscale
- VM 103 – TrueNAS SCALE

Dedykowany adapter Ethernet Realtek, przekazany do VM przez USB Passthrough, odbiera kopie pakietów z portu monitorującego przełącznika.

## Zaimplementowane funkcjonalności

- Pasywne monitorowanie ruchu sieciowego za pomocą Port Mirroring.
- Przechwytywanie pakietów TCP, UDP, DNS i innych protokołów.
- Detekcja podejrzanych zdarzeń z wykorzystaniem Suricata IDS.
- Własna reguła wykrywania zwiększonej liczby pakietów TCP SYN (SID 1000002).
- Zapisywanie zdarzeń w formacie EVE JSON.
- Dashboard EveBox z filtrowaniem i analizą alertów.
- Integracja z API AdGuard Home na poziomie odczytu statystyk DNS.
- Diagnostyka routingu sieciowego i wpływu VPN na widoczność ruchu.

**TESTY:**

`nmap -sT -Pn -p 20-50 --max-rate 10 192.168.1.1`

Suricata wygenerowała alert:

**HOME SOC - Possible TCP SYN Scan**

Alert został zapisany w `eve.json` i wyświetlony w EveBox.

Potwierdzono również przechwytywanie ruchu pomiędzy urządzeniami Wi-Fi a usługami homelabu.

## Ograniczenia

Sensor monitoruje ruch przechodzący przez porty objęte mirroringiem. Nie zapewnia pełnej widoczności całego ruchu wewnętrznego Wi-Fi i wirtualnej sieci Proxmoxa.

Suricata działa w trybie pasywnym IDS – wykrywa zdarzenia, ale nie blokuje automatycznie ruchu.

**Network Security, IDS, Traffic Analysis, TCP/IP, Port Mirroring, Linux Administration, Proxmox, Suricata, JSON Log Analysis, Threat Detection, SOC Monitoring.**

---

**Status projektu:** Active Development

**Środowisko:** Cybersec Homelab
