Documentación de Arquitectura y Procesos: Estandarización de Itinerarios

1. Contexto y Objetivos del Proyecto
Petru DMC es un operador mayorista de viajes enfocado en el mercado español, cuyo modelo de negocio se basa en la personalización: aproximadamente el 90% de los itinerarios son diseñados a medida.
Actualmente, la generación de itinerarios se realiza de forma manual mediante hojas de cálculo y redacción libre, lo que genera:
Falta de estandarización entre vendedores
Inconsistencias en el formato del PDF final
Errores en fechas, servicios y alojamientos
Alto tiempo operativo
Objetivo del proyecto:
Estandarizar la generación de itinerarios
Reducir el trabajo manual
Minimizar errores operativos
Mantener flexibilidad para viajes personalizados

2. Análisis de Procesos (AS-IS vs. TO-BE)
Proceso Actual (AS-IS)
Construcción manual del itinerario por vendedor
Uso de Excel sin estructura unificada
Redacción libre día por día
Selección manual de servicios
PDFs con formato inconsistente
Problemas:
Falta de control y trazabilidad
Variabilidad en la calidad
Duplicación de esfuerzo
Alto riesgo de error
Adicionalmente, la información del cliente se captura de forma desestructurada (emails, mensajes), sin un proceso formal de estructuración.

Proceso Propuesto (TO-BE)
Uso de datos estructurados
Construcción del itinerario por día con inputs controlados
Selección de servicios desde catálogo centralizado
Uso de IA para:
Transformar información desestructurada en datos estructurados
Generar texto descriptivo
Generación automática de PDF con plantilla estándar
Mejoras:
Consistencia en outputs
Reducción de tiempos
Menor error humano
Mayor control de información
Mejor personalización

3. Arquitectura del Sistema Propuesta
Se propone una arquitectura simple, escalable y orientada a la operación, incorporando IA como capa de asistencia.
Componentes
0. Captura e interpretación de información
Permite transformar información desestructurada del cliente en datos estructurados.
Flujo:
Input: texto libre (email, mensaje, notas)
IA extrae:
intereses
estilo de viaje
ritmo
nivel de lujo
Output: datos estructurados reutilizables

1. Capa de entrada
Interfaz para carga y edición de datos.
Carga de cliente y viaje
Validación de datos
Edición de preferencias
Selección mediante listas controladas

2. Base de datos
Almacenamiento estructurado.
Modelo relacional simplificado
Separación catálogo / operación
Persistencia de preferencias
Fuente única de verdad

3. Lógica de negocio
Procesa y organiza la información.
Validación de datos
Construcción del itinerario
Relación entre entidades
Funcionalidad clave:
Sugerencias automáticas de servicios
Matching entre:
intereses del cliente
tags del catálogo

4. Motor de generación de texto
IA orientada exclusivamente a redacción.
Genera descripciones por día
Usa:
datos estructurados
servicios seleccionados
preferencias
Restricción:
No toma decisiones de negocio

5. Capa de presentación
Generación del output final.
Uso de plantilla estándar
Inserción dinámica
Exportación automática a PDF

Enfoque del sistema
Flujo iterativo:
Información desestructurada → Datos estructurados → Recomendaciones → Itinerario → Texto → Ajustes

4. Definición del Esquema de Datos
El sistema separa datos estructurados de la presentación y actúa como base para IA y generación de itinerarios.

Cliente
nombre_cliente
mercado_origen
tipo_grupo

Preferencias del Cliente
(Pueden ser manuales o extraídas por IA)
estilo_viaje
intereses
ritmo_viaje
nivel_lujo
Uso:
Personalización
Recomendaciones
Generación de contenido

Viaje
titulo_viaje
fecha_inicio / fecha_fin
ciudad_origen
num_pasajeros
tipo_viaje
regimen_general
cliente_id
Relación: cliente 1:N viajes

Días (Itinerario)
numero_dia
fecha
origen
destino
texto_manual (opcional)

Servicios por Día
servicio_id
orden
Tipos:
excursión
traslado
vuelo
hotel

Alojamiento
destino
noches
hotel_nombre
categoria
tipo_habitacion

Bloques adicionales
servicios_incluidos
no_incluye
opcionales
notas

5. Diseño de Base de Datos
Base relacional implementada en Google Sheets.
Tablas principales
clientes
id
nombre_cliente
mercado_origen
tipo_grupo
estilo_viaje
intereses
ritmo_viaje
nivel_lujo
viajes
id
cliente_id
titulo
fecha_inicio
fecha_fin
ciudad_origen
num_pasajeros
tipo_viaje
regimen_general
dias
id
viaje_id
numero_dia
fecha
origen
destino
texto_manual
servicios_catalogo
id
nombre
tipo
destino
descripcion_ia
tags
dia_servicios
id
dia_id
servicio_id
orden
alojamientos
id
viaje_id
destino
noches
hotel_nombre
categoria
tipo_habitacion

Tablas complementarias
servicios_incluidos (id, viaje_id, descripcion)
no_incluye (id, viaje_id, descripcion)
opcionales (id, viaje_id, nombre, precio, destino)
notas (id, viaje_id, descripcion)

Uso dentro del sistema
Almacena datos estructurados
Soporta recomendaciones (intereses ↔ tags)
Alimenta generación de contenido
Permite trazabilidad

Relaciones clave
clientes → viajes (1:N)
viajes → dias (1:N)
dias → dia_servicios (1:N)
servicios_catalogo → dia_servicios (1:N)
viajes → alojamientos (1:N)

6. Características del Diseño
Separación entre datos y presentación
Separación cliente / viaje
Separación catálogo / operación
Uso de IDs únicos
Modelo relacional simple
Integración con IA (input + output)
Personalización basada en preferencias
Sugerencias automáticas mediante matching semántico 

