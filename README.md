# SOU-LAB-CNI

## 🎯 Obiettivo
Creazione Playbook Ansible per deploy Prometheus, Grafana con config per ingestion log Prometheus e HAProxy come reverse proxy per crittografare la connessione dall'esterno verso Grafana e Prometheus

## 📝 Descrizione
Ai fini di gestione e sperimentazione, si è deciso di usare una mappatura locale delle cartelle all'interno dei container piuttosto che la funzione nativa dei volumi podman.
Il Playbook si occupa di eseguire le seguenti task
- Installare podman usando il package manager auto
- Inizializzare e far partire podman
- Creazione strutturata delle varie cartelle necessarie da mappare nei container
- Creazione dei file di config 
- Generazione di una chiave privata a 4096 bit atta alla generazione del certificato PEM per HAProxy
- Generazione del certificato basato sulla precedente chiave privata
- Creazione dei file di configurazione per HAProxy
  - Configurazione path del certificato
  - Configurazione acl con hdr_beg per fare match sull'inizio stringa dato l'uso di una porta non standard
  - Configurazione dei backend
- Creazione file di configurazione Grafana
  - Definizione utenza admin con password non standard
  - Definizione cartella salvataggio dati
  - Definizione direzione log e livello
- Creazione file di config Prometheus
  - Impostato monitoraggio su se stesso
  - Impostato label su "prom-monitor" per tenere facilmente traccia dei dati di questa istanza
- Creazione file di config Datasource per Grafana con impostata l'istanza di prometheus
- Creazione dei due pod richiesti, **soufe1** e **soufe2**
- Creazione Network virtuale condivisa da assegnare ai vari container
- Creazione Container HAProxy su pod soufe1
  - Mappato file di config e cartella ocntenente i certificati generati
  - Specificato comando start con file di config
  - Mappate le porte da esporre
- Creazione Container Grafana
  - Mappato file di configurazione custom
  - Mappata cartella contenente i datasource Yaml
  - Mappata cartella contenente i dati
  - Specificata variabile d'ambiente per file di configurazione custom
- Creazione Container Prometheus
  - Mappato file di configurazione Yaml
  - Mappata cartella contenente i dati(TSDB) di Prometheus
  - Specificato nel comando iniziale cartella dove salvare i dati
  - Specificato nel comando iniziale il file di config
  - Specificato nel comando iniziale  --web.enable-remote-write-receiver per abilitare la ricezione dati da prometheus da altri agenti per usi futuri
## 🔜 TO-DO
- [ ] Configurare Prometheus per raccolta dati host
- [ ] Messa in sicurezza interna di Grafana e Prometheus
- [ ] Creazione Automatizzata Dashboard Custom

---

**Autore:** Daniele Rossi
