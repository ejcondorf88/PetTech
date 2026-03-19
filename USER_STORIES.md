# Historias de Usuario — PetTech MVP

---

# Épica 1: Gestión de Mascotas

## HU-01 - Registrar información básica de la mascota

- **Como** administrador
- **Quiero** registrar los datos básicos de una mascota
- **Para** almacenar su información en el sistema

### Criterios de Aceptación

```gherkin
Feature: Registro de información básica de mascota

  Scenario: Registro exitoso
    Given que el administrador del refugio completa los campos obligatorios
    When confirma el registro de la mascota
    Then el sistema almacena los datos de la mascota

  Scenario: Validación de campos obligatorios
    Given que el administrador deja vacíos campos obligatorios como la edad o la especie
    When intenta guardar la mascota
    Then el sistema muestra un mensaje de error destacando los campos faltantes
    And no crea el registro en la base de datos

  Scenario: Registro con especie no reconocida
    Given que el administrador ingresa una especie que no pertenece al listado permitido
    When intenta guardar la mascota
    Then el sistema rechaza el registro
    And muestra un mensaje indicando que la especie ingresada no es válida
```

### Story Points HU-01
- **Estimación:** 5 SP
- **Justificación:** Registro completo con múltiples validaciones de campos obligatorios, regla de especie permitida y almacenamiento en base de datos. Esfuerzo moderado-alto tanto en DEV como en QA.

---

## HU-02 – Registrar información de salud

- **Como** administrador
- **Quiero** registrar el estado de salud de la mascota
- **Para** tener control sobre vacunas y esterilización

### Criterios de Aceptación

```gherkin
Feature: Registro de información médica de la mascota

  Scenario: Registro de información de salud exitoso
    Given que el administrador ingresa datos de salud completos
    When guarda la información
    Then el sistema la almacena correctamente asociada a la mascota

  Scenario: Intento de registro sin seleccionar el estado de vacunación
    Given que el administrador deja el campo de historial de vacunas sin completar
    When intenta guardar la información de salud
    Then el sistema muestra un mensaje indicando que el historial de vacunación es un dato requerido
    And no almacena el registro incompleto

  Scenario: Registro de mascota ya esterilizada
    Given que el administrador marca la mascota como esterilizada
    When guarda la información de salud
    Then el sistema registra la condición de esterilización de la mascota
    And este dato queda disponible como referencia en el proceso de adopción
```

### Story Points HU-02
- **Estimación:** 3 SP
- **Justificación:** Funcionalidad acotada de registro médico con validaciones básicas. Baja complejidad técnica al ser una extensión del perfil de mascota ya existente.

---

## HU-03 - Subir fotos de la mascota

- **Como** administrador
- **Quiero** subir una fotografía de la mascota
- **Para** mejorar su visualización en la plataforma

### Criterios de Aceptación

```gherkin
Feature: Carga de fotografía de mascota

  Scenario: Carga de imagen válida
    Given que el administrador selecciona una imagen en formato JPG o PNG menor a 5 MB
    When la carga en el formulario
    Then el sistema la almacena en el proveedor externo y guarda la URL de referencia

  Scenario: Validación de formato de imagen incorrecto
    Given que el administrador intenta subir un archivo que no es una imagen como un PDF o DOCX
    When intenta procesar el registro
    Then el sistema notifica que el formato de archivo no es compatible
    And no almacena ningún registro

  Scenario: Validación de tamaño máximo de imagen
    Given que el administrador intenta subir una imagen que supera los 5 MB
    When intenta procesar la carga
    Then el sistema rechaza el archivo
    And muestra un mensaje indicando que el tamaño máximo permitido es de 5 MB
```

### Story Points HU-03
- **Estimación:** 3 SP
- **Justificación:** Funcionalidad complementaria de carga y validación de imágenes con integración a proveedor externo. Riesgo medio por dependencia de servicio de almacenamiento en la nube.

---

# Épica 2: Gestión de Familias

## HU-04 - Registrar información básica de familia

- **Como** familia adoptante
- **Quiero** registrar mi información personal
- **Para** crear mi perfil en el sistema

### Criterios de Aceptación

