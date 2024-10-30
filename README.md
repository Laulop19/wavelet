# Variabilidad de la Frecuencia Cardiaca usando la Transformada Wavelet

## 💡 CONCEPTOS IMPORTANTES

### Sistema Nervioso Autónomo (SNA)
El SNA regula la frecuencia cardíaca y su variabilidad a través de dos ramas:

- **Sistema Simpático**:  
  Aumenta el ritmo cardíaco y la fuerza de contracción del corazón para preparar el cuerpo ante situaciones de estrés o actividad física intensa. La actividad simpática activa las células del nodo sinoauricular (marcapasos natural del corazón) para incrementar la frecuencia cardíaca, generando ritmos más rápidos y consistentes.

- **Sistema Parasimpático**:  
  Reduce el ritmo cardíaco y promueve la recuperación, controlando la frecuencia durante el descanso. El nervio vago, asociado al parasimpático, disminuye la actividad del nodo sinoauricular, favoreciendo una frecuencia cardíaca más baja y variada, que indica relajación y recuperación del organismo.

### Variabilidad de la Frecuencia Cardíaca (HRV)
La HRV mide las fluctuaciones en los intervalos entre latidos consecutivos (intervalo R-R) y refleja la capacidad del SNA para adaptar la frecuencia cardíaca en función de las necesidades del cuerpo. Una HRV alta indica una mayor capacidad de adaptación y flexibilidad del sistema cardíaco, mientras que una HRV baja puede señalar limitaciones en la regulación autónoma o condiciones de estrés.

### Frecuencias de Interés en la HRV
Para el análisis de la HRV, se estudian ciertas bandas de frecuencia en los intervalos R-R:

- **Frecuencia Baja (LF: 0.04 - 0.15 Hz)**:  
  Refleja principalmente la actividad del sistema simpático, aunque también incluye elementos del parasimpático. Cambios en esta banda sugieren un cambio en la respuesta de estrés o activación del cuerpo.

- **Frecuencia Alta (HF: 0.15 - 0.4 Hz)**:  
  Se asocia principalmente con la actividad parasimpática y con la respiración. Valores altos de HF indican una buena actividad vagal y control parasimpático. Frecuencias altas son típicas en periodos de relajación o recuperación.

Una relación LF/HF alta sugiere predominancia simpática, mientras que una relación baja indica mayor actividad parasimpática.

A medida que una persona envejece, la HRV tiende a disminuir. Esto ocurre porque el sistema nervioso autónomo (SNA) pierde flexibilidad y capacidad de respuesta, especialmente en su componente parasimpático, que suele reducir su influencia en la frecuencia cardíaca. El valor típico de HRV normal es que la edad de 20 años suele tener una HRV media en el rango de 55-105, mientras que la edad de 60 años tiende a estar entre 25-45.

Los valores de potencia de LF y HF suelen ser más altos en jóvenes porque hay más variabilidad en la frecuencia cardíaca. Esto muestra que el SNA es flexible y responde rápidamente a cambios. En adultos, la potencia espectral de LF y HF disminuye, ya que la frecuencia cardíaca se vuelve más constante, reflejando menos adaptabilidad del SNA.

## Transformada Wavelet y la Wavelet Morlet

La Wavelet Morlet es una onda compuesta por una exponencial compleja multiplicada por una ventana gaussiana, lo que le da la capacidad de detectar eventos localizados en una señal. Se trata de una wavelet compleja y continua, que permite obtener una representación detallada y continua de la señal en diferentes escalas de frecuencia a lo largo del tiempo. Esto es ideal para señales no estacionarias, como el ECG, donde las características pueden variar con el tiempo.

### 🚩 CARACTERÍSTICAS CLAVE DE LA TRANSFORMADA WAVELET MORLET

- **Continuidad**:  
  Al ser una wavelet continua, la Transformada Wavelet Morlet ofrece una representación fluida en el tiempo, lo que facilita el análisis detallado de cada instante de la señal.

- **Resolución en tiempo y frecuencia**:  
  La Morlet wavelet tiene una alta precisión tanto en el dominio del tiempo como en el de la frecuencia, lo cual es ideal para identificar cambios graduales o rápidos en la señal.

