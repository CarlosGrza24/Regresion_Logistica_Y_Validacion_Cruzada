# Regresión logística y validación cruzada

Este proyecto analiza la relación entre distintos sistemas de riego y la probabilidad de que un municipio presente alta vs baja cantidad de unidades de producción (UP) agropecuarias activas. El objetivo es predecir alta/baja con 
regresión logística y evaluar su desempeño con validación cruzada. La hipótesis es que la tecnificación del riego (aspersores, pivote central, microaspersión, goteo, etc.) se asocia positivamente con una mayor probabilidad de alta actividad.

Los datos provienen del **Censo Agropecuario 2022 (INEGI)**, disponibles públicamente en la plataforma oficial de descargas:
- [INEGI – Censo Agropecuario 2022](https://www.inegi.org.mx/app/descarga/ficha.html?tit=1795268&ag=0&f=csv)

Archivo utilizado: [CA2022_Cultivos_Superficie_Produccion.csv](CA2022_Cultivos_Superficie_Produccion.csv)  

## Datos utilizados
Del archivo [DiccionarioDeDatos.csv](DiccionarioDeDatos.csv), las variables incluidas en el dataset son:

- Total UP agropecuaria activas → Total de unidades de producción activas (objetivo, luego binarizado por mediana 1=alta, 0=baja)  
- UP Agricolas → Total de UP agrícolas  
- UP Argri Riego (R) → UP agrícolas con riego  
- UP Riego Gravedad (RG) → UP con riego por gravedad  
- UP R Por Microaspersión → UP con riego por microaspersión  
- UP R Por Aspersión → UP con riego por aspersión general  
- UP R Por Aspersión Pivote Central → UP con riego por pivote central  
- UP R Por Aspersión Avance Frontal → UP con riego por avance frontal  
- UP R Por Aspersión Cañón → UP con riego por aspersión de cañón  
- UP R Por Aspersores → UP con riego por aspersores  
- UP R Sistema De Goteo → UP con riego por goteo  
- (Identificadores como NOM_MUN, ENTIDAD, NOMBRE se eliminaron en el preprocesamiento).

## Metodología

1. **Exploración inicial**
   - Revisión de tipos, valores faltantes y distribución de la variable objetivo.

2. **Limpieza y preprocesamiento**
   - Eliminación de columnas identificadoras.
   - Filtro de outliers (Tukey) sobre el objetivo.
   - Control de colinealidad: se descartan predictores con |r|>0.85.

3. **Construcción del objetivo**
   - Binarización de Total UP agropecuaria activas con la mediana: 1=alta, 0=baja (clases balanceadas ~50/50).

4. **Partición de datos (80/20)**
   - train_test_split(..., stratify=y_bin) para conservar la proporción 0/1 en Train y Test (~50/50 en ambos).

5. **Modelado**
   - Pipeline = StandardScaler + LogisticRegression.
   - Validación cruzada 5-fold estratificada con cross_validate (métricas: accuracy, recall, precision, F1, ROC-AUC).

6. **Evaluación en test y umbrales**
   - Cálculo de probabilidades en Test.
   - Métricas a tres umbrales (0.3, 0.5, 0.7) + matrices de confusión.
   - Curva ROC y AUC.

7. **Interpretación**
   - Coeficientes β estandarizados y odds ratio (e^β) para estimar el aporte relativo de cada sistema de riego.

## Archivos del proyecto
- [Reporte en ipynb](A2.1_648241.ipynb)
- [Reporte en html](A2.1_648241.html)  
- [CA2022_Cultivos_Superficie_Produccion.csv](CA2022_Cultivos_Superficie_Produccion.csv)  
- [DiccionarioDeDatos.csv](DiccionarioDeDatos.csv)
