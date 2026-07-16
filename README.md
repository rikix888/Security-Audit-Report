# Security Audit Report: Analisi delle Vulnerabilità di una WLAN Domestica

Questo progetto documenta un'attività di Vulnerability Assessment condotta su una rete locale (WLAN). L'obiettivo è stato simulare il comportamento di un utente interno malintenzionato per verificare la resilienza dell'infrastruttura di rete e dei dispositivi connessi, applicando tecniche di Information Gathering e attacchi Man-In-The-Middle (MITM).

**⚠️ Disclaimer Etico:** *Tutte le attività documentate in questo progetto sono state eseguite in un ambiente di test isolato e controllato (rete privata di proprietà). Non sono stati eseguiti test verso reti esterne o IP pubblici. Questo materiale è pubblicato a scopo puramente didattico e di ricerca in ambito cyber security.*

## 🛠 Strumenti e Tecnologie Utilizzate
* **Sistemi Operativi:** Kali Linux / Linux Mint
* **Information Gathering:** Nmap, curl, nslookup
* **Analisi del Traffico e Spoofing:** Bettercap, Wireshark
* **Protocolli Analizzati:** TCP/IP, ARP, DNS, HTTP/HTTPS

## 🔍 Metodologia di Test

### 1. Information Gathering (Network Mapping)
* **Host Discovery:** Esecuzione di scansioni `nmap -sn` per mappare gli indirizzi IP attivi sulla subnet `192.168.1.0/24`.
* **Port Scanning & OS Detection:** Utilizzo di Nmap con flag aggressivi (`-A -T4 -p-`) per identificare porte aperte, servizi in esecuzione (es. server web Apache/XAMPP) e fingerprinting del sistema operativo.

### 2. Simulazione MITM (Man-In-The-Middle)
* **ARP Spoofing:** Avvelenamento della cache ARP del target e del gateway tramite il modulo `arp.spoof` di Bettercap, configurando il sistema attaccante con IP Forwarding attivo per intercettare il traffico in modalità full-duplex.
* **DNS Spoofing:** Alterazione della risoluzione dei nomi di dominio (`dns.spoof`) per tentare il reindirizzamento delle richieste della vittima verso un indirizzo IP arbitrario.

## 🛡️ Analisi Difensiva e Mitigazione
I test offensivi hanno dimostrato la resilienza dell'infrastruttura grazie all'implementazione di protocolli e configurazioni di sicurezza moderne:

* **Mitigazione ARP Spoofing:** L'isolamento dei client (AP Isolation) configurato sui router moderni impedisce la comunicazione diretta a livello datalink (Layer 2) tra dispositivi wireless, bloccando di fatto il posizionamento MITM.
* **Mitigazione DNS Spoofing (HSTS e HTTPS):** I tentativi di reindirizzamento e intercettazione sono stati bloccati dalle policy HSTS (HTTP Strict Transport Security) dei browser e dall'assenza di certificati TLS validi per i domini falsificati, garantendo l'integrità della crittografia end-to-end.
* **Hardening & Ripristino:** Per ripristinare le tabelle inquinate a seguito dei test, sono state eseguite procedure di pulizia della cache ARP (`arp -d *` / `ip -s -s neigh flush all`) e della cache DNS (`ipconfig /flushdns` / `systemd-resolve --flush-caches`).

---

📄 **Report Completo:** [Scarica il file PDF con il resoconto tecnico e l'analisi dettagliata](./nome-del-tuo-file.pdf)