```gherkin
Feature: Registro de familia adoptante

  Scenario: Registro exitoso con información completa
    Given que la familia ingresa datos personales válidos y completos
    When completa el registro
    Then la información queda almacenada correctamente en el sistema

  Scenario: Validación de datos obligatorios
    Given que la familia omite datos personales obligatorios como el número de identificación o la edad
    When intenta registrarse
    Then el sistema rechaza el registro
    And muestra los campos faltantes

  Scenario: Validación de mayoría de edad
    Given que la familia ingresa una edad menor a 18 años
    When intenta completar el registro
    Then el sistema rechaza el registro
    And informa que el solicitante debe ser mayor de edad para adoptar
```

### Story Points HU-04
- **Estimación:** 5 SP
- **Justificación:** Registro de información personal con múltiples validaciones (campos obligatorios, regla de mayoría de edad, unicidad de cédula y correo) y almacenamiento en base de datos. Complejidad y esfuerzo similares a HU-01.

---

## HU-05 - Registrar condiciones del hogar y experiencia

- **Como** familia adoptante
- **Quiero** registrar las condiciones de mi hogar y mi experiencia con mascotas
- **Para** facilitar el proceso de evaluación para adopción

### Criterios de Aceptación

```gherkin
Feature: Registro de condiciones del hogar y experiencia

  Scenario: Registro de condiciones del hogar exitoso
    Given que la familia ingresa información completa del hogar y experiencia con mascotas
    When guarda los datos
    Then el sistema almacena la información correctamente asociada al perfil de la familia

  Scenario: Intento de registro sin campos obligatorios del hogar
    Given que la familia no selecciona el tipo de vivienda o no indica el tamaño del hogar
    When intenta guardar la información
    Then el sistema muestra un mensaje indicando que los campos de tipo de vivienda son obligatorios
    And no actualiza el perfil con información incompleta

  Scenario: Intento de registrar condiciones ya existentes
    Given que la familia ya tiene condiciones del hogar registradas en el sistema
    When intenta registrar nuevamente la misma información
    Then el sistema muestra una alerta indicando que ya existe un registro de condiciones del hogar
    And no permite duplicar el registro
```

### Story Points HU-05
- **Estimación:** 3 SP
- **Justificación:** Registro de condiciones del hogar con validaciones básicas de campos obligatorios y restricción de duplicidad. Funcionalidad específica y de baja complejidad técnica.

---

# Épica 3: Visualización de Mascotas

## HU-06 - Ver listado de mascotas

- **Como** familia adoptante
- **Quiero** ver las mascotas disponibles
- **Para** explorar opciones de adopción

### Criterios de Aceptación

```gherkin
Feature: Visualización del listado de mascotas disponibles

  Scenario: Visualización de mascotas disponibles
    Given que existen mascotas registradas con estado disponible en el sistema
    When se consulta el listado de mascotas
    Then el sistema muestra únicamente las mascotas disponibles para adopción

  Scenario: Exclusión de mascotas no disponibles
    Given que existen mascotas con diferentes estados de disponibilidad en el sistema
    When se consulta el listado de mascotas
    Then el sistema incluye únicamente las mascotas con estado disponible
    And excluye las mascotas en proceso de adopción o ya adoptadas

  Scenario: Información relevante en el listado
    Given que existen mascotas disponibles para adopción
    When se consulta el listado de mascotas
    Then cada mascota presenta nombre, especie, raza, edad y fotografía para su evaluación básica

  Scenario: Ausencia de mascotas disponibles
    Given que no existen mascotas con estado disponible en el sistema
    When se consulta el listado de mascotas
    Then el sistema informa que no hay mascotas disponibles actualmente
```

### Story Points HU-06
- **Estimación:** 3 SP
- **Justificación:** Visualización de listado con filtro por estado de disponibilidad. Consulta directa a base de datos sin lógica compleja. Baja complejidad técnica.

---

## HU-07 – Ver detalle de mascota

- **Como** familia adoptante
- **Quiero** ver el detalle de una mascota
- **Para** conocer sus características antes de solicitar adopción

### Criterios de Aceptación

