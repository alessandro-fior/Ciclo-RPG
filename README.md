# Ciclo RPG

Esempio minimale di ciclo di lettura in RPG fixed format su IBM i / AS400.

Il programma legge sequenzialmente il file `MYFILE`, mostra il contenuto del
campo `CAMPO1` con `DSPLY` e termina quando l'operazione `READ` attiva
l'indicatore di fine file `90`.

## File del progetto

- `listato.rpg`: sorgente RPG fixed format con ciclo di lettura su file.

## Codice incluso

```rpg
FMYFILE   IF   E           K DISK

C           READ      MYFILE                  90
C   N90     DOW       *IN90 = *OFF

C           DSPLY     CAMPO1

C           READ      MYFILE                  90
C           ENDDO

C           SETON                     LR
```

## Come funziona

1. La specifica `F` dichiara il file `MYFILE` come file di input su disco.
2. Il primo `READ` tenta di leggere il primo record.
3. L'indicatore `90` viene usato per rilevare la fine del file.
4. Il ciclo `DOW` continua finche `*IN90` resta spento.
5. `DSPLY CAMPO1` mostra il valore del campo letto.
6. A fine programma viene acceso `LR` per chiudere il ciclo di vita del job.

## Prerequisiti

- Ambiente IBM i / AS400.
- File fisico o logico `MYFILE` disponibile in libreria.
- Campo `CAMPO1` definito nel formato record del file.
- Compilatore RPG compatibile con sorgenti fixed format.

## Compilazione

Il comando dipende dalla versione RPG e dalla collocazione del sorgente. Un
esempio tipico, da adattare alla libreria e al membro sorgente usati, e:

```cl
CRTRPGPGM PGM(MIALIB/CICLORPG) SRCFILE(MIALIB/QRPGSRC) SRCMBR(LISTATO)
```

Se il sorgente viene convertito in RPG IV, si puo usare `CRTBNDRPG`:

```cl
CRTBNDRPG PGM(MIALIB/CICLORPG) SRCFILE(MIALIB/QRPGLESRC) SRCMBR(LISTATO)
```

## Note

- `DSPLY` e utile per esempi e debug, ma in programmi applicativi reali e
  preferibile scrivere su log, display file o altra destinazione esplicita.
- Il nome del file `MYFILE` e del campo `CAMPO1` va sostituito con i nomi reali
  presenti nel tuo ambiente.
- Per nuovi sviluppi RPG, valuta RPG IV free format: e piu leggibile e piu
  vicino agli standard moderni del linguaggio.
