# C4P Buildout

Questo è il buildout per far girare un sito camerale c4p.

Con questo buildout, è possibile instanziare un sito camerale con diverse configurazioni:

- sviluppo
- staging
- produzione

## Composizione

La struttura del progetto è suddivisa in 2 parti principali:

- supervisor
- componenti (cartella components)

Supervisor è un sistema per il controllo dei processi attivi.
Serve a coordinare tutti i componenti di questo buildout.

I componenti sono 3, e possono essere utilizzati tutti insieme o soltanto alcuni:

- varnish (http accelerator)
- haproxy (bilanciatore)
- plone (il sito c4p vero e proprio)

In fase di sviluppo si può utilizzare anche solamente il componente Plone, mentre in staging/produzione (quando si usano più di una istanza) è meglio attivare anche haproxy per bilanciare le richieste.

Varnish è un reverse proxy che si occupa anche di gestire la cache delle risorse. E' utile inserirlo nello stack di produzione davanti alle altre risorse, in modo da velocizzare i tempi di caricamento delle pagine del sito pubblico.

Ogni componente (compreso supervisor) è strutturato nello stesso modo, e va compilato col comando di buildout.

In ogni componente sono presenti le istruzioni per la compilazione e la gestione di quello specifico componente.
In questa sezione verrà spiegato il funzionamento si supervisor.

## Prerequisiti

- python2.7
- virtualenv

## Per iniziare

I primi passi da fare per poter utilizzare i vari moduli, sono:

- bootstrap
- compilazione del buildout

E' presente un Makefile con una serie di comandi utili.

Per fare velocemente il bootstrap di un modulo, bisogna prima di tutto creare un file buildout.cfg.
Non c'è bisogno di creare per forza un file da zero, ma basta fare un link simbolico ad uno dei file presenti nella cartella "profiles" di ogni modulo.
In questa cartella sono presenti una serie di profili di buildout predefiniti per ogni esigenza (development, staging, production).

Quindi basta eseguire i seguenti comandi:

```bash
ln -s profiles/simple.cfg buildout.cfg
make bootstrap
```

In questo modo verranno eseguite le seguenti azioni:

- creazione del virtualenv col python2.7 nella cartella corrente
- installazione nel pip del virtualenv appena creato delle dipendenze necessarie
- compilazione del buildout con il profilo selezionato in precedenza.

Alla fine del processo, ci saranno una serie di comandi nella cartella `bin` per controllare i vari componenti:

- supervisord (comando per lanciare il demone supervisor)
- supervisorctl (controllo dei vari processi attivi)
- start (alias for supervisord)
- status (alias for supervisorctl status)
- restart (alias for supervisorctl shutdown && wait some time && supervisord)
- stop (alias for supervisorctl shutdown)
- graceful (alias for supervisorctl restart all)

In aggiunta verranno installati una serie di init script per sistemi debian nella cartella `etc`.
Questi script possono essere copiati nella cartella `/etc/init.d` di sistema, per gestire l'auto start::

```
cp etc/initscript /etc/init.d/c4p.production
update-rc.d c4p.production defaults
```

## Configurazione

Per far eseguire supervisord da un utente differente, basta cambiare il valore di `supervisord-user` nella sezione `[supervisor]`:

```config
[supervisor]
supervisord-user = plone
```

Nei vari profili a disposizione, ci sono una serie di processi predefiniti da monitorare.
Per aggiungerne, toglierne, basta modificare questa lista a piacere.
