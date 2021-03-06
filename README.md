# Nodo Bitcoin

Repository creata per contenere tutti i file di configurazione e relativi servizi per implementare un nodo Bitcoin e Lightning

Per poter implementare un nodo Bitcoin è richiesta almeno una capacità di 500GB, consigliato 1TB

### Guida step-by-step per installare un nodo Bitcoin e Lightning su Ubuntu
* Servizi:
  * [Bitcoin Core](#bitcoin-core)
  * [Block Explorer (btc-rpc-explorer)](#btc-rpc-explorer)
  * [C-lightning](#c-lightning)

## Bitcoin Core

### Installazione
```bash

# Scarica il binario
wget https://bitcoincore.org/bin/bitcoin-core-0.21.0/bitcoin-0.21.0-x86_64-linux-gnu.tar.gz

# Estrai il binario
tar -xvf bitcoin-0.21.0-x86_64-linux-gnu.tar.gz

# copia tutto il contenuto della cartella nella directory /usr/bin
sudo cp bitcoin-0.21.0/bin/* /usr/bin
```
### Configurazione
Crea e modifica il file di configurazione `bitcoin.conf`

```bash
mkdir -p ~/.bitcoin
nano ~/.bitcoin/bitcoin.conf
```
Copia e incolla le seguanti informazioni, infine salva
```bash
server=1
txindex=1
```

### Avvia Bitcoin
```bash
bitcoind
```

Testa se funziona
```bash
bitcoin-cli getblockchaininfo
```

## btc-rpc-explorer

### Installazione Screen (se non istallato)
```bash
screen --version

# Se non è installato lanciare il comando

sudo apt install screen
```


### Installazione
```bash
# Pulisci da eventuali installazioni precedenti
rm -rf ~/btc-rpc-explorer

# Download
git clone https://github.com/janoside/btc-rpc-explorer
cd ~/btc-rpc-explorer

# Installa il Block Explorer
npm install

# Avvia il Block Explorer
npm start
```

### Configurazione

Configura .env `~/.config/btc-rpc-explorer.env` 
Puoi trovarlo direttamente nella repository Configuration File 

Visita [btc-rpc-explorer docs](https://github.com/janoside/btc-rpc-explorer) per saperne di più.

### Avvia

Scarica li script presenti nella cartella btc-rpc-explorer-script, salva nella cartella btc-rpc-explorer solo explorer.sh, gli altri script lasciali nella directory principale
```bash
chmod +x explorer.sh
chmod +x start-explorer.sh 
```

### Avvia lo script
```bash
./start-explorer.sh 
```
### Avvio automatico in caso di reboot
```bash
crontab -e

#incollare in fondo al file
@reboot    ~/start-boot.sh
```

Apri http://localhost:3002/node-status oppure nel nostro caso http://"IP":3002/node-status


## c-lightning

### Installazione
```bash
# Dipendenze
sudo apt-get update
sudo apt-get install -y \
  autoconf automake build-essential git libtool libgmp-dev \
  libsqlite3-dev python3 python3-mako net-tools zlib1g-dev libsodium-dev \
  gettext
  
# Pulisci da eventuali installazioni precedenti
rm -rf ~/lightning &&

# Download
git clone https://github.com/ElementsProject/lightning.git
cd lightning

# Installa c-lightning
./configure
make
sudo make install
```

### Configurazione

Crea e modifica il file di configurazione

```bash
mkdir -p ~/.lightning
nano ~/.lightning/config
```
Copia e incolla le seguanti informazioni, infine salva
```bash
#Sostituire user con il vostro nome utente (quello con cui vi loggate su linux)

alias="NOME_DEL_NODO"
log-level=debug
log-file=/home/user/.lightning/bitcoin/debug.log

#Nel caso abbiate un IP Statico, altrimenti cancellate la parte sottostante
bind-addr=
announce-addr="IP"
```

### Avvia
```bash
lightningd
```

Testa se funziona
```bash
lightning-cli getinfo
```
