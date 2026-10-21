# Fase 2. Diseño orientado a objetos y UML

## 1. Diseño de clases

A continuación se presenta la propuesta de clases para el sistema del invernadero:

### Tabla de clases, atributos y métodos

| Clase | Atributos propuestos | Tipo de dato | Métodos propuestos | Responsabilidad |
| :--- | :--- | :--- | :--- | :--- |
| **Sensor** | - id<br>- ubicacion<br>- activo<br>- ultimaMedicion | String<br>String<br>boolean<br>double | + Sensor(id, ubicacion)<br>+ getId(): String<br>+ getUbicacion(): String<br>+ isActivo(): boolean<br>+ setActivo(activo): void<br>+ getUltimaMedicion(): double<br>+ registrarMedicion(valor): void<br>+ evaluarMedicion(): String | Es la clase base. Guarda los datos generales de cualquier sensor (ID, ubicación, estado activo y última lectura) y define la función de evaluación. |
| **SensorTemperatura** | - limiteInferior<br>- limiteSuperior | double<br>double | + SensorTemperatura(id, ubicacion, limInf, limSup)<br>+ getLimiteInferior(): double<br>+ getLimiteSuperior(): double<br>+ evaluarMedicion(): String | Mide la temperatura en grados Celsius y revisa si el ambiente está frío, bien o caliente según sus límites. |
| **SensorHumedad** | - umbralSeco<br>- umbralSaturado | double<br>double | + SensorHumedad(id, ubicacion, umbralSeco, umbralSat)<br>+ getUmbralSeco(): double<br>+ getUmbralSaturado(): double<br>+ evaluarMedicion(): String | Mide la humedad del aire en porcentaje e indica si está seco, adecuado o saturado. |
| **SensorHumedadSuelo** | - umbralMinimo | double | + SensorHumedadSuelo(id, ubicacion, umbralMinimo)<br>+ getUmbralMinimo(): double<br>+ evaluarMedicion(): String<br>+ requiereRiegoUrgente(): boolean | Mide la humedad de la tierra en porcentaje para saber si necesita agua o si está suficientemente húmeda. |
| **SistemaRiego** | - idZona<br>- encendido | String<br>boolean | + SistemaRiego(idZona)<br>+ getIdZona(): String<br>+ isEncendido(): boolean<br>+ activarRiego(): void<br>+ desactivarRiego(): void<br>+ evaluarYRegar(sensorSuelo: SensorHumedadSuelo): void | Representa la bomba de agua. Controla su encendido o apagado según lo que marque el sensor de tierra. |

### Análisis de atributos y métodos

- **Encapsulación:** Todos los atributos son de tipo privado (private) para evitar que desde otra clase se cambien valores directos sin pasar por métodos.
- **Constructores:** Reciben los datos esenciales para inicializar el objeto. La superclase pide el identificador y la ubicación, mientras que las subclases piden eso más sus propios límites o umbrales.
- **Getters y métodos de acceso:** Para consultar información desde fuera se usan métodos como getUltimaMedicion o isActivo. Para cambiar la lectura se usa registrarMedicion.
- **Clase general contra especializaciones:** La información común a todo sensor vive en Sensor. Los umbrales específicos y la lógica para decir si un dato es alto o bajo se quedan en cada subclase.

---

## 2. Jerarquía de clases

El diseño se organiza de la siguiente manera:

1. **Jerarquía de Sensores (Herencia):**
   - **Sensor** es la superclase padre.
   - **SensorTemperatura**, **SensorHumedad** y **SensorHumedadSuelo** heredan de Sensor porque todas son sensores. Comparten la estructura básica y solo cambian la forma en que evalúan su lectura.

2. **Sistema de Riego (Colaboración):**
   - **SistemaRiego** no hereda de Sensor. Es una clase aparte que colabora con SensorHumedadSuelo pidiéndole su lectura para decidir si prende o apaga el agua.

---

## 3. Diagrama UML inicial

## Diagrama UML inicial

![Diagrama UML inicial](uml-inicial.png)

---

## 4. Justificación del diseño

### 1. ¿Por qué propusiste esas clases?
Porque cubren lo que pide el problema: los tres tipos de mediciones (temperatura, humedad de aire, humedad de suelo), una clase base para no repetir código y la clase para controlar el riego.

### 2. ¿Cuál es la responsabilidad principal de cada una?
- **Sensor:** Guardar los datos comunes (ID, ubicación, estado, mediciones) y dar la estructura base.
- **SensorTemperatura:** Evaluar si la temperatura está baja, en rango o alta.
- **SensorHumedad:** Determinar si el aire está seco, bien o saturado.
- **SensorHumedadSuelo:** Evaluar la humedad de la tierra y avisar si ocupa riego.
- **SistemaRiego:** Encender o apagar el riego según lo que marque el suelo.

### 3. ¿Por qué determinados atributos serán privados?
Para aplicar encapsulamiento y evitar que desde fuera (como en la clase Main) se cambien valores de forma directa, como modificar la última lectura sin pasar por un método.

### 4. ¿Qué información proporcionarás mediante los constructores?
Al crear un Sensor se pide el identificador y la ubicación. En las subclases se reciben esos mismos datos (para pasarlos a la superclase mediante super) junto con los umbrales específicos de cada uno. En SistemaRiego solo se pide el identificador de la zona.

### 5. ¿Cuál será la superclase?
Sensor.

### 6. ¿Qué clases serán subclases?
SensorTemperatura, SensorHumedad y SensorHumedadSuelo.

### 7. ¿Por qué existe una relación "es un" entre ellas?
Porque un sensor de temperatura, uno de humedad de aire y uno de suelo son tipos de sensores. Todos guardan identificador, ubicación y registran lecturas.

### 8. ¿Qué información o comportamiento decidiste colocar en la superclase?
Los atributos id, ubicacion, activo, ultimaMedicion y los métodos comunes como registrarMedicion, los métodos de consulta y la versión base de evaluarMedicion.

### 9. ¿Qué elementos permanecerán en las subclases?
Los umbrales o límites específicos de cada variable física y la implementación de evaluarMedicion adaptada a cada tipo de lectura.

### 10. ¿Por qué SistemaRiego no pertenece a la jerarquía de sensores?
Porque no es un sensor; es un actuador que suministra agua. Si heredara de Sensor, tendría atributos innecesarios como ultimaMedicion. Solo necesita comunicarse con el sensor de suelo.

### 11. ¿Qué decisiones tomaste para evitar duplicar responsabilidades?
Poner los atributos comunes en la superclase Sensor para no escribirlos en cada clase hija, y dejar que la tarea de regar recaiga en SistemaRiego y no en los sensores.