# Automatización control operativo PowerBI
Automatización de un proceso de control operativo mediante Power BI, utilizando transformación de datos, reglas de negocio y medidas DAX para detectar incidencias en declaraciones.

# Detección automatizada de declaraciones y arqueos de caja

## 📌 Descripción

Desarrollo de una solución de Business Intelligence en Power BI para automatizar la identificación de incidencias relacionadas con declaraciones de caja, ventas y posibles arqueos, reduciendo la necesidad de revisión manual de grandes volúmenes de movimientos transaccionales.


## 🎯 Objetivo

Automatizar la identificación de:

- Declaraciones pendientes.
- Declaraciones en cero después de registrar ventas.
- Posibles arqueos.
- Movimientos que requieren revisión operativa.

El objetivo principal es facilitar la supervisión diaria y fortalecer
la detección preventiva de posibles anomalías operativas.

---

## 💼 Problema de negocio

El proceso original requería revisar manualmente grandes volúmenes
de movimientos en Excel para identificar si cada empleado había
realizado correctamente su declaración al finalizar su turno.

La revisión manual dificultaba:

- Identificar rápidamente empleados pendientes.
- Relacionar ventas con declaraciones.
- Detectar secuencias anómalas de movimientos.
- Dar seguimiento a múltiples sucursales y empleados.

---

## 💡 Solución propuesta

Se desarrolló un modelo de datos en Power BI compuesto por tres tablas:

### Registro Transacc
Contiene los movimientos operativos:

- Fecha
- Tipo de transacción
- ID de empleado
- Sucursal
- Ventas
- Ret/Dec
- Bloque

### Empleados

Catálogo de empleados:

- ID de empleado
- Nombre

### Tiendas

Catálogo de sucursales:

- Código de tienda
- Nombre de tienda
- Sector

Las tablas se relacionan mediante los identificadores de empleado
y sucursal.

---

## 🔎 Lógica de detección

El análisis considera la secuencia de movimientos de cada empleado
por sucursal y período operativo.

### Declaración pendiente

Se identifica cuando existen ventas mayores a cero y no se encuentra
una declaración posterior correspondiente.

### Declaración en cero

Se identifica cuando existen ventas mayores a cero y posteriormente
se registra una declaración de caja con Ret/Dec igual a cero.

### Posible arqueo

Se identifica cuando existe una declaración de caja en cero y
posteriormente aparece una declaración con un importe mayor a cero.

> Las reglas son utilizadas como indicadores de revisión y no como
> evidencia definitiva de una irregularidad.

---

## 📊 Indicadores principales

El dashboard contempla indicadores para:

- Empleados con declaraciones pendientes.
- Declaraciones en cero.
- Posibles arqueos.
- Número de casos identificados.
- Detalle de empleados y sucursales involucrados.

---

## 🛠️ Herramientas

- Power BI
- DAX
- Power Query
- Modelado de datos
- Análisis de datos
- Visualización de información

---

## 📈 Resultado

La solución permite pasar de una revisión manual de movimientos a
un esquema de monitoreo basado en reglas de negocio.

El usuario puede identificar los casos relevantes y posteriormente
consultar el detalle de los movimientos asociados para realizar una
revisión más profunda.

---

## 🔮 Mejoras futuras

- Automatización de la actualización de datos.
- Alertas automáticas para incidencias.
- Seguimiento histórico por empleado y sucursal.
- Indicadores de reincidencia.
- Clasificación de niveles de riesgo.
- Integración con otras fuentes de información.


# 01. Contexto y objetivo

## Contexto

El proceso requería revisar manualmente los movimientos realizados por empleados en distintas sucursales para determinar si las ventas habían sido declaradas correctamente al finalizar el turno.

Debido al volumen de información, la revisión visual dificultaba identificar oportunamente declaraciones pendientes, declaraciones en cero y secuencias de movimientos que requerían una revisión adicional.

Situación anterior
Revisión manual de movimientos.
Identificación visual de casos.
Uso de colores para clasificar incidencias.
Dificultad para revisar múltiples empleados y sucursales.
Riesgo de omitir registros durante la revisión.

El proceso analizado contiene información transaccional de múltiples
sucursales y empleados.

La revisión de las declaraciones requería analizar secuencias de
movimientos para determinar si las ventas realizadas durante el
período operativo habían sido declaradas correctamente.

## Problema

El análisis manual presentaba dificultades para:

1. Revisar grandes volúmenes de registros.
2. Identificar empleados pendientes.
3. Relacionar ventas y declaraciones.
4. Detectar patrones que requerían revisión.
5. Mantener un seguimiento consistente entre sucursales.

## Objetivo

