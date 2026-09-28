# Proyecto Final — Text Mining & Image Recognition
Marielena Lopez Salguero 25007402
Danny Rubén García Díaz 25000233

Espacio de trabajo>
https://drive.google.com/file/d/1d4S_bSNkR6B2hNoE-rN4bxk2Eixdaisc/view?usp=drive_link

Contiene dos notebooks de Google Colab que resuelven los dos problemas del enunciado HT3.

Problema1_WordCloud.ipynb y Problema2_Fruits_Vegetables_CNN

## 1. Descripción del proyecto

| # | Problema | Notebook | Resumen |
|---|----------|----------|---------|
1. Word Cloud | `Problema1_WordCloud.ipynb` 
A partir de ~1.6 M de tweets (Sentiment140), se determinan los 3 usuarios más populares con expresiones regulares, se arma un corpus por usuario (Content, ID, Timestamp, Length), se extrae el contexto (palabras que rodean a cada `@usuario`) y se aplican stopwords + stemming + lemmatization para generar un WordCloud del Top 10 términos por usuario. 
2.  Fruits and Vegetables Recognizer (CNN) | `Problema2_Fruits_Vegetables_CNN.ipynb` | CNN que distingue 6 clases (3 frutas + 3 vegetales, fusionando Fresh+Rotten). Se aplica data augmentation, normalización 1/255 y tamaño 128×128 RGB; se entrenan y comparan 3 arquitecturas y se evalúa la mejor sobre el conjunto de test. 

Decisiones técnicas tomadas:
- Problema 1: encoding `latin-1`, `header=None`, ranking por menciones en `text` (no en `author`), contexto con ventana de 5 palabras, stemming (Porter) + lemmatization (WordNet), WordCloud vía `generate_from_frequencies`.
- Problema 2: 6 clases fusionadas, carpeta plana para `flow_from_directory`, split 70/20/10, `ImageDataGenerator` con augmentación sólo en train, score de selección = `test_accuracy − gap_train_val`, `shuffle=False` en test.

---

## 2. Enunciado

El enunciado completo está en HT3.pdf.

---

## 3. Datasets: dónde obtenerlos y dónde colocarlos

Los datasets  de GitHub (superan el límite de 100 MB) por lo que se almacenan en Google Drive y los notebooks los montan en automatco.

### 3.1 Problema 1 — `tw_source.csv` (Sentiment140)
- Dónde obtenerlo: dataset Sentiment140 (~228 MB, 1,600,000 filas). 
- Ubicacion en Drive:
  ```
  /content/drive/MyDrive/TextMining/Dataset_problema1/tw_source.csv
  ```

### 3.2 Problema 2 — `Fruits_Vegetables_Dataset(12000)`
- Dónde obtenerlo: dataset Kaggle `Fruits_Vegetables_Dataset(12000)` (carpeta con subcarpetas `Fruits/` y `Vegetables/`, 20 clases Fresh/Rotten).
- ubicacion en Drive para que corra:
  ```
  /content/drive/MyDrive/TextMining/Dataset_problema2/Fruits_Vegetables_Dataset(12000)
  ```


## 4. Orden de ejecución de los notebooks

Los notebooks son independientes entre si, pero se recomienda este orden por complejidad creciente:
Al momento de correrlos el problema 2 tomo mucho mas tiempo del esperado que el 1 por lo que se recomienda iniciar con el word cloud

1. **`Problema1_WordCloud.ipynb`** (~5–10 min)
   - Monta Drive, carga `tw_source.csv`, extrae menciones con regex, arma corpus por usuario, aplica NLP y genera 3 WordClouds.
   - Secciones 14 y 15 (interpretación de cada usuario y respuesta a "¿por qué lo citan?") se completan manualmente tras ver el Top 10 real.

2. **`Problema2_Fruits_Vegetables_CNN.ipynb`** (~30–90 min con GPU, ~60–120 min con CPU)
   - cargar Drive, copiar ~7,200 imágenes, split 70/20/10, entrena 3 CNN (15 épocas c/u), compara y selecciona la mejor, evalúa sobre test.
   - Secciones 14 responde a la pregunta planteada en el pdf HT3 (¿por qué la CNN seleccionada funciona mejor?) y la 15 (conclusiones) se completan con los valores reales del entrenamiento.



## 5. como ejecutar en Colab

1. Subir los dos `.ipynb` a Google Drive.
2. Clic derecho y Abrir con → Google Colaboratory.
3. Entorno de ejecución, Cambiar tipo de entorno de ejecución a GPU T4.
4. Ejecutar las celdas en orden o entorno de ejecución y Ejecutar todas.
  Es recomendable si ejecuta todas ir revisando y dejar la pesta;a activa


## 6. Estructura del repositorio

```
text-mining-proyecto-final/
├── README.md
├── HT3.pdf                         # enunciado del proyecto
├── Problema1_WordCloud.ipynb       # Problema 1 — Word Cloud
├── Problema2_Fruits_Vegetables_CNN.ipynb   # Problema 2 — CNN
```



## 7. Reproducibilidad

- `SEED = 42` en `random`, `numpy` y `tf.random` (Problema 2).
- Resultados de referencia validados (Problema 2, 2 épocas de prueba):
  - CNN1: test_acc ≈ 0.69 · CNN2: ≈ 0.71 · CNN3: ≈ 0.68
  - Con 15 épocas los valores mejoran; la tabla y selección se completan con los valores reales tras la ejecución final.