```gherkin
Feature: Visualización del detalle de una mascota

  Scenario: Visualización del detalle completo de una mascota
    Given que la familia selecciona una mascota del listado
    When accede a su detalle
    Then el sistema muestra la información completa de la mascota incluyendo datos generales, fotografías e historial de salud

  Scenario: Visualización de fotografías asociadas
    Given que la familia accede al detalle de una mascota
    When la mascota tiene imágenes asociadas en el sistema
    Then el sistema muestra las fotos de la mascota

  Scenario: Mascota sin fotografías registradas
    Given que la familia accede al detalle de una mascota
    When la mascota no tiene imágenes asociadas
    Then el sistema muestra una imagen de reemplazo predeterminada en lugar de las fotos

  Scenario: Visualización del historial básico de salud
    Given que la familia accede al detalle de una mascota
    When la mascota tiene información de salud registrada
    Then el sistema muestra su historial básico de salud
```

### Story Points HU-07
- **Estimación:** 5 SP
- **Justificación:** Consulta que consolida información de múltiples tablas (mascotas, salud, fotos) mediante JOIN. Requiere manejo de casos nulos y respuesta de reemplazo para fotos. Mayor alcance y complejidad que el listado simple.

---

# Épica 4: Solicitud y Matching

## HU-08 – Solicitar adopción

- **Como** familia adoptante
- **Quiero** enviar una solicitud de adopción
- **Para** expresar interés en una mascota

### Criterios de Aceptación

```gherkin
Feature: Solicitud de adopción

  Scenario: Solicitud de adopción exitosa
    Given que la familia adoptante tiene su perfil completo con condiciones del hogar registradas
    And la mascota seleccionada se encuentra en estado disponible
    When la familia confirma el envío de la solicitud de adopción
    Then el sistema registra la solicitud con estado pendiente
    And el sistema actualiza el estado de la mascota a en proceso de adopción

  Scenario: La solicitud no se aprueba automáticamente
    Given que la familia adoptante ha enviado una solicitud de adopción
    When la solicitud es registrada en el sistema
    Then el estado de la solicitud queda como pendiente
    And la solicitud no debe estar en estado aprobado ni rechazado

  Scenario: Familia sin condiciones del hogar registradas
    Given que la familia no ha completado el registro de condiciones del hogar
    When intenta enviar una solicitud de adopción
    Then el sistema bloquea la solicitud
    And muestra un mensaje indicando que debe completar el perfil del hogar primero
```

### Story Points HU-08
- **Estimación:** 3 SP
- **Justificación:** Flujo de registro de solicitud con validación de precondiciones y actualización atómica de estado de mascota. Lógica directa con riesgo de condición de carrera ya contemplado como riesgo técnico.

---

## HU-09 – Consultar detalle de solicitud de adopción

- **Como** administrador
- **Quiero** visualizar la información de la familia y la mascota en una solicitud
- **Para** analizar la viabilidad de la adopción

### Criterios de Aceptación

```gherkin
Feature: Consulta de solicitud de adopción

  Scenario: Visualización de información completa de la solicitud
    Given que existe una solicitud de adopción registrada en el sistema
    When el administrador accede a su detalle
    Then el sistema muestra la información consolidada de la familia y de la mascota asociadas a esa solicitud

  Scenario: Intento de acceso a una solicitud inexistente
    Given que un administrador intenta acceder al detalle de una solicitud mediante un identificador que no existe en el sistema
    When el sistema procesa la petición
    Then el sistema muestra un mensaje informativo: "La solicitud consultada no existe o ha sido eliminada permanentemente"
```

### Story Points HU-09
- **Estimación:** 3 SP
- **Justificación:** Flujo exclusivo de visualización sin lógica de negocio compleja. Requiere JOIN entre tablas y manejo de respuesta 404 para ID inexistente.

---

## HU-10 – Registrar decisión sobre solicitud

- **Como** administrador
- **Quiero** aprobar o rechazar una solicitud de adopción
- **Para** controlar el proceso de asignación de mascotas

### Criterios de Aceptación

