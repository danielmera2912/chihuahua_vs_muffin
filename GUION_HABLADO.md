# Guión Hablado — Chihuahua vs Muffin

---

## Slide 1 — Portada

"Os voy a presentar nuestro clasificador de chihuahuas contra muffins, un reto clásico de visión por computador."

---

## Slide 2 — El Problema

"Como veis, las dos clases se parecen mucho: forma redonda, tonos marrones, texturas similares. Hasta a un humano le cuesta. Usamos un dataset de Kaggle con unas 5.400 imágenes, repartidas en train, validación y test como veis en la tabla."

---

## Slide 3 — Arquitectura del Modelo

"Nuestra CNN tiene 4 bloques convolucionales con filtros crecientes, cada uno con Batch Normalization y MaxPooling. En las capas densas metemos Dropout agresivo para que el modelo no memorice los datos de entrenamiento y luego falle con imágenes nuevas. Y el Data Augmentation —flip, rotación, zoom, contraste— va integrado como capas del modelo, así solo se activa al entrenar."

---

## Slide 4 — Entrenamiento

"Entrenamos con Adam y binary crossentropy. Lo importante son los callbacks: EarlyStopping con paciencia de 5, ReduceLROnPlateau que baja el learning rate si se estanca, y un límite de 10 minutos. Al final entrenó 12 épocas de 30 posibles. En las gráficas se ve que converge de forma estable."

---

## Slide 5 — Resultados

"El modelo llega a un 91.45% de precisión en test sobre 1.076 imágenes nuevas. El AUC-ROC es 0.972, muy alto. Y el test supera a la validación, así que no hay overfitting, o lo que es lo mismo, el modelo no se ha sobreajustado a los datos de entrenamiento."

---

## Slide 6 — Experimentos y Comparativa

"Partimos de un modelo base con 3 bloques y sin regularización, que daba un 75%. Añadimos un cuarto bloque, Data Augmentation, BatchNorm y Dropout, y subimos a más del 90%. Lo que más impactó fue el Data Augmentation y el BatchNorm."

---

## Slide 7 — Análisis de Errores

"De 1.076 imágenes solo fallamos en 92, un 8.5%. Y es simétrico: 46 chihuahuas confundidos y 46 muffins. Los errores se dan en imágenes ambiguas: chihuahuas claros, muffins desde ángulos raros, o ilustraciones. Con Grad-CAM vimos que en esos casos el modelo se fija en el fondo en vez del objeto, que es lo que causa el fallo."

---

## Slide 8 — Matriz de Confusión y Curvas ROC

"Aquí tenéis la matriz de confusión: 488 chihuahuas y 496 muffins bien clasificados, y 46 errores por clase. Totalmente equilibrado. Las curvas ROC y Precision-Recall confirman un AUC por encima de 0.97."

---

## Slide 9 — Demo en Vivo (Gradio)

"También hicimos una interfaz web con Gradio donde se puede subir una imagen y ver la predicción con su confianza. Tiene un modo avanzado que muestra el mapa de calor Grad-CAM."

---

## Slide 10 — Mejoras Posibles

"Como mejoras futuras: transfer learning con MobileNetV2 o EfficientNet, ampliar el dataset con ejemplos difíciles, probar un Vision Transformer, o hacer ensemble de varios modelos."

---

## Slide 11 — Conclusiones

"En resumen: 91.45% de precisión en test con una CNN desde cero. Las claves fueron Data Augmentation, BatchNorm con Dropout, y usar Grad-CAM y TensorBoard para depurar.

En cuanto al equipo: Juan Manuel preparó los datos, Manuel diseñó y entrenó el modelo, Israel hizo las visualizaciones y análisis de errores, y yo me encargué de la documentación y la presentación."

---

## Si te preguntan...

| Pregunta | Respuesta rápida |
|---|---|
| "¿Por qué CNN y no transfer learning?" | "Queríamos entender la arquitectura desde cero. Transfer learning sería el siguiente paso." |
| "¿Cómo evitáis overfitting?" | "Data Augmentation, Dropout del 50% y 30%, BatchNorm y EarlyStopping." |
| "¿Qué precisión en test?" | "91.45% sobre 1.076 imágenes. AUC-ROC: 0.972." |
| "¿Qué es Grad-CAM?" | "Genera mapas de calor que muestran qué zonas de la imagen influyeron más en la predicción." |
| "¿Qué mejoraríais?" | "Transfer learning con MobileNetV2, más datos difíciles, y probar un Vision Transformer." |
| "¿El umbral de confianza?" | "sigmoid < 0.5 es chihuahua, ≥ 0.5 es muffin. En Gradio usamos 0.7 para descartar predicciones poco seguras." |
