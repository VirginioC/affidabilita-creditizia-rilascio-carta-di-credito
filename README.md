# Previsione dell'affidabilità creditizia per il rilascio della carta di credito

## Descrizione del progetto
Questo progetto, realizzato durante il Master in Data Science di ProfessionAI, si propone di sviluppare col linguaggio **Python** e la libreria **scikit-learn**, su ambiente **Google Colab**, un modello di machine learning per prevedere l'affidabilità dei clienti di una banca che richiedono la carta di credito. Il progetto analizza i dati dei richiedenti, fornendo uno strumento utile alle istituzioni finanziarie per ridurre i rischi di insolvenza e migliorare l'efficienza del processo decisionale.

### Politica aziendale
Si ipotizza che la banca abbia necessità di adottare una **politica di assunzione del rischio** e pertanto l'obiettivo è quello di massimizzare la recall dei clienti ritenuti affidabili. In questo modo si riducono al minimo i clienti affidabili classificati come inaffidabili senza perdere potenziali clienti validi, anche a costo di una precisione minore.

### Dataset
Si hanno a disposizione due dataset, contenuti all'interno del file zip `credit_card_approval.zip` presente nel repository:

1. **`application_record.csv`**: contiene informazioni personali e finanziarie dei richiedenti come ID, età, stato civile e reddito (438557 samples).
2. **`credit_record.csv`**: include i dati sullo storico dei pagamenti dei richiedenti (1048575 samples):
   
   - `ID`: codice identificativo univoco del cliente.
   - `MONTHS_BALANCE`: il mese dei dati estratti è il punto di partenza ossia 0 è il mese corrente, -1 è il mese precedente e così via.
   - `STATUS`: **0**: scaduto da 1-29 giorni; **1**: scaduto da 30-59 giorni; **2**: scaduto da 60-89 giorni; **3**: scaduto da 90-119 giorni; **4**: scaduto da 120-149 giorni; **5**: debiti scaduti o inesigibili, più di 150 giorni; **C**: saldati quel mese ed **X**: nessun prestito per il mese.

## Struttura del progetto
Il progetto è organizzato nelle seguenti fasi:

1. **Analisi esplorativa**:
   - Studio delle variabili presenti nei file `application_record.csv` e `credit_record.csv`.
  
2. **Creazione della variabile target RELIABILITY**:
   - Si combinano in un'unica variabile le features `MONTHS_BALANCE` e `STATUS` creando il target **`RELIABILITY`**: indicatore di affidabilità suddiviso in 3 classi (**classificazione multiclasse**):
      - **Classe I (Indefinite Reliability)**: il cliente non ha mai richiesto un prestito (*STATUS* assume soltanto la modalità X).
      - **Classe R (Reliable Customer)**: il cliente è affidabile (modalità più frequente di *STATUS*: C).
      - **Classe U (Unreliable Customer)**: il cliente è inaffidabile (modalità più frequente di *STATUS*: 0, 1, 2, 3, 4 o 5).

   - **Classi sbilanciate**: I (9.18 %) - R (39.24 %) - U (51.58 %).
   
2. **Preprocessing dei dati**:
   - Pulizia e unione dei dati provenienti dai due dataset.
   - Encoding delle variabili categoriche tramite label encoding e one-hot encoding.
  
3. **Analisi descrittiva del comportamento delle features in relazione al target**:
   - Confronto tra le diverse features e la variabile target costruita.

5. **Modellazione e tecniche di bilanciamento delle classi**:
   - Si valutano i seguenti modelli di machine learning per svolgere la classificazione multiclasse applicando quando possibile tecniche di bilanciamento e feature selection:
     
     - **Logistic Regression**: utilizzo del parametro `class_weight="balanced"`
     - **Complement Naive Bayes**
     - **Support Vector Machines**: utilizzo del parametro `class_weight="balanced"`
     - **K-Nearest Neighbors**: utilizzo del parametro `weights="distance"`
     - **Decision Tree**: valutate le prestazioni anche con **feature selection**.
     - **Random Forest**: valutate le prestazioni anche con **feature selection**.
     - **Multilayer Perceptron**: valutate le prestazioni utilizzando anche la tecnica `SMOTE`.

6. **Addestramento e valutazione**:
   - Addestramento e valutazione tramite **cross-validation** (eventuale standardizzazione o normalizzazione dei dati a seconda del modello).
   - Metriche valutate: **accuracy**, **precision**, **recall**, **AUC** e **confusion matrix** (quest'ultima calcolata solo per il modello "migliore").
   - Priorità alla massimizzazione della **recall della classe R (Reliable Customer)** in base alla politica di assunzione del rischio della banca.
  
7. **Scelta del modello "migliore" e risultati sintetici**:
   - Il modello "migliore" è il **Decision Tree con le feature selezionate**: recall della classe R sul test set pari al 63.56 %.
   - Applicando poi dei **pesi personalizzati** alle classi (`class_weight={"I": 0.1, "R": 1.0, "U": 0.1}`), la **recall della classe R** sul test set raggiunge l'**84 %** individuando un numero elevato di clienti affidabili anche se a discapito di una precision di circa il **50 %** (viene concessa la carta anche a clienti con un profilo di rischio più alto). Viene quindi rispettata la politica aziendale della banca.

## Tecnologie utilizzate
- **Linguaggio**: Python
- **Ambiente di sviluppo**: Google Colab (Jupyter Notebook)
- **Librerie**:
   - `pandas`
   - `numpy`
   - `matplotlib`
   - `seaborn`
   - `scikit-learn`
   - `imblearn`

## Utilizzo  
1. Scarica o clona il repository.
2. Apri il file `affidabilità_creditizia_rilascio_carta_di_credito.ipynb` su Google Colab o altri ambienti compatibili con Jupyter Notebook.
3. Esegui il codice passo-passo per ottenere i risultati.
4. I dataset utilizzati si trovano nel file `credit_card_approval.zip` presente nel repository ed estratto durante l'esecuzione del notebook.

## Autore
[Virginio Cocciaglia](https://github.com/VirginioC)

---
