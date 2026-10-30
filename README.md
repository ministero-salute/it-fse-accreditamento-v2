# Accreditamento FSE 2.0
Questo repository raccoglie i risultati dei test effettuati per l'accreditamento dei software con il sistema FSE 2.0 a sostituzione del precedente it-fse-accreditamento, che è stato dismesso.

- [Accreditamento FSE 2.0](#accreditamento-fse-20)
	- [Procedura di caricamento dei risultati](#procedura-di-caricamento-dei-risultati)
		- [Struttura dei risultati (data.json)](#struttura-dei-risultati-datajson)
		- [File riassuntivo (report-checklist.xlsx)](#file-riassuntivo-report-checklistxlsx)
		- [Pull request](#pull-request)
	- [Gestione equivalenza versione software](#gestione-equivalenza-versione-software)
		- [Procedura di caricamento delle informazioni di equivalenza applicativa](#procedura-di-caricamento-delle-informazioni-di-equivalenza-applicativa)
		- [Struttura file json di equivalenza](#struttura-file-json-di-equivalenza)
	- [Nuova tipologia documentale per applicativo già presente nella Lista dei software convalidati](#nuova-tipologia-documentale-per-applicativo-già-presente-nella-lista-dei-software-convalidati)

## Invocazione del servizio di validazione del Gateway ai fini dell'accreditamento
Per lo svolgimento dei casi di test previsti dall'accreditamento, è necessario invocare il servizio di validazione del Gateway (le cui specifiche tecniche sono riportate al link [integrazione-gateway](https://github.com/ministero-salute/it-fse-support/blob/main/doc/integrazione-gateway/README.md#31-request)), valorizzando il campo "activity" come di seguito indicato:

  "activity": "VALIDATION_FOR_ACCREDITAMENTO"



## Procedura di caricamento dei risultati
Per inviare i risultati dei test effettuati è necessario eseguire una pull request. 

 
La struttura del repository è la seguente:
    
        GATEWAY/{commonName auth}/{subject_application_vendor}/{subject_application_id}/{subject_application_version}

per ogni software quindi, bisognerà creare la rispettiva *directory di sottomissione* rispettando lo schema.

---

L'insieme dei caratteri che compongono il path della directory è `[a-zA-Z0-9.-_]`[^1] gli *spazi* vanno trasformati in *underscore* `' ' -> _` eventuali altri caratteri non compresi nell'insieme saranno *omessi*.

---

Ad esempio:  
`GATEWAY/A1#111FORNITORE1/FOO_SPA/BARMED/V.4.2.0`

I dati da inviare comprendono:
* file `data.json` contenenti i riferimenti di tracciatura dei risultati dei test
* file riassuntivo (foglio di calcolo) `report-checklist.xlsx` di tutti i test case eseguiti

**ATTENZIONE**: 
1. I file pdf usati nelle invocazioni dei servizi NON devono essere inviati.
2. Per l’invocazione dei servizi devono essere utilizzati esclusivamente i codici fiscali inclusi nella [whitelist](https://github.com/ministero-salute/it-fse-accreditamento-v2/blob/main/Test%20Case/CF%20WHITELIST.txt) fornita. L'utilizzo di cf 		diversi da quelli in white list comporta un KO durante il processo di accreditamento. 
3. Sul presente repository NON devono essere caricati dati sensibili. I software vendor non impiegano in nessun modo dati riconducibili a persone identificate o identificabili, ai sensi del Regolamento (UE) 2016/679 (GDPR). L’apertura di una pull request è subordinata alla presa visione dell'informativa privacy e all’accettazione della dichiarazione di presa di responsabilità da allegare all'EUSurvey.


I risultati delle verifiche verranno mandate per mail all'indirizzo immesso nel [modulo di richiesta accreditamento](https://ec.europa.eu/eusurvey/runner/FSE-2-validazione-new).

### Struttura dei risultati (data.json)
All'interno della directory bisognerà creare un file `data.json` con il seguente schema:

|ATTENZIONE|
|---|
|Come conseguenza del punto 1 sopra, rimosso il campo `files` dalla struttura dati, non essendo previsto il caricamento dei file (quindi neanche la referenziazione degli stessi nel data.json ).  |


```json
{
	"appVendor": "vendor",
	"appID": "id",
	"appVersion": "version",
	"results": [
		{
			"id": 1,
            "ts":"2022-12-23T11:42:00Z",
			"traceId": "b5e75a93-e517-480b-9eaa-6cb8b81d03d8",
			"workflowInstanceId": "2.16.840.1.113883.2.9.2.120.4.4.97bb3fc5bee3032679f4f07419e04af6375baafa17024527a98ede920c6812ed.7ea73d015c^^^^urn:ihe:iti:xdw:2013:workflowInstanceId",
		}
	]
}
```

le chiavi contrassegnate da `*` sono obbligatori

|chiave|descrizione|
|---|---|
|*appVendor|subject_application_vendor|
|*appID|subject_application_id|
|*appVersion|subject_application_version|
|*results|vedi [Results](#results)|

**Results** <a id="results"></a>

contiene la lista dei risultati, uno per ogni caso testato
|chiave|descrizione|
|---|---|
|*id|identificativo del test case nel piano di test|
|*ts|timestamp del test in formato ISO_8601 semplificato `YYYY-MM-DDThh:mm:ssZ` |
|*traceId|traceId restituito dal sistema|
|workflowInstanceId|workflowInstanceId restituito dal sistema|



### File riassuntivo (report-checklist.xlsx)

Il file riassuntivo deve essere composto a partire dalla [check list](Test%20Case) che individua tutti i possibili test case.


Il file modificato dovrà contenere solo i test individuati da *tipo documento* e *servizio* oggetto dell'accreditamento.

Il file `report-checklist.xlsx` andrà caricato nella *directory di sottomissione*.

Si riporta di seguito il changelog delle modifiche introdotte sul file report-checklist in ottemperanza a quanto espresso dal Decreto settembre 2023: 

| Versione checklist| Data rilascio      | CDA adeguato                                          | Test case obbligatorio da decreto                           |
|-------------------|------------------- |-------------------------------------------------------|-------------------------------------------------------------|
| 8.2.0		    | 12/06/2024         | PSS                                                   |		444		                               |
| 8.2.1		    | 19/07/2024         | RAP, RAD                                              |         417, 446                                            |
| 8.2.2             | 04/12/2024         | RAP                                                   |         417, 418, 460                                       |
| 8.2.3             | 14/02/2025  	 | RSA, LDO, LAB, VPS, CERT_VAC, SING_VAC                | 448, 450, 452, 454, 456, 457, 458                           |
| 8.2.4             | 05/03/2025  	 | RSA, LDO, LAB, VPS, CERT_VAC, SING_VAC, RAD, PSS, RAP | 448, 450, 452, 454, 456, 457, 458                           |
| 8.2.5             | 28/03/2025  	 | RSA, LDO, LAB, VPS, CERT_VAC, SING_VAC, RAD, PSS, RAP | 448, 450, 452, 454, 456, 457, 458, 471, 444, 417, 418, 460  |
| 8.2.6             | 14/05/2025  	 | RSA, LAB, RAD 					 | 448, 450, 452, 454, 456, 457, 458, 471, 444, 417, 418, 460  |
| 8.2.7             | 04/12/2025  	 | PSS 					 | 476  |
| 8.3.0             | 30/01/2026  	 | TPI, LETT_SCREEN, LETT_VACC, CCE 					 | 479, 491, 506, 521  |
| 9.0.0             | 19/03/2026  	 | TELEV, REL_CTC 					 | 531, 551 |

### Pull request
Per inviare i risultati sarà quindi necessario fare un pull request.  
I passi sono sostanzialmente:
* fork del repository
* creazione nel repository forkato della *directory di sottomissione*
* creazione del file `data.json`
* apertura della pull request

La descrizione della pull request dovrà contenere i dati che compongono la *directory di sottomissione*:
* `commonName auth`
* `subject_application_vendor`
* `subject_application_id`
* `subject_application_version`


## Gestione equivalenza versione software 
 
Il fornitore ha la possibilità di autodichiarare l’equivalenza tra una versione di software **già validata** ed una sua release successiva **se questa non ha subito modifiche al codice nei moduli relativi alla gestione dei CDA2 e al colloquio col gateway**.


L’equivalenza è pertanto da intendersi come:
* stesse tipologie documentali trattate
* stessi servizi invocati
* stessa gestione degli errori e casi di applicabilità


Dati questi presupposti le versioni del software contenute nell’autodichiarazione **non è necessario che siano sottoposte al processo di convalida**.

 
### Procedura di caricamento delle informazioni di equivalenza applicativa

Per dichiarare l’equivalenza tra le versioni di un applicativo è necessario eseguire una pull request che riporti come  titolo **”Dichiarazione di versioni equivalenti”**, e contenente il solo file json `versions.json` con le informazioni di equivalenza, la cui struttura è di seguito riportata.

Per eseguire la pull request, seguire le indicazioni sopra riportate.

 
### Struttura file json di equivalenza

All’interno della *directory di sottomissione* creare un file `versions.json` con il seguente schema:

```json
{
    	"appVendor": "vendor",
    	"appID": "id",
    	"appVersion": "version3.0",
    	"ts":"2023-06-08T15:30:29Z",
		"equiv_releases": [
			"version3.1",
			"version3.2",
			"version3.2.1"
		]
  }
```

Di seguito la descrizione delle chiavi del file `versions.json`:

|chiave|descrizione|
|---|---|
|**appVendor**|`subject_application_vendor`|
|**appID**|`subject_application_id`|
|**appVersion**|`subject_application_version` dell'applicativo validato|
|**ts**|timestamp di produzione del file versions.json in formato ISO_8601 semplificato `YYYY-MM-DDThh:mm:ssZ` |
|**equiv_releases**|array di versioni equivalenti a quella validata|


## Nuova tipologia documentale per applicativo già presente nella Lista dei software convalidati

Nel caso un applicativo abbia superato la Fase 1 per una particolare tipologia documentale (per es. LDO) e il fornitore sottometta una nuova richiesta di accreditamento per un’altra tipologia documentale (per es. RSA), l’applicativo ripeterà l’intera Fase 1 per le due tipologie documentali.


I fornitori dovranno sottomettere una nuova pull request contenente tutti i casi di test (e relativo data.json) eseguiti per le due tipologie documentali ( LDO+RSA ).


All’interno della Lista dei software convalidati la soluzione applicativa sarà riportata due volte: nella prima riga si riporterà l’esito relativo al LDO, nella seconda l’esito relativo a LDO, RSA.


Ai fini del conteggio dei documenti trasmessi dalle Regioni/PA si terrà presente la data di conclusione della Fase 1 di ciascuna tipologia documentale.




[^1]: attenzione, questa non è la regular expression corretta, è necessario fare escape di `.`:`[a-zA-Z0-9\.-_]` 


