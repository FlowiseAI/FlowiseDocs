# Desafío 4: Gerente de Reclutamiento

## Objetivo
Crear un agente capaz de evaluar candidatos según requisitos específicos y gestionar su información en Google Sheets.

## Funcionalidades
- Evaluación de requisitos
- Recopilación de información de contacto **(solo si se cumplen los requisitos)**
- Integración con Google Sheets **(para almacenar la información de los candidatos que cumplen los requisitos)**

## Casos de Prueba
Se proporcionarán múltiples prompts y casos para evaluar el funcionamiento del agente.

## Requisitos Técnicos
- Integración con Google Sheets
- Capacidad de procesamiento de lenguaje natural
- Sistema de evaluación de criterios
- Gestión de datos de contacto

![Esquema](../../../gitbook/assets/partes/parte6/desafio6.png)

## Requisitos Específicos del Puesto (Electricista)

Estos son los requisitos específicos que el agente debe poder evaluar para el puesto de **Electricista**:

- **Formación y Certificaciones (Excluyente):**
    - **Formación Profesional (FP) en Electricidad o equivalente:** Necesario. Se valorará formación específica en instalaciones de baja tensión, automatismos, etc.

- **Experiencia Laboral (Excluyente):**
    - **Mínimo de 3 años de experiencia demostrable como electricista en instalaciones residenciales y/o comerciales:** Se debe poder verificar la experiencia a través de referencias o certificados de trabajo.

- **Otros (Deseable):**
    - Disponibilidad para trabajar en horarios flexibles, incluyendo fines de semana y festivos si fuese necesario.
    - Carnet de conducir.

## Datos a Recopilar y Enviar a Google Sheets (Solo si se cumplen los requisitos)

**El agente solo debe recopilar y almacenar en Google Sheets los siguientes datos de los candidatos que cumplan con todos los requisitos excluyentes:**

-   **Nombre:** Nombre del candidato.
-   **Apellido:** Apellido del candidato.
-   **Correo Electrónico:** Dirección de correo electrónico del candidato.
-   **Teléfono:** Número de teléfono del candidato.

**Nota:** Si un candidato no cumple con los requisitos excluyentes, no se debe recopilar ni almacenar su información de contacto en Google Sheets.