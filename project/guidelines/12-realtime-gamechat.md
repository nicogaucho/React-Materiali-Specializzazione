# Realtime Chat

Da questo momento consetiremo agli utenti autenticati di poter avere, per ogni gioco, una live chat forum per messaggiare in tempo reale con altri utenti autenticati e visitanti la piattaforma.

## Table public.messages

Come primo passo avremo bisogno di una tabella nel database per salvare la lista dei messaggi lasciati dai nostri utenti.

Avremo bisogno di generere la tabella public.messages come abbiamo già fatto con public.profiles.

Assicuriamoci di proteggere sempre la tabella associata agli utenti abilitando Row Level Security (RLS) e facendo riferimento alla tabella auth.users per garantire l'integrità dei dati. Useremo on delete cascade per mantenere le relazioni coerenti.

Per ottenere tutto questo, useremo questa volta l'interfaccia grafica di supabase, nella sezione ```Table Editor``` usiamo il tasto ```create table```:

![An image](../../assets/create-messages-table.png)

Abilitiamo (RLS) e Realtime.

### messages columns

Per le colonne della tabella messages sceglieremo le seguenti opzioni: 

![An image](../../assets/columns-messages.png)

- id: uuid not null
- updated_at: timestamps now() not null
- content: text not null
- profile_id: uuid auth.uid() not null
- profile_username: text
- game_id: int8 not null

Dopo aggiungeremo il vincolo referenziale con la tabella profiles.

![An image](../../assets/foreignkey.png)

dopo averlo definito clicchiamo su ```save```.

Il risultato sarà il seguente:

![An image](../../assets/messages-table.png)

una nuova tabella messages, con i campi **id**, primary key, **updated_at**, **content**, **profile_id** foreign key, **profile_username**, **game_id**.

## messages authorization

Per autorizzare solo gli utenti autenticati l'inserimento di un messaggio, creeremo 2 policy molto semplici sulla nostra tabella messages

### Select policy (RLS)

In lettura consentiremo a tutti gli utenti di leggere i messaggi, creando ```Enable read access for all users```

![An image](../../assets/select-policy-messages.png)

### Insert policy (RLS)

In inserimento solo gli utenti autenticati potranno inserire un messaggio, creando
```Enable insert for authenticated users only```

![An image](../../assets/insert-policy-messages.png)

### messages policies

Dopo aver salvato le policies:

![An image](../../assets/messages-policy.png)

## Chat component

