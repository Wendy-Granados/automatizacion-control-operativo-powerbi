# automatizacion-control-operativo-powerbi
Automatización de un proceso de control operativo mediante Power BI, utilizando transformación de datos, reglas de negocio y medidas DAX para detectar incidencias en declaraciones.

# Detección automatizada de declaraciones y arqueos de caja

## 📌 Descripción

Desarrollo de un dashboard en Power BI orientado al monitoreo y
detección de incidencias relacionadas con declaraciones de caja,
ventas y posibles arqueos.

El proyecto transforma un proceso de revisión manual de movimientos
transaccionales en un análisis automatizado que permite identificar
empleados y sucursales que requieren revisión.

---

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

Desarrollar una solución analítica en Power BI que permita identificar
automáticamente casos que requieren revisión y proporcionar el detalle
necesario para validar cada caso.

La documentación técnica del proyecto se encuentra en la carpeta
`docs/`.
