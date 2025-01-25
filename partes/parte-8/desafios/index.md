# Desafío 6: Emergencia en la Misión a Marte

## Objetivo
Gestionar una serie de emergencias durante una misión tripulada a Marte utilizando un equipo de Multi-agents en Flowise, cada uno con roles y herramientas específicas.

## Requisitos

La tripulación está compuesta por:


- Capitán (Alex Ramsey) (Supervisor): Coordina al equipo y toma las decisiones finales.
- Ingeniero (Zoe Tanaka): Responsable del mantenimiento y reparación de la nave. Puede usar la Calculadora para cálculos complejos.
- Médico (Dr. Ben Carter): Se encarga de la salud de la tripulación. Puede usar una retriever tool para acceder a historiales médicos.
- Encargado de Comunicaciones (Liam O'Connell): Gestiona las comunicaciones con la Tierra y mantiene un registro de eventos. Debe usar una custom tool para anotar en Google Sheets los eventos que ocurran.
- Científico de Superficie (Dr. Anya Sharma): Investiga fenómenos en Marte. Puede usar Serp API para investigar descubrimientos.

## Escenario

La nave se encuentra a mitad de camino hacia Marte cuando se presentan las siguientes emergencias:

1. Falla en el Soporte Vital:

Prompt para el Capitán: "El sistema de soporte vital está fallando. Los niveles de oxígeno están disminuyendo. Ingeniero, necesito un análisis de la situación y un plan de acción inmediato. Médico, evalúa la condición de la tripulación, con especial atención a aquellos con historiales médicos relevantes. Encargado de comunicaciones, registra los niveles de oxígeno y la condición de la tripulación en el registro."

2. Tormenta Solar:

Prompt para el Capitán: "Alerta de tormenta solar, necesitamos una ruta de desvío. Ingeniero, ayúdame a trazar un nuevo curso, usa la calculadora para estimar el consumo de combustible con la nueva ruta. Encargado de comunicaciones, informa a la Tierra sobre la situación y la nueva ruta."

3. Descubrimiento en Marte:

Prompt para el Capitán: "Hemos recibido imágenes que muestran una posible estructura artificial en la superficie de Marte. Científico de Superficie, investiga con Serp API y determina si es seguro acercarse o si representa un peligro. Necesito un informe completo. Encargado de comunicaciones registra toda la información relevante."

4. Miembro de la Tripulación Enfermo:

Prompt para el Capitán: "El miembro de la tripulación Liam O'Connell presenta síntomas preocupantes: tos seca, fiebre y alucinaciones auditivas. Médico, accede a los historiales médicos con la Retriever Tool y determina un posible diagnóstico y tratamiento. Necesito un plan de acción. Encargado de comunicaciones, registra los síntomas, el diagnóstico y el tratamiento en el registro de eventos."

5. Desviación de Trayectoria:

Prompt para el Capitán: "Nos hemos desviado de la trayectoria. Ingeniero, necesito tu ayuda para recalcular la ruta de regreso al curso original, usa la calculadora para determinar el ajuste de propulsión necesario y estimar el combustible adicional que se consumirá. Encargado de comunicaciones registra el evento, la causa, la nueva ruta y el consumo estimado de combustible en el registro."

## Recursos

[Historial médico de la tripulación](../../../pdf/historial.medico.pdf)