```gherkin
Feature: Gestión de decisión de solicitud

  Scenario: Aprobación de solicitud pendiente
    Given que el administrador ha revisado una solicitud con estado pendiente
    When decide aprobarla
    Then el sistema registra la solicitud como aprobada
    And la mascota permanece en estado en proceso de adopción

  Scenario: Rechazo de solicitud pendiente
    Given que el administrador ha revisado una solicitud con estado pendiente
    When decide rechazarla
    Then el sistema registra la solicitud como rechazada
    And el sistema devuelve el estado de la mascota a disponible para recibir nuevas solicitudes

  Scenario: Intento de cambiar una decisión ya aprobada
    Given que existe una solicitud con estado aprobada
    When el administrador intenta modificar su estado
    Then el sistema rechaza la operación
    And retorna un mensaje indicando que la decisión ya fue registrada y no puede modificarse
```

### Story Points HU-10
- **Estimación:** 5 SP
- **Justificación:** Lógica de cambio de estado con actualización en cascada sobre dos tablas (solicitud y mascota). Incluye manejo de conflictos (cambio de estado ya aprobado) y reversión de estado en caso de rechazo. Mayor complejidad transaccional.

---

## HU-11 – Sugerir alternativa de adopción

- **Como** administrador
- **Quiero** sugerir una mascota alternativa a una familia
- **Para** mejorar la probabilidad de éxito en la adopción

### Criterios de Aceptación

```gherkin
Feature: Sugerencia de mascota alternativa

  Scenario: Registro de sugerencia exitoso
    Given que el administrador identifica una mascota disponible como mejor opción para la familia
    When asigna esa mascota como sugerida a la solicitud de la familia
    Then el sistema registra la sugerencia de adopción correctamente

  Scenario: Intento de sugerir una mascota no disponible
    Given que el administrador selecciona una mascota que ya está en proceso de adopción
    When registra la sugerencia para esa familia
    Then el sistema invalida la operación
    And muestra un mensaje informativo: "La mascota seleccionada no se encuentra disponible para adopción"
```

### Story Points HU-11
- **Estimación:** 3 SP
- **Justificación:** Registro de sugerencia en tabla propia con validación de disponibilidad de la mascota sugerida. Sin lógica de negocio compleja ni tablas adyacentes adicionales.

---

# Épica 5: Confirmación de Adopción

## HU-12 – Confirmar adopción

- **Como** administrador
- **Quiero** confirmar una adopción
- **Para** registrar que la mascota ha encontrado una familia

### Criterios de Aceptación

```gherkin
Feature: Confirmación de adopción

  Scenario: Cambio de estado a adopción exitosa
    Given que existe una solicitud de adopción con estado aprobada
    When el administrador confirma la adopción
    Then el estado de la solicitud cambia a adopción exitosa

  Scenario: Registro de la fecha de adopción
    Given que el administrador confirma una adopción
    When la adopción es registrada como exitosa
    Then el sistema registra automáticamente la fecha de la adopción

  Scenario: Vinculación de mascota con familia
    Given que una adopción ha sido confirmada
    When el sistema registra la adopción como exitosa
    Then la mascota queda vinculada a la familia adoptante en el registro de adopciones
    And el estado de la mascota se actualiza a adoptada

  Scenario: Intento de confirmar una solicitud que no está aprobada
    Given que existe una solicitud con estado pendiente o rechazada
    When el administrador intenta confirmar la adopción
    Then el sistema rechaza la operación
    And muestra un mensaje indicando que solo se pueden confirmar solicitudes aprobadas
```

### Story Points HU-12
- **Estimación:** 3 SP
- **Justificación:** Cambio de estado de solicitud, registro de fecha de adopción y actualización del estado de la mascota. Lógica transaccional directa sobre tablas relacionadas.

---

## HU-13 – Visualizar adopciones realizadas

- **Como** administrador
- **Quiero** ver el historial de adopciones
- **Para** hacer seguimiento de adopciones exitosas

### Criterios de Aceptación

