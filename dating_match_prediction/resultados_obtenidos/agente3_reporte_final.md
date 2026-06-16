---
Pipeline: Recomendacion y Matchmaking en Apps de Citas
Transformer: mistral-large-latest
Dataset: 12,000 pares de usuarios
Modelo: Logistic Regression
---

```markdown
# Reporte Ejecutivo: Modelo de Predicción de "Likes Mutuos" en la App de Citas

## 1. Resumen Ejecutivo
Desarrollamos un modelo para predecir qué parejas de usuarios tienen mayor probabilidad de darse un *like mutuo* (o "match") en la app. Analizamos 12,000 interacciones reales y limpiamos datos inconsistentes (como perfiles incompletos o ubicaciones mal escritas). **El hallazgo clave es que el modelo actual tiene una capacidad predictiva limitada**: acierta solo un poco más que el azar, pero nos da pistas valiosas sobre qué factores influyen en los *matches*.

---

## 2. ¿Qué tan bueno es el modelo?
Imagina que estás adivinando si una moneda caerá cara o cruz:
- **Si lanzas la moneda al aire (azar)**, acertarías el 50% de las veces.
- **Nuestro modelo acierta el 59% de las veces**, es decir, solo un 9% mejor que el azar. No es un resultado fuerte, pero es un punto de partida para mejorar.

**¿Qué significa esto en la práctica?**
- De cada 100 *matches* reales, el modelo detecta **51** (pero se pierde 49).
- También predice **34 *matches* que en realidad no ocurren** (falsas alarmas).

*Nota*: Un modelo ideal debería acertar al menos el 70-80% para ser útil en producción.

---

## 3. ¿Qué hace que dos personas tengan *match*?
El modelo identificó los 5 factores más importantes para predecir un *like mutuo*. **Ordenados por relevancia**:

1. **Calidad del perfil (promedio entre ambos usuarios)**
   - *Qué es*: Perfiles con fotos claras, descripciones detalladas y sin información contradictoria.
   - *Insight*: Los usuarios prefieren perfiles completos y auténticos. Un perfil bien hecho aumenta las chances de *match* en un **2.5%**.

2. **Popularidad del usuario menos popular de la pareja**
   - *Qué es*: Si uno de los dos tiene muchos *likes* de otros usuarios, la probabilidad de *match* sube.
   - *Insight*: La "escasez" juega a favor: si un usuario es muy deseado, el otro tiene más incentivos para darle *like* (efecto "premio").

3. **Compatibilidad de intereses y estilo de vida**
   - *Qué es*: Similitud en gustos (ej.: música, hobbies), horarios de actividad en la app o metas de relación.
   - *Insight*: Los usuarios buscan conexiones con personas afines. Una compatibilidad alta aumenta las chances de *match* en un **1.9%**.

4. **Popularidad del usuario más popular de la pareja**
   - *Qué es*: Si el usuario con más *likes* de la pareja es muy activo o atractivo, el *match* es más probable.
   - *Insight*: La popularidad genera un "efecto imán": los usuarios populares atraen más *likes*, incluso de quienes no los conocen.

5. **Distancia geográfica**
   - *Qué es*: Cercanía física entre los usuarios (en kilómetros).
   - *Insight*: La proximidad sigue siendo clave. Aunque la app es digital, los usuarios prefieren *matches* cercanos (posiblemente para facilitar encuentros).

---

## 4. Riesgos y Limitaciones
**1. El modelo no es lo suficientemente preciso (alerta [ALTO])**
   - *Riesgo*: Si lo usáramos para recomendar parejas, **1 de cada 3 *matches* sugeridos sería incorrecto** (falso positivo). Esto podría generar frustración en los usuarios ("¿Por qué me recomiendas a alguien que no me dará *like*?").
   - *Oportunidad*: Podemos usarlo como filtro inicial, pero no como única herramienta de recomendación.

**2. Los datos actuales explican solo una parte del comportamiento (alerta [ALTO])**
   - *Riesgo*: Hay factores que el modelo no captura, como la química personal o el contexto temporal (ej.: un usuario que solo busca citas los fines de semana).
   - *Oportunidad*: Necesitamos enriquecer los datos con información más dinámica (ej.: patrones de uso de la app).

**3. Hay más *no-matches* que *matches* en los datos (alerta [MEDIO])**
   - *Riesgo*: El modelo está "entrenado" para ser más conservador y evitar falsos positivos, lo que puede hacer que pierda *matches* reales.
   - *Oportunidad*: Podemos ajustar el umbral de predicción para priorizar más *matches*, aunque esto aumente los errores.

---

## 5. Próximos Pasos Recomendados
**Prioridad 1: Mejorar la calidad de los datos (corto plazo)**
   - **Acción**: Añadir variables que capturen el *contexto* de los usuarios, como:
     - Horarios de actividad en la app (ej.: "¿Ambos están en línea los domingos por la noche?").
     - Historial de *likes* recientes (ej.: "¿El usuario A suele dar *like* a perfiles similares al de B?").
   - **Impacto esperado**: Podría aumentar la precisión del modelo en un 5-10%.

**Prioridad 2: Experimentar con recomendaciones híbridas (mediano plazo)**
   - **Acción**: Combinar el modelo actual con reglas de negocio, como:
     - Mostrar primero a usuarios con alta compatibilidad de intereses + cercanía geográfica.
     - Usar el modelo solo para filtrar parejas con probabilidad de *match* > 60%.
   - **Impacto esperado**: Reducir falsos positivos y mejorar la experiencia de usuario.

**Prioridad 3: Validar el modelo en un entorno real (largo plazo)**
   - **Acción**: Lanzar un *A/B test* con un grupo pequeño de usuarios para medir:
     - ¿Aumentan los *matches* reales con las recomendaciones del modelo?
     - ¿Los usuarios reportan mayor satisfacción con las sugerencias?
   - **Impacto esperado**: Datos reales para ajustar el modelo antes de escalarlo.

**Prioridad 4: Explorar modelos alternativos (largo plazo)**
   - **Acción**: Probar enfoques más avanzados, como:
     - Modelos que aprendan patrones temporales (ej.: "Los usuarios dan más *likes* los viernes por la noche").
     - Sistemas que incorporen *feedback* en tiempo real (ej.: si un usuario ignora una recomendación, ajustar