Model AI de Verificare a Calității

Disciplina: Rețele Neuronale
Instituție: POLITEHNICA București – FIIR
Student: [Numele Tau Aici]
Link Repository GitHub: [Link-ul tău]
Data: 03.12.2025

2. Descrierea Setului de Date

2.1 Sursa datelor

Origine: Dataset hibrid compus din date reale (setul public "Casting Product Image Data" - piese turnate industriale) și date sintetice generate prin tehnici de augmentare avansată.

Modul de achiziție: [ ] Senzori reali / [ ] Simulare / [X] Fișier extern (Kaggle) / [X] Generare programatică (Augmentare).

Perioada / condițiile colectării: Datele reale provin dintr-un proces de turnare industrial; datele sintetice au fost generate în etapa curentă pentru a simula condiții de iluminare variabile.

2.2 Caracteristicile dataset-ului

Număr total de observații: 1,300 imagini (733 originale + 567 augmentate pentru balansare).

Număr de caracteristici (features): 90,000 (pixeli per imagine, la rezoluția 300x300).

Tipuri de date: [ ] Numerice / [ ] Categoriale / [ ] Temporale / [X] Imagini (Matrici de pixeli Grayscale).

Format fișiere: [ ] CSV / [ ] TXT / [ ] JSON / [X] PNG / [ ] Altele: [...]

2.3 Descrierea fiecărei caracteristici

În contextul Computer Vision, "caracteristicile" sunt pixelii individuali ai imaginii și eticheta asociată.

Caracteristică

Tip

Unitate

Descriere

Domeniu valori

pixel_intensity

numeric

-

Intensitatea luminoasă a fiecărui punct din imagine (Grayscale).

0 – 255

label_class

categorial

–

Clasificarea piesei: {ok_front (Bun), def_front (Defect)}

{0, 1}

image_width

numeric

px

Lățimea standardizată a imaginii de intrare.

300

image_height

numeric

px

Înălțimea standardizată a imaginii de intrare.

300



3. Analiza Exploratorie a Datelor (EDA) – Sintetic

3.1 Statistici descriptive aplicate

Distribuția pe clase (Original):

Clasa Defect: 60% (înainte de balansare era majoritară).

Clasa OK: 40% (necesită augmentare).

Dimensiuni: Imaginile originale variază ușor, dar majoritatea sunt centrate pe piese de 300x300px.

Intensitate medie: Histogramele pixelilor arată o distribuție bimodală (fundal întunecat vs. piesa metalică strălucitoare).

3.2 Analiza calității datelor

Detectarea valorilor lipsă: 0% (Nu există imagini corupte care nu pot fi deschise).

Detectarea valorilor inconsistente: S-au identificat 5 imagini cu iluminare extrem de slabă (prea întunecate), marcate pentru excludere.

Identificarea caracteristicilor redundante: Fundalul negru al imaginilor nu oferă informație utilă, dar va fi gestionat automat de straturile de convoluție (CNN).

3.3 Probleme identificate

[x] Dezechilibru de clasă: Setul original conține mai multe piese defecte decât bune (un paradox industrial, de obicei e invers).

[x] Variație mică de unghi: Toate pozele sunt "top-down" (vedere de sus). Modelul ar putea eșua dacă piesa e rotită.

[x] Rezoluție: Imaginile sunt Grayscale, deci modelul nu se poate baza pe culoare (rugină), ci strict pe textură/formă.

4. Preprocesarea Datelor

4.1 Curățarea datelor

Eliminare duplicatelor: S-a verificat hash-ul MD5 al fișierelor; 0 duplicate identice găsite.

Tratarea valorilor lipsă: Nu se aplică (imagini complete).

Tratarea outlierilor: Imaginile cu dimensiuni atipice (<200px) au fost redimensionate forțat sau eliminate din set.

4.2 Transformarea caracteristicilor

Normalizare: Împărțirea valorilor pixelilor la 255.0 pentru a aduce intervalul [0, 255] în [0, 1] (esențial pentru convergența CNN).

Encoding pentru variabile categoriale: Etichetele folderelor (def_front, ok_front) au fost codificate binar: 0 = OK, 1 = Defect.

Ajustarea dezechilibrului de clasă: S-a aplicat augmentare (rotire, zoom, flip) doar pe clasa minoritară (ok_front) pentru a egaliza numărul de exemple.

4.3 Structurarea seturilor de date

Împărțire recomandată:

70–80% – train (pentru învățarea trăsăturilor vizuale)

10–15% – validation (pentru monitorizarea epocilor și tuning)

10–15% – test (date complet noi pentru evaluarea finală)

Principii respectate:

Stratificare: Se păstrează proporția Defect/OK în toate cele 3 seturi.

Fără scurgere de informație: Imaginile augmentate derivate dintr-o imagine originală stau în același set cu originalul (nu punem originalul în Train și varianta rotită în Test).

4.4 Salvarea rezultatelor preprocesării

Date preprocesate (augmentate) salvate fizic în data/processed/ organizate pe foldere de clasă.

Generatoarele de date Keras (ImageDataGenerator) configurate în cod pentru încărcare dinamică.

5. Fișiere Generate în Această Etapă

data/raw/ – imaginile originale descărcate ("Casting Data").

data/processed/ – structura finală gata de antrenare (train/, test/ cu subfoldere def_front, ok_front).

src/data_acquisition/augment_data.py – scriptul de generare a datelor sintetice (Augmentare).

src/preprocessing/image_utils.py – funcții de redimensionare și normalizare.



6. Stare Etapă (de completat de student)

[x] Structură repository configurată

[x] Dataset analizat (EDA realizată - verificare imagini)

[x] Date preprocesate (Augmentare și organizare foldere)

[x] Seturi train/val/test generate

[x] Documentație actualizată în README 