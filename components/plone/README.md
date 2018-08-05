# C4P Buildout - Plone

Questo è il buildout per far girare un sito Plone.

## Per iniziare

Per poter lanciare il buildout del componente Plone, è necessario eseguire due passi:

- bootstrap
- compilazione del buildout

E' presente un Makefile con una serie di comandi utili.

Per fare velocemente il bootstrap di un modulo, bisogna prima di tutto creare un file buildout.cfg.
Non c'è bisogno di creare per forza un file da zero, ma basta fare un link simbolico ad uno dei file presenti nella cartella "profiles" di ogni modulo.
In questa cartella sono presenti una serie di profili di buildout predefiniti per ogni esigenza (development, staging, production).

Quindi basta eseguire i seguenti comandi:

```bash
ln -s profiles/development.cfg buildout.cfg
make bootstrap
```

In questo modo verranno eseguite le seguenti azioni:

- creazione del virtualenv col python2.7 nella cartella corrente
- installazione nel pip del virtualenv appena creato delle dipendenze necessarie
- compilazione del buildout con il profilo selezionato in precedenza.

Alla fine del processo, ci saranno una serie di comandi nella cartella `bin` per controllare i vari componenti:

- instance (nel caso di compilazione del profilo di development)
- instance1
- instance2
- ...
