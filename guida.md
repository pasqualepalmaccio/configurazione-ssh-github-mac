#Configurazione SSH Github su Mac OS

# Introduzione
Il supporto di credenziali su HTTPS è stato deprecato in favore del collegamento tramite SSH KEYS su Github. Il concetto è questo: dobbiamo generare le chiavi (pubblica e privata) e aggiungerle all'Agent sulla nostra macchina locale. Dobbiamo infine mettere a conoscenza Github della nostra chiave creandone una nuova e inserendo il testo della nostra chiave pubblica nell'apposita textarea. 

# Iniziamo

Per prima cosa generiamo le nostre chiavi e registriamole nella direcotry <b>~/.ssh/</b> .
<pre><code>ssh-keygen -t ed25519 -C "latuamailgithub@mail.com"</code></pre>

Ci verrà chiesto se assegnare un nome o una path alle chiavi sostituendo quella di default ~/.ssh/. Non dobbiamo modificare nulla ma soltano <b>premere invio</b>. In questo modo verranno create 2 chiavi: <b>id_ed25519</b> ed <b>id_25519.pub</b> nella direcory ~/.ssh/ . 

Nello step successivo dobbiamo inserire una <b>passphrase</b> che dobbiamo ricordarci in quanto sarà richiesta per accedere alla chiave. 

Dopo questo step abbiamo creato le nostre chiavi.

Possiamo visualizzarle tramite: 
<pre><code>ls ~/.ssh/</code></pre>
Qui dentro potrebbero esserci altre chiavi oltre quelle appena generate. 

Il passaggio successivo consiste nell'avviare l'agent ssh lanciando il comando:
<pre><code>eval "$(ssh-agent -s)"</code></pre>
Ci fornirà un output con il Process Id dell'agent. 

A questo punto siamo pronti per creare o aggiungere le istruzioni alla configurazione dell'agent. Per farlo dobbiamo verificare che sia presente nella directory ~/.ssh/ un file chiamato <b>config</b>. 

Proviamo semplicemente ad aprirlo e verifichiamo se esiste:

<pre><code>open ~/.ssh/config</code></pre>

Se l'output da errore "file non esistente" possiamo crearlo.

<pre><code>touch ~/.ssh/config</code></pre>

Se esiste il file dobbiamo modificarlo per scriverci le nostre istruzioni:

<pre><code>nano ~/.ssh/config</code></pre>
Aggiungiamo in questo file il seguente blocco di istruzioni. 
<pre><code>Host *.github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519</code></pre>

  Ci siamo quasi.. 
  Ora possiamo aggiungere la nostra chiave:
  <pre><code>ssh-add --apple-use-keychain ~/.ssh/id_ed25519
</code></pre>

L'ultimo passaggio è quello di copiare il contenuto della nostra chiave pubblica per poi inserirlo su github. Possiamo farlo semplicemente con: 
<pre><code>pbcopy < ~/.ssh/id_ed25519.pub
</code></pre>

##Completiamo la configurazione su Github

Apriamo github, clicchiamo sul profilo in alto a destra e clicchiamo sulla voce <b>settings</b>.

Nella sidebar clicchiamo sulla voce <b>SSH and GPG keys</b> e creiamo una nuova chiave tramite <b>New Key</b>

Forniamo un <b>titolo</b> che ci piace (In genere ne uso uno che indica il dispositivo in cui c'è quella chiave "Macbook Pro")
e nel campo <b>key</b> incolliamo il testo copiato tramite il comando <em>pbcopy</em> precedente. 
La chiave è di tipo <b>Authentication</b>.

A qesto punto abbiamo finito e possiamo interagire con i repository tramite ssh. 