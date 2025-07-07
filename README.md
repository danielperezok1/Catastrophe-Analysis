# Catastrophe-Analysis

##  Contexto

Durante el procesamiento del mes **junio de 2020 (202006)**, el equipo de Data Warehousing informó un **error crítico**: múltiples variables quedaron con **valor 0** en lugar de sus valores reales. Esta falla impactó directamente en la calidad del dataset usado para entrenar modelos de predicción de bajas.


## Hipótesis

> Si reimputamos las variables rotas (en cero) utilizando distintas técnicas de imputación, el modelo debería mejorar su performance y generar una mayor ganancia económica en la campaña de retención proactiva.


## 🧠 Sesgos Cognitivos 

###  Sesgo de Confirmación

> Tendencia a interpretar resultados que confirmen nuestras creencias previas, en este caso, que imputar **debería mejorar** el modelo.

###  Mitigación del sesgo

- Comparación objetiva entre métodos usando métricas cuantitativas como **ganancia acumulada**.
- Evaluación con criterio **ceteris paribus**, manteniendo todo el resto del workflow sin cambios.

---

## 🔬 Procedimiento Experimental

Se probaron **cuatro alternativas de imputación** sobre las variables rotas del período 202006:

1. **CEROS**
   - No se modifica el dataset. Las variables rotas permanecen en cero.
2. **NA**
   - Los ceros se reemplazan por `NA`, confiando en que **LightGBM** maneje internamente los valores faltantes.
3. **Interpolación Lineal**
   - Se reconstruyen los valores faltantes usando una recta entre los valores de los meses anterior y posterior.
   - Se aplica cliente por cliente, por variable afectada.
4. **MICE** (Multiple Imputation by Chained Equations)
   - Imputa los valores faltantes usando otras variables del cliente (edad, antigüedad, cantidad de productos, etc.).

---

## 📊 Resultados

- Las **curvas de ganancia acumulada** para los cuatro métodos muestran un comportamiento **prácticamente idéntico**.
- Todos los modelos alcanzan su **máximo de ganancia en el mismo punto**.
- Se observa que el ordenamiento de los clientes por probabilidad de **BAJA+2** es consistente entre métodos.

---

## 📈 Conclusiones

- **No hay diferencias  en ganancia** entre los métodos de imputación.
- Se puede priorizar la técnica **más simple, rápida y reproducible**.
- Las estrategias más recomendables:  imputar como `NA` si se utiliza un modelo que maneja faltantes (ej. LightGBM).

---

## 💡 Recomendación

> En contextos donde las variables rotas afectan un único período, y el modelo permite trabajar con valores faltantes, **imputar con `NA` es suficiente**. No se justifica el uso de técnicas más complejas si no mejoran significativamente la métrica objetivo.

---



