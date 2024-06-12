# Stable Diffusion guide to model and generation

![00010-274069434.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00010-274069434.png)

# Introduzione

Guida per l’utilizzo di stable diffusion per gli studenti di NTA.

# cos’è stable diffusion ?

**STABLE-DIFFUSION** è un **modello di diffusione:**

ovvero un tipo di modello generativo , ovvero 

Un modello generativo un tipo di modello di intelligenza artificiale che può creare nuovi dati simili a quelli su cui è stato addestrato.

**Stable diffusion** trasforma un immagine rumorosa in un immagine chiara gradualmente , applicando delle trasformazioni e dei campionamenti ad ogni immagine.

funziona a **steps** ovvero a passaggi, ogni passaggio migliora l’immagine in base ai risultati prodotti dallo steps precedente, riducendo così il rumore nell’immagine.

ciò che accade si divide in 2 processi:

1. **forward process:** 
    1. Aggiunge rumore all’immagine in più passaggi , parte da un immagine chiara fino ad ottenere un immagine completamente rumorosa.
2. **Reverse process:**
    1. Inverte il forward process , partendo dall’immagine rumorosa e rimuovendo gradualmente il rumore per ottenere l’immagine finale.

in breve accade questo: si parte da un immagine pulita Xo

ad ogni passaggio t (tempo) viene aggiunto un pò di rumore all’immagine seguendo un processo gaussiano in cui l’immagine ad ogni passo è un pò più rumorosa dell’altra.

in fine , dopo T passaggi , l’immagine e completamente rumorosa. 

dopo di che si parte da un immagine completamente rumorosa e si rimuove ad ogni passaggio T dove t=T,t=T-1, …, 1 . viene rimosso un po di rumore per riprendere l’informazione originale dell’immagine

in fine dopo il 1 passaggio si ottiene un immagine Xo che dovrebbe essere simile all’immagine iniziale ovvero quelle del dataset su cui ci basiamo , oppure con le caratteristiche richieste.

in modelli come **stable diffusion abbiamo la necessità di avere dei DATASET** ovvero grandi quantità di immagini su cui basare le nostre operazioni.

nel caso possiamo anche creare da soli il nostro dataset , utilizzando immagini reali che in quel caso vanno **etichettate e classificate.**

**il modello deve essere ADDESTRATO .** 

cioe il modello che noi diamo in pasto a stable diffusion viene addestrato sul dataset , ogni immagine del dataset verrà sottoposta al **forward process.**

perchè il forward process? → perchè così il modello potrà prevedere il rumore 

successivamente le immagini che andremo a creare tramite il nostro prompt o altro, verranno sottoposte al processo di **reverse process** partendo dal rumore fino al risultato voluto.

**note:**

l’hardware e fondamentale per sviluppare immagini dato che necessita di una grande potenza di calcolo. 

una scheda vide nvidia e consigliata ma è stata trovata una soluzione anche per chi ha AMD.

anche senza un hardware potente o ottimizzato potete destreggiarvi cambiando la grandezza delle immagini e evitando di usare algoritmi pesanti.

senza scendere troppo nel dettaglio andiamo a installare stable diffusion:

# first step: download and install stable-diffusion

prima di scaricare stable diffusion è necessario accertarsi di avere python , versione 3.6.10 installato nel proprio pc 

<aside>
⚠️ *guida redatta 05/24 determinate informazioni come versione di python o git possono variare nel tempo essendo stable diffusion aggiornato da una community molto attiva IN OGNI CASO CONSULTARE LA GUIDA DI STABLE DIFFUSION SU GITHUB:* https://github.com/AUTOMATIC1111/stable-diffusion-webui

</aside>

**strumenti necessari:** 