Diseñar e implementar un dashboard en Power BI capaz de analizar automáticamente los movimientos transaccionales y detectar casos que requieren revisión, permitiendo consultar los indicadores generales y posteriormente desglosar cada caso hasta el empleado, sucursal y movimientos involucrados

# 02. Proceso de análisis de datos

FUENTES DE DATOS
       ↓
EXTRACCIÓN
       ↓
PERFILAMIENTO
       ↓
LIMPIEZA Y TRANSFORMACIÓN
       ↓
VALIDACIÓN
       ↓
MODELADO
       ↓
REGLAS DE NEGOCIO
       ↓
DAX / MÉTRICAS
       ↓
VISUALIZACIÓN
       ↓
VALIDACIÓN DE RESULTADOS
       ↓
INSIGHTS / DECISIONES

# 03 Extracción de datos

| Tabla               | Propósito                               |
| ------------------- | --------------------------------------- |
| `Registro Transacc` | Contiene los movimientos operativos     |
| `Empleados`         | Catálogo e identificación de empleados  |
| `Tiendas`           | Catálogo e identificación de sucursales |

# 04 Perfilamiento inicial de los datos
Se realizó un perfilamiento inicial para identificar problemas de calidad de datos antes de aplicar transformaciones. Las modificaciones se realizaron únicamente cuando existía una justificación basada en la estructura del dato y su función dentro del proceso.

# 05 Limpieza y transformación

## 5.1 Tipos de datos
Se revisaron los tipos de datos para evitar errores de conversión y garantizar que las operaciones y medidas DAX utilizaran valores compatibles.
Verificar que cada campo tenga el tipo correcto:

Durante la carga inicial se detectaron inconsistencias de tipos de datos que impedían la correcta actualización del modelo. Se revisaron y corrigieron los campos afectados antes de continuar con el modelado.

| Campo            | Tipo esperado                    |
| ---------------- | -------------------------------- |
| Fecha            | Fecha                            |
| Bloque           | Texto                            |
| ID Empleados     | Número/texto según su naturaleza |
| Suc              | Texto/identificador              |
| Ventas           | Decimal/moneda                   |
| Ret/Dec          | Decimal/moneda                   |
| Tipo Transacción | Texto                            |

## 5.2 Valores nulos y vacíos
Se identificaron valores nulos y vacíos y se evaluó su impacto de acuerdo con la función de cada campo. No se eliminaron registros únicamente por contener valores nulos, debido a que un valor faltante puede representar una condición válida del proceso o afectar la trazabilidad del movimiento.

###Criterio
¿Tiene nulo?
      ↓
¿Es válido que esté vacío?
   ↙       ↘
 Sí         No
 ↓           ↓
Conservar   Investigar/corregir

Por ejemplo, si un campo no aplica para determinado tipo de transacción, un cero o vacío puede ser parte de la lógica del negocio.

## 5.3 Estandarización de nombres

Se estandarizaron los nombres de columnas para facilitar su identificación y uso en Power Query y DAX.

Por ejemplo:

Nombres inconsistentes
        ↓
Nombres descriptivos y consistentes

## 5.4 Valores inconsistentes
Se revisaron categorías de texto para detectar diferencias de escritura, espacios, mayúsculas/minúsculas y otras inconsistencias que pudieran afectar filtros, agrupaciones y reglas de negocio.

# 06 Requerimientos de negocio

| Incidencia            | Condición                                          |
| --------------------- | -------------------------------------------------- |
| Declaración pendiente | Ventas > 0 y no existe declaración posterior       |
| Declaración en cero   | Ventas > 0 y declaración posterior con Ret/Dec = 0 |
| Posible arqueo        | Declaración = 0 seguida de declaración > 0         |

## Identificadores utilizados

Sucursal → Registro Transacc[Suc] → Tiendas[Código]

Empleado → Registro Transacc[ID Empleados] → Empleados[Emp]

# 03 Modelo de datos

<img width="672" height="462" alt="Captura de pantalla 2026-08-24 145657" src="https://github.com/user-attachments/assets/efdf6126-6b11-4ee8-b782-c93db018e64d" />
Registro Transacc funciona como tabla de hechos, mientras que Tiendas y Empleados funcionan como tablas de referencia/dimensión.

Registro Transacc - Tabla principal de movimientos.

Empleados - Dimensión de empleados.

Tiendas - Dimensión de sucursales.

Las relaciones permiten complementar los movimientos transaccionales con información descriptiva de empleados y sucursales sin duplicar innecesariamente los catálogos.

# 04 Lógica de negocio
## Reglas de negocio

### Declaración correcta

Se considera correcta cuando existen ventas y posteriormente se registra una declaración con un importe mayor a cero, de acuerdo con las reglas operativas establecidas.