- **Forma Gaussiana**:  
  La función de Morlet está modulada por una envolvente gaussiana, lo que permite una transición suave entre las escalas de frecuencia y minimiza el ruido en el análisis de los picos y variaciones de la señal de ECG.

- **Uso en análisis local**:  
  Esta wavelet es particularmente útil para detectar cambios sutiles en cortos períodos de tiempo dentro de la señal de ECG, capturando patrones rápidos y eventos de interés en la frecuencia cardíaca.
## 📖 DIAGRAMA DE FLUJO DEL PLAN DE ACCIÓN PARA CUMPLIR EL OBJETIVO DE LA PRÁCTICA 

## 💾 ADQUISICIÓN DE LA SEÑAL

En la siguiente imagen podremos ver la señal original de 5 minutos y un fragmento de 10 segundos para que se logre distinguir mejor la señal y picos R-R:

Señal original y filtrada de 5 minutos:
![jiji](https://github.com/user-attachments/assets/551d5b58-f29e-411a-8a56-d484bac1e50b)

Señal original y filtrada de 10 segundos:
![image](https://github.com/user-attachments/assets/cc9bf19a-2590-4534-94f0-23ea048c8a62)


Para la adquisición de la señal ECG, utilizamos un sensor **AD8232** conectado a tres electrodos en una configuración específica que se detalla en la siguiente imagén. 
<div align="center">
    <img src="https://github.com/user-attachments/assets/c6038390-5c65-4b90-b01c-908cbbc80bd7" width="300" alt="Descripción de la imagen">
</div>

Los electrodos fueron colocados sobre la piel limpia de la persona, capturando la actividad eléctrica del corazón y enviando la señal al sensor AD8232. Este se conectó a una **STM** que, a través de un cable USB-C, transmitió los datos por conexión serial al computador. En el computador, una interfaz desarrollada en **Python** recibió y mostró la señal en tiempo real, además de guardar los datos adquiridos en un archivo .txt para su posterior procesamiento. Esta configuración permitió una adquisición eficiente y confiable de la señal ECG, manteniendo la calidad y la integridad de los datos para el análisis posterior.

### Parámetros Técnicos
- **Frecuencia de muestreo**: 1000 Hz, lo que significa que se registraron 1000 muestras por segundo. Esto es una frecuencia adecuada para captar detalles rápidos de la señal de ECG, ya que los eventos cardíacos importantes, como los picos QRS, tienen una duración de milisegundos.

- **Tiempo de muestreo**: Con una frecuencia de muestreo de 1000 Hz, el tiempo de muestreo es de 0.001 segundos (1 ms), lo que asegura que se capturen todos los eventos significativos de la señal cardíaca sin distorsión.

- **Niveles de cuantificación**: Es el proceso de asignar un valor discreto a cada muestra, convirtiendo así el valor analógico continuo en un valor digital. Se usaron 50 bytes para la cuantificación, es decir, por cada 0.001 segundos se reciben 50 datos.

- **Media de la señal ECG**: Hace referencia al valor promedio del intervalo R-R (tiempo entre dos picos R consecutivos en el ECG) a lo largo de los 5 minutos de grabación. La media de nuestra señal es de **984.21 ms**, lo que nos indica una frecuencia cardíaca promedio de aproximadamente 61 latidos por minuto (1 latido cada 984.21 ms).

- **Desviación estándar**: Indica la variabilidad en los intervalos R-R. Nuestra desviación estándar es de **329.42 ms**, lo que sugiere que la frecuencia cardíaca varía de un latido a otro, lo cual es normal y esperado en un ECG humano. La HRV se mide precisamente a través de esta variación entre intervalos R-R, y un valor moderado de desviación estándar en este rango sugiere una flexibilidad del sistema cardíaco que puede ser mejorada.
## 📊 FILTROS

## 🔍 ANÁLISIS DE LA HRV EN EL DOMINIO DEL TIEMPO

## 💻 APLICACIÓN DE TRANSFORMADA WAVELET
### Transformada Wavelet Morlet
La transformada wavelet utilizada fue la wavelet Morlet. Esta wavelet es apropiada para análisis de señales como ECG debido a su capacidad de representar frecuencias de oscilación, permitiendo identificar patrones transitorios y cambios en la frecuencia a lo largo del tiempo. Además, la wavelet de Morlet es adecuada para capturar tanto componentes de baja como de alta frecuencia en una señal compleja como el ECG, donde eventos breves o transitorios pueden contener información significativa (como latidos del corazón, arritmias, etc.).

Para efectos de una mejor visualización del espectrograma, inicialmente se analizará el espectrograma de la señal completa es decir de los 5 minutos y luego de un fragmento de 30 segundos.

### Espectrograma de la Transformada Wavelet Morlet (5 minutos)
![image](https://github.com/user-attachments/assets/14759483-77b8-4220-9fa6-a84cae1a2591)
En el espectrograma de la señal EKG completa (300 segundos), que abarca un rango de frecuencias de 0 a 100 Hz, se observa una serie de líneas verticales que indican actividad periódica en todo el rango temporal.

- **Banda de baja frecuencia (0 a 20 Hz):** Esta banda, que es relevante para observar los componentes fundamentales del ECG como las ondas P, T y el complejo QRS, muestra una periodicidad marcada, lo que corresponde a la actividad regular del corazón. Sin embargo, no parece haber picos claros o variaciones destacadas en la potencia a lo largo del tiempo, lo que indica que no hay cambios abruptos en la actividad cardíaca durante los 5 minutos de registro. Esto es consistente con un ritmo cardíaco estable.

- **Banda de alta frecuencia (20 a 100 Hz):** Las frecuencias más altas están generalmente relacionadas con ruido electromiográfico (EMG) o interferencias ambientales. Aunque no se observan picos muy pronunciados en esta banda, sí se observan algunos patrones más intensos en las frecuencias altas. Sin embargo, en este espectrograma de 5 minutos, la actividad de alta frecuencia parece mantenerse constante y no sugiere ninguna anomalía o evento de alta energía durante el período de análisis.

El espectrograma de 5 minutos refleja un ECG estable sin grandes cambios en la potencia espectral tanto en las frecuencias bajas (asociadas a la actividad cardíaca) como en las altas (posiblemente ruido o actividad menor).

### Espectrograma de la Transformada Wavelet Morlet (30 segundos)
![image](https://github.com/user-attachments/assets/11baae30-b91c-46ea-b9fe-3cdc1b88fb36)
El espectrograma de 30 segundos proporciona una vista más detallada y permite observar eventos con mayor precisión.

- **Banda de baja frecuencia (0 a 20 Hz):** En esta banda, se observan picos claros en color verde y azul claro, lo que indica variaciones en la potencia espectral. Estas variaciones estan relacionadas con los latidos del corazón, más específicamente con las ondas R (pico principal del complejo QRS en el ECG), que suelen tener un alto contenido energético. Los picos periódicos en esta banda reflejan la actividad cíclica del corazón, y los colores más claros (verde/azul claro) indican un aumento de la potencia en esos momentos, lo que corresponde a los latidos del corazón o a eventos específicos del ciclo cardíaco.

- **Banda de alta frecuencia (20 a 100 Hz):** En esta región también hay ciertos picos, aunque menos pronunciados, lo que podría estar asociado con ruido de alta frecuencia o actividad eléctrica relacionada con la contracción muscular del corazón. Las líneas verticales claras en esta banda corresponden a pequeñas interferencias electromiográficas o a fluctuaciones rápidas en la señal.

#### Variaciones de la Frecuencia y Potencia Espectral
A lo largo del tiempo, en la señal se observan fluctuaciones en la potencia espectral, especialmente en la banda baja, lo que indica una actividad cardíaca cíclica. Los picos verdes y azules claros sugieren que hay variaciones en la amplitud del ECG en esos puntos, lo cual representa latidos con mayor energía o variabilidad en la fuerza de los latidos. En cuanto a las frecuencias altas, no se detectan cambios sustanciales a lo largo del tiempo, lo que sugiere que el ruido o las interferencias permanecen relativamente constantes durante el periodo de análisis.

Entonces, frecuencias bajas (0-20 Hz) reflejan la actividad cardíaca, con picos correspondientes a los latidos del corazón (complejos QRS) y variaciones periódicas en la potencia espectral y frecuencias altas (20-100 Hz) están más relacionadas con ruido o actividad de alta frecuencia, pero en este caso, no hay eventos de alta energía que sugieran anomalías significativas.