1. Git → [https://git-scm.com/downloads](https://git-scm.com/downloads)
2. Python 3.6.10→ [https://www.python.org/downloads/release/python-3610/](https://www.python.org/downloads/release/python-3610/)

1. crea una cartella per il tuo progetto : 
    1. clicca il tasto home di windows.
    2. digita **C:\** e premi invio 
    3. Spostati nel campo **Users** (utente )
    4. vai nel tuo utente 
    5. nell’Url della cartella digita **cmd**
    
    ![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled.png)
    
    nel terminale:
    
    1. creiamo un ambiente virtuale:
    
    ```python
    py -m venv **EnvStableDiffusion #**(o il nome che preferite)
    ```
    
    1. attiviamo l’ambiente virtuale:
    
    ```python
    ./Project/EnvStableDiffusion/Scripts/activate
    
    in windows : .\Project\EnvStableDiffusion\Scripts\activate
    ```
    

**(mi raccomando il punto prima dello slash altrimenti non esegue)**

a questo punto una volta attivato l’ambiente controlliamo che ci siano installati git ,python, e pip

```python
git #controlliamo se è presente , se ritorna un warning va installato
py #spunterà un prompt di testo o un errore se non presente cliccare CTRL+Z per uscire
pip --version #controlliamo ci sia pip
```

a questo punto cloniamo il repository con git clone:

```python
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

una volta completato il download di tutte le dipendenze , comparirà nella cartella del progetto (fuori dalla cartella dell’ambiente virtuale) , una cartella nominata **stable-diffusion-webui.git**

entriamo nella cartella **troviamo il file BATCH webui-user CLICCARE TASTO DESTRO , MODIFICA (anche nel blocco note)**

**dopo di che** 

nel campo aggiungiamo : 

COMMANDLINE_ARGS=  --xformers --autolaunch  --autoupdate

**ADESSO CI BASTERA’ CLICCARE SUL FILE WEBUI-USER (FILE BATCH DI WINDOWS)** 

per avviare il tutto.

# Install on AMD gpu

come si installa stable diffusion?

stable diffusion necessita di una GPU invidia essendo creato basandosi su Cuda tool kit , ma è stata trovata una soluzione. 

come fare :

innanzitutto , creare una cartella es **Project .**

dopo di che aprire il terminale all’interno di quella cartella:

piccolo trucco (windows) → nell’url della cartella digitate **cmd**  verrà automaticamente aperta la finestra del prompt dei comandi nel percorso della cartella.

innanzi tutto avendo già python installato nel vostro sistema si può creare un ambiente virtuale.

digitate : 

```python
**sintassi:** py -m venv **EnvStableDiffusion name_project (non copiare)**

```

```python
**#nel mio esempio:**
py -m venv **EnvStableDiffusion #**(o il nome che preferite)
```

una volta fatto questo avrete creato un ambiente virtuale .

ATTIVIAMOLO → nel mio esempio:

```python
/Project/EnvStableDiffusion/Scripts/activate
```

![sulla sinistra avrete il nome del vostro ambiente.](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%201.png)

sulla sinistra avrete il nome del vostro ambiente.

sempre nel vostro ambiente virtuale controllate che :

1. sia presente **git → basterà digitare git e premere invio**
2. sia presente python 3.6.10 **→ basterà digitare py e premere invio (per uscire CTRL+Z)**
3. sia presente pip →stessa procedura (se pip non è presente 

```python
python -m pip install --upgrade pip
```

a questo punto una volta che c’è python,pip e git , dovremo effettuare due download il primo è 

**pytorch attraverso pip.**

```python
python -m pip install torch==2.0.0 torchvision==0.15.1 torch-directml

```

una volta installato pytorch facciamo un clone da git della repository.

```python
git clone https://github.com/lshqqytiger/stable-diffusion-webui-amdgpu.git

```

**attendiamo il download.**

una volta terminato il download , dovremo fare una piccola modifica.

andiamo dentro la cartella stable-diffusion-webui

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/0ea48a93-f84d-4adf-b5bb-b954eab26722.png)

**andiamo sul file .bat , tasto destro e clicchiamo modifica** 

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%202.png)

potete sovrascrivere il file così:

@echo off

set PYTHON=
set GIT=
set VENV_DIR=
set COMMANDLINE_ARGS= --use-directml --opt-sub-quad-attention --no-half --disable-nan-check --autolaunch

call webui.bat

**NON CI RESTA CHE FARE DOPPIO CLICK SUL FILE BATCH E SI APRIRA’ STABLE DIFFUSION.**

# Analisi Schermata Home Stable-Diffusion:

avviato stable diffusion ci verrà presentata la seguente schermata:

![schermata iniziale ](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%203.png)

schermata iniziale 

analizziamo la schermata in modo dettagliato :

- checkpoint: il "checkpoint" è un file che contiene il modello addestrato. Caricare un checkpoint permette al sistema di utilizzare quel modello specifico per generare immagini.
- prompt:Il "prompt" è il testo che descrive l'immagine da generare. Inserendo qui una descrizione dettagliata guideremo il modello nella creazione dell'immagine.
- negative prompt: Il "negative prompt" è il testo che descrive cosa evitare nell'immagine generata. Puoi specificare elementi o caratteristiche che non vuoi siano presenti nell'immagine.
- generation:La "generation" è il processo di creazione dell'immagine basato sul prompt e sugli altri parametri impostati.
- Hires.fix e schedule type :
    - **Hires.fix**: migliora la qualità dell'immagine finale.  usata per ottenere immagini ad alta risoluzione partendo da una generazione a bassa risoluzione.
    - **Schedule Type**: Influisce sul modo in cui il modello perfeziona l'immagine.
- Sampling steps: indicano il numero di passaggi che il modello esegue per generare l'immagine. Più passaggi generalmente portano a una qualità migliore, ma richiedono più tempo.
- **CFG scale: La "CFG scale" (Classifier-Free Guidance scale) controlla quanto strettamente il modello deve seguire il prompt. Un valore più alto significa che l'immagine sarà più aderente alla descrizione del prompt, mentre un valore più basso permette più libertà creativa al modello.**

SAMPLING METHOD:

come funzionano ? 

in breve questi sono **metodi di campionamento.** ogni metodo migliora l’immagine in modi diversi , in base alle formule matematiche che applica ad ogni layer di ogni immagine del nostro dataset. 

si parte da un immagine “rumorosa” per ottenere poi un immagine dettagliata

la scelta di quale usare dipende da cosa vogliamo **velocità o dettagli?** 

la scelta delle due cose dipende dal tempo che abbiamo e inoltre dal tipo di hardware a nostra disposizione , tenete presente che è importante non esagerare , non sempre un metodo che magari è il migliore  nel  dettaglio è il più adatto ai vostri scopi.

1. DDIM: denoising diffusion , in breve e un metodo che usa meno passaggi per generare le immagini , **pro : dato il numero ridotto di passaggi si ottengono buoni risultati velocemente**, **contro:** risultati scarsi con dataset di poche immagini o di media qualità. 
2. DDIM: denoising diffusion , in breve e un metodo che usa meno passaggi per generare le immagini , **pro : dato il numero ridotto di passaggi si ottengono buoni risultati velocemente**, **contro:** risultati scarsi con dataset di poche immagini o di media qualità. 
3. PLMS: **Pseudo linear multistep ,** campiona linearmente l’immagine e ne migliora la qualità , **produce un immagine dettagliate.**
4. EULER: Campionamento Euleriano sull’immagine, e quello più usato. Ad ogni passo t, l'algoritmo riduce il rumore nell'immagine basandosi su una stima della "derivata" (il cambiamento necessario per ridurre il rumore).
5. EULER A: variante adattiva di Euler , ottimizza la qualità delle immagini .
6. LMS: laplacian pyramid sampling: usa una piramide di immagini a diverse risoluzioni **migliora i dettagli fini e la nitidezza.**
7. HEUN: usa una previsione corretta del campionamento. **bilancia tra accuratezza e stabilità le immagini negerati** 

**per immagini rapide→DDIM**

**per alta qualità→PLMS O LMS**

**VIA DI MEZZO : EULER O EULER A.**

# Prompt e negative Prompt:

fondamentali per controllare il contenuto e le caratteristiche delle immagini generate.

**prompt:** descrizione testuale che si deve fornire al modello per indicare cosa si desidera generare.

serve a guidare il modello guidando la creazione dell’immagine.

**negative prompt:** specifica gli elementi che non vogliamo nella nostra immagine.Funziona come una lista di esclusione, aiutando il modello a capire cosa non deve includere.

**Esempi**:

- "Senza persone."
- "Evitare colori scuri."
- "Senza alberi."

## Come funziona

fornendo un prompt e un negative al modello entrambi vengono usati per guidare il processo di generazione . 

il modello combina le indicazioni del prompt con le restrizione del negative prompt per generare un immagine che cerca di rispettare il piu possibile entrambe le soluzioni.

# Dove prendere i modelli:

civita ai è una community di appassionati che vi permette di avere modelli ad alta risoluzione gratuitamente.

ecco il link:

[https://civitai.com/search/models?sortBy=models_v9](https://civitai.com/search/models?sortBy=models_v9)

una volta scaricato il modello non resterà che andare nella cartella di stable diffusion:

entrare nella cartella models , ed accedere successivamente alla cartella stable-diffusion

**Path generico: Project\stable-diffusion-webui\models\Stable-diffusion**

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%204.png)

# Estensioni :

le estensioni sono componenti aggiuntivi di stable diffusion che ci permettono di effettuare diverse operazioni a seconda del tipo di estensione.

installato stable diffusion avremo diversi tipi di estensioni di default già caricare 

come 

- txt2img
- img2img
- PNG info

ma **possiamo aggiungerne ancora in base alle nostre necessità.**

# Installare un estensione:

andiamo nel campo extensions in alto a dx , e andiamo su **available.**

clicchiamo il pulsante **load from:**

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%205.png)

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%206.png)

ci apparirà una lista di estensioni che possiamo installare comodamente cliccando su install.

il sistema di valutazione a **stars**  vi consente di valutare secondo l’esperienza di altri utilizzatori ,quale è la migliore.

ogni estensione ha un suo scopo , quindi **è consigliabile leggere la sua documentazione .**

clicchiamo sul nome dell’estensione in grassetto:

ad esempio **Replacer.**

si aprirà la documentazione su github per la nostra estensione con la spiegazione delle operazioni da svolgere per il funzionamento .

[Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled.mp4)

Replacer è un'estensione per [AUTOMATIC1111/stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui). L'obiettivo di questa estensione è quello di automatizzare il mascheramento degli oggetti tramite prompt di rilevamento, utilizzando [sd-webui-segment-anything](https://github.com/continue-revolution/sd-webui-segment-anything) e img2img inpainting in un'unica scheda facile da usare. È utile anche per l'inpaint in batch e l'inpaint nei video con AnimateDiff

**DOPO L’INSTALLAZIONE ANDARE IN ISTALLED E CLICCARE APPLY AND RESTART UI.**

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%207.png)

# Generiamo la nostra immagine  con Txt2Img

andremo a scrivere nel prompt ciò che vogliamo generare, le parentesi non sono obbligatorie ma forzano il modello a soffermarsi su quelle caratteristiche.

**prompt :** 

(ginger girl:1.2)(detailed eyes,detailed hair),(red hair),(dark sky),(sunrise:1.2),(falling leaves,petals,flowers:1.2),(dynamic ,action packed, fantastic realism)

**negative promp:**  (worst quality,low quality,letterboard,mutated hands and fingers),(static scenes),deformed arms,deformed legs,deformed eyes, NSFW

quindi dal prompt stiamo descrivendo il nostro soggetto 

vogliamo una scena vivida e dinamica con una ragazza dai capelli rossi come protagonista. I suoi capelli sono di un rosso brillante e i suoi occhi e capelli sono incredibilmente dettagliati.

Il cielo sullo sfondo è scuro, probabilmente suggerendo un'atmosfera notturna o al crepuscolo. 

C'è un tocco di alba all'orizzonte, con luce arancione o rosa, creando un contrasto.

Intorno alla ragazza, foglie, petali e fiori cadono delicatamente, aggiungendo un senso di movimento. L'intera immagine è dinamica, con un realismo fantastico.

![00003-3370817518.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00003-3370817518.png)

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%208.png)

**se volete ricrearla identica vi basterà aggiungere le impostazioni che vedete sotto l’immagine.**

**importante il seed quando volete ricreare la stessa immagine da un dataset.**

nel checkpoint andremo a mettere il nostro dataset di dati.

mentre nel prompt e negative prompt descriviamo rispettivamente ciò che desideriamo creare e non.

## faciamo una piccola modifica :

### adesso attiviamo hires.fix e refiner

come accennato prima hires e refiner servono per migliorare l’immagine, ma rallentano il processo di generazione , e di molto, ma **utilizzando un algoritmo più veloce otteniamo uno splendido risultato in tempi molto brevi.**

![00004-346813675.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00004-346813675.png)

unprompted : si carica il video una volta aggiunta l’estensione , si clicca il tasto divide by frame , poi si deve copiare il link , nella cartella dello scipt all’interno dell’estensione si avranno le immagini divide frame by frame .

![attiviamo Hires e refiner.](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%209.png)

attiviamo Hires e refiner.

un alternativa , è usare DDIM , ci velocizzerà ancora di più il processo.

![00005-1825032838.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00005-1825032838.png)

# Txt2Img animate diff

- stable diffusion dependency
    - Extension used :
        - Animate diff
            - animation: [https://huggingface.co/guoyww/animatediff/tree/main](https://huggingface.co/guoyww/animatediff/tree/main)
            
             vanno nella cartella
            
            Project\stable-diffusion-webui\extensions\sd-webui
            
            animatediff\model
            
            - Stabilization: [https://huggingface.co/manshoety/AD_Stabilized_Motion/tree/main](https://huggingface.co/manshoety/AD_Stabilized_Motion/tree/main)
            - Example dataset :[https://civitai.com/models/30240/toonyou](https://civitai.com/models/30240/toonyou)
    - PROMPT:
    
    (masterpiece, good quality), girl, (curly hair,deatiled hari,cure,perfect face), short hair, cafe, barista, (petals,flower),dark skin,(fantastic realistic,dinamic,action-packed), happy, looking at viewer, hairband, waist apron, earrings, upper body),static image,static motion
    
    - NEGATIVE PROMPT:
        - (worst quality, low quality, letterboxed),NSFW
        - (worst quality,low quality,letterboard,mutated hands and fingers),(static scenes),deformed arms,deformed legs,deformed eyes, NSFW

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2010.png)

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2011.png)

ECCO IL RISULTATO FINALE:

![00006-2302930429.gif](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00006-2302930429.gif)

- Civita Ai → free model website
- Huggin face → model and script (stable diffusion)
    - used for Animate diff → [https://huggingface.co/guoyww/animatediff/tree/main](https://huggingface.co/guoyww/animatediff/tree/main)
        - version used : mm_sd_v14
        - download it :[https://huggingface.co/manshoety/AD_Stabilized_Motion/blob/main/mm-Stabilized_high.pth](https://huggingface.co/manshoety/AD_Stabilized_Motion/blob/main/mm-Stabilized_high.pth)
    - models → [https://civitai.com/models/30240/toonyou](https://civitai.com/models/30240/toonyou)

un altro esempio:

![00000-593408310.gif](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00000-593408310.gif)

per evitare il cambiamento di ambiente o di dettaglio del personaggio , basterà nel prompt specificare gli elementi che vogliate siano statici. esempio **static background**

![00001-3932027634.gif](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00001-3932027634.gif)

**prompt :**

masterpiece, good quality), girl, (curly hair,deatiled hari,cure,perfect face), short hair, cafe, barista, (petals,flower),dark skin,(fantastic realistic,dinamic,action-packed), happy, looking at viewer, hairband, waist apron, earrings, upper body),static image,static motion,open eyes,static background

 **negative prompt:**

(worst quality,low quality,letterboard,mutated hands and fingers),(static scenes),deformed arms,deformed legs,deformed eyes, NSFW

![aumento sei sampling da 20 a 40 ed il CFG scale a 8.](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2012.png)

aumento sei sampling da 20 a 40 ed il CFG scale a 8.

# Controlnet stable-diff

ControlNet Stable Diff UI è un'interfaccia utente progettata per semplificare l'utilizzo del modello di generazione di immagini Stable Diffusion.

sono veramente tante le possibili creazioni con controlNet, **face swap, illusioni, loghi, creazione di qr stilizzati** e tanto altro.

# ReActor faceswap:

ReActor e un estensione per il face swapping semplice da usare e molto versatile.

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2013.png)

dataset : [https://civitai.com/models/133621/lah-cyberpunk-or-sdxl](https://civitai.com/models/133621/lah-cyberpunk-or-sdxl)

1man, cable, cyberpunk, cyberpunk style, cyborg, facial hair, hood, jacket, looking at viewer, male focus, neon lights, open clothes, realistic, science fiction, solo, upper body,beard,glasses,
lora:sdxl_cyberpunk:0.65, open eyes,perfect eyes, no hair

(worst quality:1.5), (low quality:1.5), (normal quality:1.5), lowres, bad anatomy, bad hands, multiple eyebrow, (cropped), extra limb, missing limbs, deformed hands, long neck, long body, (bad hands), signature, username, artist name, conjoined fingers, deformed fingers, ugly eyes, imperfect eyes, skewed eyes, unnatural face, unnatural body, error, painting by bad-artist layman work, worst quality, ugly, (deformed|distorted|disfigured:1.21), poorly drawn, bad anatomy, wrong anatomy, mutation, mutated, (mutated hands AND fingers:1.21), bad hands, bad fingers, loss of a limb, extra limb, missing limb, floating limbs, amputation, Yaeba, photo, deformed, black and white, realism, disfigured, low contrast, long neck,nsfw,deformed eyes,hair

![00014-2906116358.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00014-2906116358.png)

![00010-2906116358.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00010-2906116358.png)

![00046-4278119061.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00046-4278119061.png)

![00049-2245923390.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00049-2245923390.png)

# Illusione con logo -Stable diffusion

si possono anche creare illusioni utilizzando Controlnet.

> **prompt**:  beautiful landscape of a paradise island with mountains:1.2), cinematic film still, shallow depth of field, vignette, higth resolution, epic,gorgeus,film grain.
> 
> 
> **Negative prompt**: (cinematic), anime,cartoon,graphic,text,painting,crayion,graphite,abstractmglitch
> 

Impostazioni:

Steps: 25, Sampler: DPM++ 2M, Schedule type: Karras, CFG scale: 7, Seed: 1535177732, Size: 512x512, Model hash: ec6a1ba636, Model: epicrealism_v10-inpainting, Denoising strength: 0.7, Conditional mask weight: 1, ControlNet 0: "Module: invert (from white bg & black line), Model: control_v1p_sd15_qrcode_monster [a6e58995], Weight: 1.0, Resize Mode: Crop and Resize, Processor Res: 0, Threshold A: 0.5, Threshold B: 0.5, Guidance Start: 0.0, Guidance End: 1.0, Pixel Perfect: True, Control Mode: Balanced", Hires upscale: 2, Hires steps: 25, Hires upscaler: R-ESRGAN 4x+, Version: v1.9.3

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2014.png)

![00027-1535177732.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00027-1535177732.png)

batch count : numero di immagini ricreate su cui verrà basata l’immagine finale.

consiglio di metterlo a 3 . maggiore e il valore migliore sarà il risultato ma ci vorrà molto di più in termini di tempo e computazione.

> **prompt:** beautiful landscape of a paradise island with mountains:1.2), cinematic film still, shallow depth of field, vignette, higth resolution, epic,gorgeus,film grain.
> 
> 
> **Negative prompt**: (cinematic), anime,cartoon,graphic,text,painting,crayion,graphite,abstractmglitch
> 

Steps: 25, Sampler: DPM++ 2M, Schedule type: Karras, CFG scale: 7, Seed: 1535177732, Size: 512x512, Model hash: ec6a1ba636, Model: epicrealism_v10-inpainting, Denoising strength: 0.7, Conditional mask weight: 1, ControlNet 0: "Module: invert (from white bg & black line), Model: control_v1p_sd15_qrcode_monster [a6e58995], Weight: 1.0, Resize Mode: Crop and Resize, Processor Res: 0, Threshold A: 0.5, Threshold B: 0.5, Guidance Start: 0.0, Guidance End: 1.0, Pixel Perfect: True, Control Mode: Balanced", Hires upscale: 2, Hires steps: 25, Hires upscaler: R-ESRGAN 4x+, Version: v1.9.3

**nota: cambiando Upscaler cambia il tipo di “sovrapposizione” del logo all’immagine del dataset**

un altro esempio :

> **Prompt:** (beautiful landscape of a paradise island with mountains:1.2), cinematic film still, shallow depth of field, vignette, higth resolution, epic,gorgeus,film grain.                                            **Negative prompt**: (cinematic), anime,cartoon,graphic,text,painting,crayion,graphite,abstractmglitch
> 

Steps: 25, Sampler: DPM++ 2M, Schedule type: Karras, CFG scale: 3, Seed: 1672913447, Size: 512x512, Model hash: ec6a1ba636, Model: epicrealism_v10-inpainting, Denoising strength: 0.7, Conditional mask weight: 1, ControlNet 0: "Module: invert (from white bg & black line), Model: control_v1p_sd15_qrcode_monster [a6e58995], Weight: 1.0, Resize Mode: Crop and Resize, Processor Res: 0, Threshold A: 0.5, Threshold B: 0.5, Guidance Start: 0.0, Guidance End: 1.0, Pixel Perfect: True, Control Mode: Balanced", Hires upscale: 2, Hires steps: 25, Hires upscaler: SwinIR 4x, Version: v1.9.3

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2015.png)

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2016.png)

![00038-1672913447.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00038-1672913447.png)

l’effetto varia a seconda del dataset utilizzato.

ad esempio usando ToonYoubeta6:

![00041-3727621658.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00041-3727621658.png)

![00040-3727621657.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00040-3727621657.png)

# LOGO CREATE

> **prompt:**Tropical island, plants, land, rock,Ultra realistic , highly detailed                                      **negative prompt**:cartoon, painting, illustration , watercolor, drawing , sketch, low quality, low res, NSFW
> 

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2017.png)

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2018.png)

Ecco il risultato finale

![logonike.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/logonike.png)

**cosa succede aumentando il CGF scale?** 

**aumenta la fedeltà al modello .** 

lasciamo le stesse impostazioni e aumentiamo il CGF a **17**

ed attiviamo anche **hires fix e refined per dare una qualità migliore**

![00004-483810022.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00004-483810022.png)

**con le stesse impostazioni cambiamo il prompt per dare un altro stile al nostro logo.**

# Artistic QR-Code:

**per il qr:** [https://keremerkan.net/qr-code-and-2d-code-generator/](https://keremerkan.net/qr-code-and-2d-code-generator/)

**prompt:** an ancient village ,artstation , 4k

**negative-prompt:**

Steps: 20, Sampler: DPM++ 2M, Schedule type: Karras, CFG scale: 7, Seed: 1186643098, Size: 512x512, Model hash: 4199bcdd14, Model: rev-animated-v1-2-2, Denoising strength: 1, ControlNet 0: "Module: inpaint_global_harmonious, Model: control_v11f1e_sd15_tile [a371b31b], Weight: 1.0, Resize Mode: Crop and Resize, Processor Res: 0, Threshold A: 0.5, Threshold B: 0.5, Guidance Start: 0.45, Guidance End: 0.61, Pixel Perfect: True, Control Mode: Balanced", ControlNet 1: "Module: none, Model: control_v1p_sd15_brightness [5f6aa6ed], Weight: 0.35, Resize Mode: Crop and Resize, Processor Res: 512, Threshold A: 0.5, Threshold B: 0.5, Guidance Start: 0.0, Guidance End: 1.0, Pixel Perfect: True, Control Mode: Balanced", Version: v1.9.3

## MODELLO: rev-animated-v1-2-2.safetensors [4199bcdd14]

controlNet processor:

- [https://huggingface.co/lllyasviel/ControlNet-v1-1/blob/main/control_v11f1e_sd15_tile.pth](https://huggingface.co/lllyasviel/ControlNet-v1-1/blob/main/control_v11f1e_sd15_tile.pth)
- 

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2019.png)

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2020.png)

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2021.png)

![Untitled](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/Untitled%2022.png)

![00005-1186643095.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00005-1186643095.png)

![00006-1186643096.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00006-1186643096.png)

![00007-1186643097.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00007-1186643097.png)

![00008-1186643098.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00008-1186643098.png)

![00009-1186643099.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00009-1186643099.png)

un altro esempio:

prompt:a painting of a city with a river and buildings on the hillside and a sky background with clouds and snow on the ground and a blue sky, a detailed matte painting

negative prompt:(KHFB, AuroraNegative),(Worst Quality, Low Quality:1.4)

![00026-1017503600.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00026-1017503600.png)

![00028-1017503602.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00028-1017503602.png)

![00027-1017503601.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00027-1017503601.png)

![00028-1017503602.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00028-1017503602%201.png)

![00023-1874251051.png](Stable%20Diffusion%20guide%20to%20model%20and%20generation%20411b918b2e0d43a39a2fa5d52265bf47/00023-1874251051.png)