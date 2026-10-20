# Análisis y diseño

## 1. Descripción del problema
El sistema que se va a crear en esta practica es la automatización y monitoreo de un sistema de invernadero para tener un entorno
óptimo de cultivo .

La información que se necesitara conocer sera la siguiente:
* Temperatura del aire 
* Humedad ambiental
* Humedad del suelo
* Dispositivos de medición
* Dispositivos de control

Los elementos que interviene dentro del sistema son los siguientes:
* Sensores (medición).
* Actuadores (control).

Las operaciones generales que se deberan realizar dentro del sistema son las siguientes:
* Lectura y simulación de datos.
* Interpretación de los datos.
* Acciones a realizar.
* Generación de historial de los datos y las operaciones.

## 2. Identificación de objetos
1. **Sensor de Temperatura del Aire**
    * Se necesita para tener la medición de la temperatura en el invernadero que afecta a las plantas del mismo
    * La responsabilidad que tiene es el registro de la temperatura y evaluar el rango de temperatura para las plantas en el invernadero.

2. **Sensor de Humedad Ambiental**
    * Se necesita para medir  cantidad de vapor de agua que afecta la transpiración de las plantas.
    * Sera el responsable de registrar el porcentaje de humedad relativa del aire, y determinar muy seco o saturado .

3. **Sensor de Humedad del Suelo**
    * Es el que da la información  mas directa para decidir cuándo regar.
    *  Medir el porcentaje de agua  en la tierra e indicar si la planta necesita riego  o si la tierra ya tiene suficiente humedad.

4. **Sistema de Riego**
    *  Es el actuador que controla el suministro de agua.
    *  Controlar el encendido y apagado de las bombas de riego y reportar si se encuentra activo o inactivo.

## 3. Estado y comportamiento
| Objeto propuesto | Responsabilidad                                                                                              | Información que debe conservar                                                                             | Comportamientos que debe realizar                                                                                             |
|---|--------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Sensor de temperatura | Monitorear la temperatura del aire para identificar si el clima es adecuado para las plantas.                | Ubicación en el invernadero, estado de funcionamiento (activo/inactivo) y última lectura registrada .      | Registrar nueva lectura, informar su valor actual y evaluar si la temperatura se encuentra en un nivel bajo, adecuado o alto. |
| Sensor de humedad ambiental | Monitorear la cantidad de agua presente en el aire para prevenir la deshidratación o el exceso de humedad.   | Ubicación en el invernadero, estado de funcionamiento (activo/inactivo) y última lectura registrada.       | Registrar nueva lectura, informar su valor actual y determinar si el ambiente está muy seco, óptimo o saturado.               |
| Sensor de humedad del suelo | Medir el agua disponible en la tierra de las plantas para determinar si el terreno requiere riego inmediato. | Ubicación en el invernadero, estado de funcionamiento (activo/inactivo) y última lectura registrada. | Registrar nueva lectura, informar su valor actual y evaluar si la tierra está seca , nivel adecuado o saturado.               |
| Sistema de riego | Modificar las condiciones de agua en la tierra activando o desactivando el suministro de agua.               | Identificador de zona asignada, estado operativo actual (activado/desactivado) y tiempo de riego.          | Encender el flujo de agua, apagar el flujo de agua y reportar si se encuentra actualmente regando.                            |


## 4. Características comunes y especialización

1. **Información en común** 
   * Identificadores
   * Estados
   * Historial

2. **Comportamientos en común**
   * Lecutra.
   * generar datos.


3. **¿Qué características cambian dependiendo del tipo de sensor?**
    * Unidad de medida.
    * Rangos y umbrales de referencia.
    * Lógica.

4. **¿Existe un concepto general que permita representar a todos los sensores?**
   Sí. Se puede abstratere la entidad general **Sensor**.

5. **¿Qué elementos podrían representar especializaciones de ese concepto?**
   Los elementos especializados que salen del concepto general Sensor.
    * SensorTemperatura
    * SensorHumedad
    * SensorHumedadSuelo

## 5. Relaciones entre objetos

### Objetos que colaboran e información compartida
- **Objetos que colaboran:** El SistemaRiego colabora directamente con el SensorHumedadSuelo.
- **Responsabilidades no duplicadas:** La clase Sensor centraliza los atributos y métodos comunes

### Relaciones de herencia y colaboración
- **Relaciones mediante herencia :**
   - `SensorTemperatura` Sensor.
   - `SensorHumedad`  Sensor.
   - `SensorHumedadSuelo` Sensor.
- **Relaciones mediante asociación:**
   - El `SistemaRiego` utilza un `SensorHumedadSuelo` para consultar su estado y tomar decisiones de riego.

### Justificación: ¿Por qué `SistemaRiego` no debería ser una subclase de `Sensor`?
1. **Diferencia de naturaleza y responsabilidad:** Un `Sensor` es un dispositivo de entrada/medición encargado de observar y registrar datos del entorno. Un `SistemaRiego` es un actuador de salida/control cuya función es modificar físicamente el entorno suministrando agua.
