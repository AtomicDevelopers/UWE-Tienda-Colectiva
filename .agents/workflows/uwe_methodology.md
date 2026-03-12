---
description: Metodología UWE para Diagramas de Modelado Web con PlantUML
---
# Metodología UWE (UML-based Web Engineering)

Esta skill proporciona las directrices y convenciones para crear modelos web basados en la metodología UWE utilizando repositorios con PlantUML. La metodología divide la aplicación web en diferentes modelos con preocupaciones separadas (Separación de Intereses).

## Estilo General PlantUML (Consistente en todos los modelos)
Todos los archivos `.puml` deben iniciar con las siguientes configuraciones visuales base:
```plantuml
skinparam shadow false
skinparam packageStyle rectangle
skinparam roundCorner 15
skinparam class {
  BackgroundColor #FEFECE
  BorderColor #A80036
  ArrowColor #A80036
}
```
*(Nota: En algunos modelos el estilo de la clase puede aplicar a `usecase`, `activity` o `rectangle` dependiendo de los elementos UML base utilizados en el diagrama).*

---

## 1. Modelo de Requisitos (Requirements Model)
El modelo de requisitos captura las funcionalidades principales del sistema web basado en sus usuarios mediante Casos de Uso y Flujos de Procesos (Actividades).

### Casos de Uso (Use Cases)
- **Stereotypes de UWE**:
  - `<<browsing>>`: Operaciones de solo lectura donde el usuario busca o navega en la información.
  - `<<processing>>`: Operaciones que modifican el estado de la aplicación o datos persistentes.
- **Relaciones**:
  - `<<include>>`: Operaciones necesarias para cumplir otra.
  - `<<extends>>`: Operaciones opcionales o dependientes del estado (con un caso base).

Ejemplo:
```plantuml
usecase "Listar Ciudades" as ListCities <<browsing>>
usecase "Crear Ciudad" as CreateCity <<processing>>
```

### Actividades (Activities / Process Flow)
Define el detalle de un caso de uso particular utilizando Diagramas de Actividad.
- **Stereotypes de UWE para acciones**:
  - `<<userAction>>`: Indica que el usuario interactúa expresamente validando o requiriendo información.
  - `<<systemAction>>`: Ejecutado internamente por el sistema (validar, guardar, procesar).
  - `<<displayAction>>`: Presentación de elementos (formularios, listados) hacia el usuario.

---

## 2. Modelo de Contenido (Content Model)
Modela el dominio del problema mediante un Diagrama de Clases UML tradicional. Identifica todos los objetos de información accesibles a través de la aplicación Web.
- **Sin estereotipos especiales**.
- Se usan Clases con Atributos convencionales.
- Sirve como base estructurada donde posteriormente todas las propiedades tendrán su reflejo en la navegación y vistas.

---

## 3. Modelo de Navegación (Navigation Model)
Define la estructura hipertextual estableciendo qué información puede ser accedida y cómo los usuarios navegan entre la información.

- **Stereotypes de UWE**:
  - `<<navigationClass>>`: Un nodo de navegación con contenido enfocado (Ej. Detalle de Contacto, Panel Principal).
  - `<<menu>>`: Una lista estructurada de opciones de navegación.
  - `<<index>>`: Un listado del mismo tipo de objetos (Ej. Lista de Contactos).
  - `<<query>>`: Acción de búsqueda de contenidos.
  - `<<processClass>>`: Un puente directo de navegación que dispara un proceso o workflow específico (ej. "ContactoNuevo").
- **Enlaces (Links)**:
  - `<<navigationLink>>`: Flujo hipertextual natural.
  - `<<processLink>>`: Flujo dirigido obligatorio hacia/desde un proceso de negocio.
- **Tagged Values**: 
  - `{tag=isDashboard}` o `{isHome}`: Se utiliza para identificar el punto de entrada principal.

---

## 4. Modelo de Presentación (Presentation Model)
Describe cómo el componente hipertextual o de flujo de proceso se muestra al usuario. Detalla los formularios, las tablas y la interfaz de usuario.
- Elemento base: **`rectangle`**
- **Stereotypes de UWE** en clases abstractas o rectángulos agrupados:
  - `<<presentationPage>>`: Representa una página completa en pantalla.
  - `<<presentationGroup>>`: Agrupación de elementos visuales (ej. Encabezado, Lista, Panel).
  - `<<inputForm>>`: Un formulario completo de entrada de datos.
  - `<<confirmation>>` y `<<validationError>>`: Pantallas / mensajes de alertas o diálogos del sistema.
- **Stereotypes de UWE para datos (Variables UI)**:
  - `<<text>>`: Información textual fija o de la db.
  - `<<textInput>>`: Campo corto o largo de texto.
  - `<<selection>>`: Un dropdown o combobox.
  - `<<button>>`: Botón interactivo (Ej. Submit).
  - `<<image>>` o `<<fileUpload>>`: Manejo de archivos visuales o cargas.
- Flujos: Las flechas `-->` indican interacciones de pantalla (ej. De un Botón de Búsqueda hacia una Página con la Lista de Resultados).

---

## 5. Modelo de Procesos (Process Model)
Detalla las interacciones complejas transaccionales y de operaciones de negocio (workflows).
- **Process Structure Model (Clases)**:
  - Muestra jerarquías de clases transaccionales de negocio.
  - Heredan operaciones (ej. "ContactProcessing") a clases específicas (ej. "ContactUpdate").
- **Process Flow Model (Actividades)**:
  - Un refinamiento muy atómico del Modelo de Requisitos de Actividades que toma todas las clases del diagrama estructural transaccional y las transforma en una máquina de estados o diagrama de actividad:
  - Uso intensivo de `<<userAction>>` (ej. Proveer datos de formulario) y `<<systemAction>>` (ej. ValidateData o SaveContact).

## Generación de Artefactos UWE
Al generar un diagrama con base en esta Skill:
1. Revisa el tipo de diagrama (Requisitos, Contenido, Navegación, Presentación, o Proceso).
2. Usa **únicamente los estereotipos aprobados listados aquí**.
3. Asegura que los temas de colores mantengan la sintaxis unificada de este documento.
4. Identifica cláramente al actor (ej. Admin, ShopManager, Customer).