### Declaración en cero

Se identifica cuando el empleado registra ventas mayores a cero y posteriormente realiza una Decl. Caja cuyo valor en Ret/Dec es igual a cero.

VENTAS > 0
     ↓
DECL. CAJA
RET/DEC = $0
     ↓
DECLARACIÓN EN CERO

###Declaración pendiente

Se identifica cuando existen ventas mayores a cero y no se encuentra una declaración de caja correspondiente posterior al movimiento operativo.

###Posible arqueo

Se identifica una secuencia en la que inicialmente aparece una declaración de caja con valor cero y posteriormente una declaración con importe mayor a cero, utilizada como indicador para revisión.

DECL. CAJA = $0
       ↓
DECL. CAJA > $0
       ↓
POSIBLE ARQUEO
VENTAS > 0
     ↓
     ↓
SIN DECL. CAJA
     ↓
DECLARACIÓN PENDIENTE

Estas reglas constituyen indicadores analíticos para priorizar revisiones y no determinan por sí mismas la existencia de una irregularidad.

### Secuencia esperada

VENTAS
   ↓
VENTAS
   ↓
VENTAS
   ↓
DECL. CAJA

### Declaración correcta

Ventas > 0
       ↓
Decl. Caja
Ret/Dec > 0

### Declaración en cero

Ventas > 0
       ↓
Decl. Caja
Ret/Dec = 0

Declaración pendiente
Ventas > 0
       ↓
       ↓
SIN DECL. CAJA
Posible arqueo
Decl. Caja
Ret/Dec = 0
       ↓
Decl. Caja
Ret/Dec > 0

# 05 Medidas DAX


# 06 Validación
Los resultados automatizados fueron contrastados con registros individuales y con la lógica de revisión utilizada previamente, verificando que los casos identificados correspondieran con las condiciones de negocio establecidas.

Y una tabla:

Caso	Esperado	Power BI	Estado
Ventas + declaración > 0	Correcta	Correcta	✅
Ventas + declaración = 0	Incidencia	Detectada	✅
Ventas sin declaración	Pendiente	Detectada	✅
Declaración 0 → declaración > 0	Revisión	Detectada	✅

Cuando terminemos las medidas, llenamos esta tabla con casos reales anonimizados.
Para validar las reglas se realizaron comprobaciones manuales sobre
registros individuales.

Se verificó:

- Identificación correcta del empleado.
- Identificación correcta de la sucursal.
- Secuencia temporal de movimientos.
- Existencia de ventas antes de la declaración.
- Importe registrado en Ret/Dec.
- Correspondencia entre los resultados de las medidas y los registros
  originales.

| Caso                            | Resultado esperado  | Resultado Power BI | Validación |
| ------------------------------- | ------------------- | ------------------ | ---------- |
| Ventas + declaración > 0        | No incidencia       | No incidencia      | ✓          |
| Ventas + declaración = 0        | Declaración en cero | Detectado          | ✓          |
| Ventas sin declaración          | Pendiente           | Detectado          | ✓          |
| Declaración 0 → declaración > 0 | Posible arqueo      | Detectado          | ✓          |

#07 Dashboard

Por ejemplo:

KPIs
Empleados sin declarar.
Declaraciones en cero.
Posibles arqueos.
Total de incidencias.
Filtros
Fecha.
Tienda.
Sector.
Empleado.
Tipo de incidencia.
Detalle

La selección de un indicador permite profundizar en los casos identificados y consultar los movimientos asociados para facilitar la validación operativa.

# 08. Resultados y mejoras

### 8.1 Resultados e impacto

Primero terminamos el dashboard y entonces calculamos:

registros analizados;
tiendas;
empleados;
casos detectados;
tiempo de revisión anterior;
tiempo con la solución;
reducción potencial del tiempo;
porcentaje de casos que requirieron revisión.

La automatización permitió transformar una revisión manual de aproximadamente X horas diarias en un proceso de monitoreo mediante indicadores y filtros interactivos.

### Mejoras futuras

Automatización de actualización.
Alertas automáticas.
Histórico de incidencias.
Seguimiento de reincidencia.
Score de riesgo.
Segmentación por sucursal.
Notificaciones.
Integración con otras fuentes.
Automatización completa del ETL.

# 09. Limitaciones

La clasificación depende de la calidad de los datos de origen.
Las reglas identifican indicadores de riesgo, no confirman irregularidades.
La interpretación de los movimientos depende de la correcta clasificación de bloques.
Se requiere validar las reglas ante cambios en el proceso operativo.

# 10. Tecnologías

Power BI
Power Query
DAX
Modelado dimensional
ETL
Data Quality
Data Visualization
Business Intelligence
