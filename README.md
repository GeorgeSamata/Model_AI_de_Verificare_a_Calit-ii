# 📘 README – Etapa 3: Analiza și Pregătirea Setului de Date

**Proiect:** Model_AI_de_Verificare_a_Pieselor  
**Disciplina:** Rețele Neuronale  
**Instituție:** POLITEHNICA București – FIIR  
**Student:** Șamata George Cristian 
**Data:** 13.10.2025

---


## 2\. Descrierea Setului de Date

### 2.1 Sursa datelor

  * **Origine:** Kaggle Industrial Quality Inspection
  * **Modul de achiziție:**  Fișier extern (Imagini digitale) / Senzori
  * **Perioada / condițiile colectării:** Imaginile au fost preluate static, sub iluminare controlată, pentru a evidenția defectele de suprafață.

### 2.2 Caracteristicile dataset-ului

  * **Număr total de observații:** [Ex: 1,500 imagini]
  * **Număr de intrări (Input):** 3 (Canalele RGB)
  * **Număr de ieșiri (Output):** 3 Clase (Ex: OK, Defect\_A, Defect\_B)
  * **Tipuri de date:**  Imagini (Matriceale) /  Tabelare
  * **Format fișiere:**  JPG/PNG

### 2.3 Descrierea fiecărei caracteristici (Intrări)

Rețeaua neuronală primește o imagine color, tratând fiecare canal de culoare ca o caracteristică (input) distinctă care contribuie la decizia finală.

| **Caracteristică** | **Tip** | **Unitate** | **Descriere** | **Domeniu valori** |
| :--- | :--- | :--- | :--- | :--- |
| **Canal Roșu (R)** | Matrice | Intensitate | Componenta de culoare roșie, evidențiază defecte tip rugină sau arsuri. | 0 – 255 |
| **Canal Verde (G)** | Matrice | Intensitate | Componenta de culoare verde, oferă contrast principal pentru piese. | 0 – 255 |
| **Canal Albastru (B)** | Matrice | Intensitate | Componenta de culoare albastră, utilă pentru detalii fine și umbre. | 0 – 255 |

> **Notă despre Ieșiri (3 Clase):** Sistemul clasifică piesa în una din cele 3 stări:
>
> 1.  **Piesă Bună (OK)**
> 2.  **Defect Tip 1** (ex: Zgârietură / Fisură)
> 3.  **Defect Tip 2** (ex: Deformare / Lipsă material)

-----

## 3\. Analiza Exploratorie a Datelor (EDA)

### 3.1 Statistici descriptive aplicate

  * **Distribuția claselor:** Verificarea numărului de imagini pentru fiecare din cele 3 categorii pentru a detecta dezechilibre (Class Imbalance).
  * **Analiza rezoluției:** Verificarea dimensiunilor (Height x Width) pentru a decide strategia de redimensionare.
  * **Analiza canalelor:** Vizualizarea histogramelor pentru canalele R, G, B pentru a detecta imagini supraexpuse sau prea întunecate.

### 3.2 Analiza calității datelor

  * **Detectarea formatelor greșite:** Identificarea fișierelor care nu sunt imagini (ex: `.txt`, `.thumbs`) în folderele de date.
  * **Detectarea imaginilor corupte:** Script pentru deschiderea fiecărei imagini cu `TensorFlow` pentru a valida integritatea fișierului.
  * **Verificare consistență:** Asigurarea că toate imaginile au 3 canale (RGB) și nu sunt Grayscale (1 canal).

### 3.3 Probleme identificate

  * [Exemplu] Clasa "Defect Tip 2" are cu 40% mai puține imagini decât clasa "OK".
  * [Exemplu] Variabilitate mare în dimensiunile imaginilor originale (necesită Resize).
  * [Exemplu] Prezenta zgomotului de fond în 5% din imagini.

-----

## 4\. Preprocesarea Datelor

### 4.1 Curățarea datelor

  * **Eliminare duplicate:** Ștergerea imaginilor identice folosind hash-uri MD5.
  * **Standardizare format:** Convertirea tuturor imaginilor la format `.png` sau `.jpg`.

### 4.2 Transformarea caracteristicilor

Pentru a pregăti datele pentru TensorFlow, se aplică următoarele transformări:

1.  **Redimensionare (Resizing):** Uniformizarea dimensiunilor spațiale.
      * *Target Size:* $224 \times 224$ pixeli (sau similar).
2.  **Normalizare (Rescaling):** Aducerea valorilor pixelilor în intervalul $[0, 1]$.
      * *Formulă:* $x_{new} = \frac{x_{old}}{255.0}$ aplicată pe toate cele 3 canale (R, G, B).
3.  **Encoding Ieșiri:** Transformarea etichetelor categoriale în vectori *One-Hot*:
      * Clasa 1: `[1, 0, 0]`
      * Clasa 2: `[0, 1, 0]`
      * Clasa 3: `[0, 0, 1]`

### 4.3 Structurarea seturilor de date

Datele sunt amestecate (shuffled) și împărțite menținând proporția claselor (**Stratified Split**):

  * **Train (70%):** Folosit pentru antrenarea ponderilor.
  * **Validation (15%):** Folosit pentru monitorizarea performanței (loss/accuracy) în timpul epocilor.
  * **Test (15%):** Folosit strict pentru evaluarea finală.

-----

## 5\. Fișiere Generate în Această Etapă

  * `data/raw/` – Structura originală a datasetului.
  * `data/processed/` – (Opțional) Datele salvate în format binar TFRecord pentru viteză.
  * `src/preprocessing/data_loader.py` – Funcția `image_dataset_from_directory` configurată.
  * `plots/class_distribution.png` – Graficul distribuției celor 3 clase.

-----

## 6\. Stare Etapă

  - [X] Structură repository organizată
  - [X] Analiza EDA finalizată (verificat cele 3 intrări RGB)
  - [X] Pipeline de preprocesare activ (Resize + Normalize)
  - [X] Seturi Train / Val / Test generate corect
  - [X] Documentație completă



```
```