```gherkin
Feature: Visualización de adopciones realizadas

  Scenario: Visualización del historial general de adopciones
    Given que el usuario se encuentra en la sección de adopciones realizadas
    When consulta el historial
    Then el sistema muestra únicamente los registros con estado adopción exitosa

  Scenario: Visualización paginada del historial
    Given que existen más de 10 adopciones registradas como exitosas
    When el usuario consulta el historial
    Then el sistema muestra los resultados agrupados de a 10 registros por página

  Scenario: Filtrado de adopciones por usuario específico
    Given que el usuario visualiza el historial de adopciones
    When solicita ver únicamente las adopciones asociadas a su cuenta
    Then el sistema muestra únicamente las adopciones vinculadas a ese usuario

  Scenario: Usuario sin adopciones registradas
    Given que el usuario no tiene adopciones con estado exitoso
    When consulta el historial de adopciones
    Then el sistema retorna una respuesta vacía con un mensaje informativo
```

### Story Points HU-13
- **Estimación:** 5 SP
- **Justificación:** Lógica de filtro por estado y por usuario más implementación de paginación. Complejidad media-alta en la capa de consulta con múltiples combinaciones de filtros posibles.

---

# Épica 6: Calendario de Vacunación

## HU-14 – Generar calendario de vacunas

- **Como** administrador
- **Quiero** generar un calendario inicial de vacunación
- **Para** orientar al adoptante en el cuidado de la mascota

### Criterios de Aceptación

```gherkin
Feature: Generación de calendario de vacunación

  Scenario: Generación del calendario considerando las características de la mascota
    Given que una adopción ha sido confirmada exitosamente
    When el sistema genera el calendario de vacunación
    Then el sistema considera la especie, edad e historial de vacunación de la mascota para construir el calendario

  Scenario: Generación de fechas sugeridas de vacunación para cachorro
    Given que la mascota adoptada es un cachorro sin historial de vacunas
    When el sistema calcula el protocolo de vacunación
    Then el sistema establece fechas sugeridas con refuerzos cada 3 a 4 semanas según el protocolo por especie

  Scenario: Generación de fechas sugeridas para mascota adulta
    Given que la mascota adoptada es un adulto con vacunas parciales
    When el sistema calcula el protocolo de vacunación
    Then el sistema establece únicamente las vacunas faltantes con refuerzos anuales o trianuales
    And no duplica las vacunas que ya constan en el historial de salud

  Scenario: Asociación del calendario a la adopción confirmada
    Given que el calendario de vacunación ha sido generado
    When el sistema finaliza el proceso
    Then el calendario queda asociado al identificador de la adopción confirmada
```

### Story Points HU-14
- **Estimación:** 8 SP
- **Justificación:** Lógica de negocio compleja: protocolo de vacunación diferenciado por especie (perro/gato) y etapa de vida (cachorro/adulto), con lectura del historial para evitar duplicados y disparo automático al confirmar la adopción. Alto esfuerzo en DEV y QA.

---

## HU-15 – Consultar calendario de vacunación

- **Como** familia adoptante
- **Quiero** ver el calendario de vacunación
- **Para** cumplir con los cuidados de salud de la mascota

### Criterios de Aceptación

```gherkin
Feature: Consulta del calendario de vacunación

  Scenario: Consulta exitosa del calendario tras adopción confirmada
    Given que la familia adoptante ha completado una adopción exitosa
    When accede a la información de su mascota
    Then el sistema muestra el calendario de vacunación con las fechas sugeridas correspondientes

  Scenario: Consulta de calendario para una adopción no confirmada
    Given que la familia adoptante tiene una solicitud de adopción en estado pendiente
    And la mascota no ha sido entregada a la familia
    When consulta la información de la mascota
    Then el sistema no muestra el calendario de vacunación
    And informa que existen procesos pendientes por aprobar

  Scenario: Acceso a calendario sin tener una adopción registrada
    Given que el usuario no tiene ninguna adopción registrada en el sistema
    When intenta acceder al calendario de vacunación
    Then el sistema deniega el acceso
    And muestra un mensaje informando que no hay adopciones asociadas a su cuenta
```

### Story Points HU-15
- **Estimación:** 3 SP
- **Justificación:** Flujo exclusivo de lectura del calendario generado. Requiere validación del estado de la adopción y manejo de accesos denegados. Sin lógica de generación (delegada a HU-14).