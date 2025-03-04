# [WIP] Campobase

<img src="organization/logo/1/base_logo.png" alt="Base Logo" width="250" />
<img src="https://cdn.rawgit.com/decidim/decidim/master/logo.svg" alt="Decidim Logo" width=200" />

Questa repository contiene il codice dell'immagine Docker su cui si basa l'istanza della piattaforma partecipativa di Base Italia Decidim, `Campobase`.

## Cos'è Decidim

> Free Open-Source participatory democracy, citizen participation and open government for cities and organizations

[Decidim](https://decidim.org) è una piattaforma di democrazia partecipativa, scritta in Ruby on Rails, originariamente sviluppata per il l'attività partecipativa sul sito online e offline del governo della città di Barcellona. L'installazione di queste librerie fornisce un generatore e dipendenze per aiutarti a sviluppare applicazioni web come quelle che si possono trovare su [questi esempi](#example-applications) o come [la nostra piattaforma](https://campobase.baseitalia.net).

Tutti i membri della comunità Decidim aderiscono al [Decidim Social Contract or Code of Democratic Guarantees](http://www.decidim.org/contract/).

Per tutte le istruzioni di utilizzo e installazione della piattaforma decidim rimandiamo alla [repository official decidim](https://github.com/decidim).

---

## Campobase Decidim

Questa repository contiene l'infrastruttura di setup della piattaforma Decidim di Base Italia. Il file docker-compose.yml contiene il setup dei seguenti servizi:

- decidim: immagine di Base Italia basata sulla custom build del Dockerfile presente. L'istanza è sviluppata sulle basi delle necessità dell'istanza del partito, con una serie di configurazioni di default come la lingua, colori, loghi e account (le configurazioni sono basate su variabili d'ambiente che sono impostate nel meccanismo di continuous integration e deployment al server, nessun dato sensibile è qui esposto).
- postgres: database di base su cui si basa la piattaforma. Setup e migrazioni vengono gestiti dall'immagine Decidim
- redis: key value storage per la gestione di alcuni cron e jobs di background necessari per il funzionamento della piattaforma
- mailer: istanza locale di server SMTP per l'invio delle email di registrazione alla piattaforma
- nginx: bunkerized version configurata come reverse proxy che contiene tutte le best practice per la messa in sicurezza della piattaforma

È possibile lanciare la piattaforma in locale con il comando:
> docker-compose up -d

## Configurazione

Questa repository contiene un Makefile con una serie di comandi utili per la gestione dell'applicazione. La configurazione su un nuovo server puo' essere rapidamente eseguita lanciando `make`. Make installera' le dipendenze richieste tra cui Docker e inizializzera' l'infrastruttura con `docker-compose up -d`.

Tra i comandi utili da lanciare con make ci sono `decidim-logs` per stampare i logs di decidim, `decidim` per accedere al container in cui gira l'applicazione`, `db` per accedere al container in cui gira Postgres, `db-dump/restore` per fare un backup e fare il restore dello stato del database, `db-migrate-status` per mostrare lo stato delle migrazioni sul database

## Sviluppo locale

La configurazione di default dell'immagine Docker include la creazione di un utente admin, in carica della creazione delle organizzazioni della piattaforma, e dell'organizazione PartecipAzione, di default. Quindi dopo aver lanciato l'infratstruttua è già possibile visitare la piattaforma. In caso contrario, Decidim reindirizzerebbe sempre alla configurazione di sistema su `/system`, dove l'admin deve effettuare l'accesso e creare la prima organizzazione. 

In questo caso questo passaggio è già eseguito, ma per far si che decidim rilevi la richiesta da parte dell'host corretto, bisogna far si che la chiamata arrivi proprio da `campobase.baseitalia.net`. Per poter camuffare la richiesta bisogna forzare tale host con l'indirizzo ip locale. Per fare ciò aggiungere `localhost campobase.baseitalia.net` al fine `/etc/hosts` o più semplicemente lanciare
> sudo echo "localhost campobase.baseitalia.net" >> /etc/hosts

A questo punto è possibile aprire il browser e visitando campobase.baseitalia.net dovreste vedere la schermata home.

## Contributi

Per contribuire allo sviluppo o alla gestione della piattaforma contattarmi al seguente indirizzo email: `patrick.jusic@protonmail.com`