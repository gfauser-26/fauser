# fauser

Il programma della classe per **esercizi, compiti e verifiche**. Un solo file eseguibile per Windows, macOS e Linux: crea la cartella giusta, scarica traccia e test, prova il tuo codice e consegna.

**[⬇ Scarica l'ultima versione](https://github.com/gfauser-26/fauser/releases/latest)** · [Il flusso dell'organizzazione](https://github.com/gfauser-26) · [Le tracce](https://github.com/gfauser-26/tracce)

> Questo repository contiene solo le **release** e questa guida. Non serve clonarlo.

---

## Cosa fa

```text
fauser nuovo E01 --materia tpsit      → crea tpsit/2026-27/esercizi/E01-<data>-<titolo>/
                                        e scarica TRACCIA.md e i test pubblici
fauser test                            → compila, prova ogni caso, scrive la ricevuta .esito
fauser consegna                        → rilancia i test, commit "E01: consegna", git push
```

Il tuo codice resta **sempre e solo** nel tuo repository privato `fauser-<username>`. `fauser` non invia niente a nessuno: il push lo fa Git, verso il tuo repository.

---

## Installazione

### 1. Scarica il file giusto

Dalla pagina [releases/latest](https://github.com/gfauser-26/fauser/releases/latest) scarica il file del tuo sistema **e** `SHA256SUMS`.

| Sistema | File |
|---|---|
| Windows (PC normali) | `fauser-windows-amd64.exe` |
| Windows su ARM | `fauser-windows-arm64.exe` |
| macOS con chip Apple (M1, M2, M3...) | `fauser-darwin-arm64` |
| macOS con Intel | `fauser-darwin-amd64` |
| Linux | `fauser-linux-amd64` (o `fauser-linux-arm64`) |

Non sai che Mac hai? Menu  → «Informazioni su questo Mac»: «Chip Apple» = `arm64`, «Intel» = `amd64`.

### 2. Controlla il file

Il numero stampato deve essere **uguale** a quello della riga del tuo file dentro `SHA256SUMS`.

```bash
# Windows (Git Bash) e Linux
sha256sum <file-scaricato>
# macOS
shasum -a 256 <file-scaricato>
```

### 3. Mettilo nel PATH con il nome `fauser`

```bash
# macOS e Linux
chmod +x <file-scaricato>
sudo mv <file-scaricato> /usr/local/bin/fauser

# Windows (Git Bash)
mkdir -p ~/bin
mv <file-scaricato> ~/bin/fauser.exe
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc     # una volta sola, poi apri un Git Bash nuovo
```

### 4. Prova

```bash
fauser versione
```

Deve rispondere `fauser 0.1.0 [firmato]` (il numero cambia con le versioni). Se dice **`[DEV]`** hai un binario di prova: scarica quello della release.

- **macOS** blocca il file? `xattr -d com.apple.quarantine /usr/local/bin/fauser`
- **Windows SmartScreen** avvisa? «Ulteriori informazioni» → «Esegui comunque», **solo** se il controllo SHA256 è andato bene.

### Aggiornare

Quando il docente annuncia una versione nuova, ripeti i passi 1-4 sovrascrivendo il file. Controlla con `fauser versione`.

---

## Cosa ti serve oltre a fauser

- **Git** e un account GitHub con accesso a `gfauser-26` (vedi la [pagina dell'organizzazione](https://github.com/gfauser-26)).
- Il **compilatore o interprete** del tuo linguaggio. `fauser` usa quello installato sul tuo computer:

| `lang.txt` | Serve | File principale in `src/` | Controlla con |
|---|---|---|---|
| `c` | gcc o clang | `main.c` (anche altri `.c`) | `gcc --version` |
| `cpp` | g++ o clang++ | `main.cpp` (anche altri `.cpp`) | `g++ --version` |
| `csharp` | .NET SDK | `Program.cs` | `dotnet --version` |
| `java` | JDK (javac + java) | `Main.java`, classe `Main`, senza `package` | `javac -version` |
| `python` | Python 3 | `main.py` | `python3 --version` |
| `go` | Go | `main.go`, `package main` | `go version` |
| `ts` | Node 22.6+ (meglio con `tsx`) | `main.ts` | `node --version` |
| `js` | Node.js | `main.js` | `node --version` |
| `web` | niente | pagina web o dataset, controlli di struttura | |

Le installazioni passo passo per ogni sistema sono nella **guida PDF** del docente.

---

## I comandi

| Comando | Cosa fa |
|---|---|
| `fauser lingua [linguaggio]` | imposta (o mostra) il tuo linguaggio principale, nel file `.fauser.json` alla radice del repository |
| `fauser nuovo <ID> --materia <materia>` | crea la cartella della consegna e scarica `TRACCIA.md`, i test pubblici e gli eventuali allegati (per esempio `dati.json`) |
| `fauser test [cartella]` | controlla la struttura, compila, esegue ogni caso di `tests/` e scrive `.esito` |
| `fauser test --timeout 20s` | più tempo per ogni caso (predefinito 10 s) |
| `fauser consegna [cartella]` | `fauser test`, poi `git add`, commit `<ID>: consegna` e `git push` |
| `fauser consegna --forza` | consegna anche se i test falliscono: la ricevuta lo dichiara. Spiegalo nel README |
| `fauser versione` | versione del programma |

Materie: `tpsit`, `informatica`, `gpoi`. Codici di uscita: `0` tutto ok, `1` test o controlli falliti, `2` comando sbagliato o compilatore mancante.

---

## Il ciclo di un esercizio

```bash
cd ~/scuola/fauser-<username>
git pull --rebase                         # prendi le novità
fauser nuovo E01 --materia tpsit          # cartella + traccia + test
cd tpsit/2026-27/esercizi/E01-<data>-<titolo>
code .                                    # leggi TRACCIA.md, scrivi in src/ e il README.md
fauser test                               # [OK] / [KO] per ogni caso
git add . && git commit -m "E01: leggo i due numeri" && git push   # commit piccoli, spesso
fauser consegna                           # alla fine
```

Cosa trovi nella cartella:

```text
E01-2026-10-09-somma-di-due-numeri/
├── TRACCIA.md   la consegna del docente
├── lang.txt     il linguaggio, una parola
├── src/         il TUO codice: scrivi solo qui
├── tests/       01.in 01.out 02.in 02.out ...  (non modificarli)
├── README.md    lo scrivi tu: cosa fa, come l'hai pensato, «Uso dell'AI»
└── .esito       la ricevuta di fauser test (non toccarla)
```

Nelle **verifiche** c'è una cartella per esercizio (`es1/`, `es2/`...), ognuna con il suo `lang.txt`: il linguaggio è libero. Prova un esercizio con `fauser test es1`, tutta la verifica con `fauser test`.

---

## Un comando per consegnare: `fauser consegna`

Non serve ricordarsi tre comandi. **`fauser consegna`** fa tutto in ordine e si ferma al primo problema:

1. controlla **nomi e struttura** (vedi sotto);
2. compila ed esegue **tutti i test pubblici** e scrive `.esito`;
3. se tutto passa: `git add` della cartella, commit **`<ID>: consegna`**, **`git push`**;
4. stampa **`Consegnato: <ID>`** in verde.

Se vedi un **`[KO]`** o **`Errore:`** in rosso, la consegna **non è partita**: leggi il messaggio, correggi e rilancia. Alla fine controlla su github.com che il commit `<ID>: consegna` ci sia.

---

## I nomi da rispettare

Il docente ricorregge tutte le consegne in automatico: un nome sbagliato vuol dire «file non trovato», e la consegna risulta vuota anche se il codice è giusto. Le **maiuscole contano** (`Main.java` non è `main.java`).

| Cosa | Regola | Giusto | Sbagliato |
|---|---|---|---|
| Repository | quello che ti dà il docente, non ne crei altri | `fauser-pixel-volpe` | `esercizi-tpsit`, una copia personale |
| Percorso | `<materia>/2026-27/<esercizi\|compiti\|verifiche>/<cartella>/`, lo crea `fauser nuovo` | `tpsit/2026-27/esercizi/E01-2026-10-09-somma/` | cartella spostata, rinominata o creata a mano |
| Materia | minuscola: `tpsit`, `informatica`, `gpoi` | `tpsit` | `TPSIT`, `Tpsit` |
| Cartella | `<ID>-<AAAA-MM-GG>-<titolo>`, titolo minuscolo con trattini | `C01-2026-10-13-array` | `C1-13-10-array`, `C01 array`, `C01-2026-10-13-Array` |
| ID | `E01`, `C01` (due cifre), `V1` | `E07` | `e07`, `E7`, `Es7` |
| Verifica | una cartella per esercizio: `es1/`, `es2/`... | `es1/` | `Es1/`, `esercizio1/`, `es_1/` |
| `lang.txt` | una parola minuscola | `python` | `Python`, `py`, `python3`, `C#` |
| File principale | nome fisso in `src/` (tabella sopra) | `src/Main.java` | `src/main.java`, `Main.java` fuori da `src/` |
| Java | classe `public class Main`, **senza** `package` | `public class Main {` | `package esercizio;`, `class Somma` |
| C# | un file `Program.cs` (o un `.csproj` minimo) | `src/Program.cs` | un progetto con più cartelle |
| Go | `package main` | `package main` | `package somma` |
| Test | coppie `01.in` / `01.out` in `tests/`, non modificate | `tests/01.in` | `tests/test1.txt`, `.out` corretti a mano |
| README | `README.md` nella cartella della consegna, non vuoto | `README.md` | `readme.txt`, solo il titolo |
| Input/output | standard input e standard output, niente messaggi in più | `print(a + b)` | `print("Il risultato è", a + b)` |
| Commit | il messaggio comincia con l'ID | `E01: gestisco lo zero` | `fatto`, `aaa`, `update` |

### Cosa controlla `fauser` e cosa no

| Problema | Te lo dice `fauser`? |
|---|---|
| Nome della cartella fuori formato, ID sbagliato | **sì**, si ferma |
| `lang.txt` mancante o con un linguaggio sbagliato | **sì** |
| File principale mancante o con il nome sbagliato, `src/` mancante | **sì** |
| `README.md` mancante o vuoto | **sì** |
| `tests/` mancante o con un `.in` senza il suo `.out` | **sì** |
| Verifica senza `es1/`, `es2/`... | **sì** |
| Non compila, output diverso, troppo lento | **sì**, caso per caso |
| Push rifiutato o login mancante | **sì**, «push non riuscito» |
| Cartella **spostata** nel posto sbagliato del repository | no: usa sempre `fauser nuovo` e non spostare |
| `tests/` **modificati** per far passare il codice | no, ma il docente usa gli originali: lì falliscono |
| File cambiati **dopo** `fauser test` | lo vede il docente (`.esito` «manomessa»): usa `fauser consegna`, che rilancia i test |
| Push arrivato **dopo la scadenza** | no: controlla l'ora |
| Test **nascosti** | no: pensa ai casi limite |

> **Attenzione a `--forza`.** Serve per consegnare un lavoro **che non passa tutti i test**, non per saltare i controlli sui nomi: se il problema è un nome sbagliato, correggilo, altrimenti il docente non riesce a eseguire il tuo codice.

---

## Come funzionano i test

Ogni caso è una coppia di file: **`01.in`** è quello che il programma legge da standard input, **`01.out`** quello che deve scrivere su standard output. `fauser` confronta l'uscita **carattere per carattere** (gli a capo Windows e Linux valgono uguale).

- Niente «Inserisci un numero:» se la traccia non lo chiede: è una riga in più e il caso va in **[KO]**.
- Per un caso KO `fauser` mostra **atteso** e **ottenuto**: leggili riga per riga.
- Prova a mano un caso: `python3 src/main.py < tests/01.in` (o il comando del tuo linguaggio).
- Il codice viene compilato in una cartella temporanea: `src/` resta pulita.

I test pubblici sono una parte. Il docente rifà girare **anche i test nascosti**: pensa ai casi particolari (zero, negativi, input vuoto, valori grandi).

### Le consegne `web`

Per una pagina web o un dataset la traccia porta un `controlli.json` al posto dei test. `fauser test` non esegue niente: controlla che i file richiesti ci siano, non siano vuoti e contengano quello che serve (per esempio una `fetch` di `dati.json`). Passarlo non vuol dire che il lavoro sia buono: lo valuta il docente.

### La ricevuta `.esito`

`fauser test` scrive una ricevuta firmata con l'impronta (hash) di ogni file. Se **dopo** il test cambi anche un solo carattere, la ricevuta non torna più e il docente la vede «manomessa». Rimedio: **rilancia `fauser test`** dopo l'ultima modifica (o usa `fauser consegna`, che lo fa da solo).

La ricevuta non è il voto: dice solo che sul tuo computer, a quell'ora, i test passavano. Il voto si basa sul rerun del docente e su quello che sai spiegare.

---

## Problemi frequenti

| Messaggio o problema | Cosa fare |
|---|---|
| `command not found: fauser` | Non è nel PATH: rivedi il passo 3, poi apri un terminale nuovo |
| «non sono dentro il repository delle consegne» | `cd ~/scuola/fauser-<username>` |
| «scegli prima il linguaggio» | `fauser lingua <linguaggio>` |
| «manca la materia» | aggiungi `--materia tpsit` (o `informatica`, `gpoi`) |
| «la cartella esiste già» | la consegna c'è già: entraci e continua |
| non riesce a collegarsi | controlla la rete; se la scuola blocca il sito, avvisa il docente |
| toolchain mancante (uscita 2) | installa il compilatore del tuo linguaggio e controlla con `--version` |
| `README.md` mancante o vuoto | scrivilo, con la sezione «Uso dell'AI» |
| il push è rifiutato | `git pull --rebase`, poi rilancia `fauser consegna` |
| un caso troppo lento | `fauser test --timeout 20s`, ma di solito c'è un ciclo infinito o una lettura che aspetta |

Per segnalare un problema scrivi al docente (o su Discord) la **versione** (`fauser versione`), il **comando** e l'**errore copiato**. Mai token o password.

---

## Privacy e sicurezza

- `fauser` non raccoglie dati e non manda niente fuori dal tuo computer, a parte il download delle tracce dal repository pubblico [`tracce`](https://github.com/gfauser-26/tracce) e il `git push` verso il **tuo** repository.
- Scarica `fauser` **solo** da questa pagina delle release e controlla sempre `SHA256SUMS`.
- Non mettere mai password, token o dati personali nel codice, nel README o nei commit.
