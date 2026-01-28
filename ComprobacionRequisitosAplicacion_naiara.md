# Comprobación de Requisitos de Seguridad

## 1. Nivel de seguridad requerido por la aplicación
La aplicación consiste en dos scripts Python sencillos (`lavadero.py` y `main_app.py`) que simulan el funcionamiento de un túnel de lavado de coches mediante una máquina de estados en consola.  

Características clave que determinan el nivel de seguridad ASVS:

- Es una aplicación de **consola local**, ejecutada directamente en el equipo del desarrollador/estudiante (no hay servidor, no hay interfaz web, no hay API).
- No existe autenticación de usuarios, gestión de sesiones ni roles.
- No recibe entrada de fuentes externas no confiables: los parámetros (prelavado, secado, encerado) se pasan directamente en el código o como argumentos fijos en ejemplos de prueba. No hay lectura de stdin interactiva peligrosa, ni parámetros HTTP, ni ficheros subidos, ni conexiones a red.
- No maneja ni almacena datos sensibles (PII, financieros, médicos, contraseñas, claves, etc.). Solo acumula un valor numérico de ingresos simulados en memoria.
- No utiliza dependencias externas (solo librerías estándar de Python: ninguna instalación vía pip).
- No hay operaciones criptográficas, logging persistente, uploads/downloads, deserialización de datos no confiables, llamadas a comandos del sistema ni acceso a recursos del sistema de forma peligrosa.
- Las únicas "entradas" son booleanos controlados por el programador; no hay riesgo real de inyección, overflow, path traversal, etc.

Por todo ello, el nivel de seguridad **más adecuado** según OWASP ASVS es el **Nivel 1** (L1 – Opportunistic / Protección contra vulnerabilidades fáciles de descubrir y ataques automatizados básicos).

- **Nivel 2** ya requeriría controles que no aplican aquí (TLS obligatorio, logging de seguridad detallado, protección contra CSRF, gestión segura de secretos, etc.).
- **Nivel 3** es para aplicaciones críticas con requisitos de alta assurance (HSM, re-autenticación frecuente, auditoría estricta, etc.).

En resumen: **ASVS Nivel 1** es suficiente y realista para esta aplicación sin exposición ni valor real en juego.

## 2. Hoja de cálculo cumplimentada

He descargado la plantilla OWASP ASVS checklist for audits (versión 4.0.2) y la he renombrado como **ASVS-Checklist-Naiara.xlsx**.

Archivo subido en la tarea: [ASVS-Checklist-Naiara.ods](https://github.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/blob/main/archivos/ASVS-Checklist-naiara.ods)

He rellenado principalmente las secciones solicitadas en la actividad:

- **Input Validation** (hoja 5): varios requisitos marcados como "Valid" o "Non-valid" con comentarios y referencias al código.
- **Malicious Code** (hoja 10): la mayoría marcados como "Not Applicable" por ser una aplicación de Nivel 1 y sin carga dinámica de código, sin permisos excesivos, sin phone-home, etc.

En la pestaña **ASVS_Results** se observa:

- Cumplimiento total: **~1.09%** (3 de 276 criterios cumplidos).
- Única categoría con algo de cumplimiento: **Input Validation** (~11.11%).
- Todas las demás categorías a **0%**.

Esto es completamente esperable dada la simplicidad extrema de la aplicación.

**Capturas de pantalla:**
- Resultados globales y gráfico de telaraña:  
  ![Resultados globales](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img41.png)

- Sección Input Validation (detalle):  
  ![Input Validation](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img42.png)

- Sección Malicious Code (detalle):  
  ![Malicious Code](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img43.png)


## 3. Análisis del grado de cobertura

El porcentaje global de cumplimiento es extremadamente bajo (**1.09%**), con solo 3 criterios validados de 276 totales. La única categoría que muestra algo de cumplimiento es **Input Validation** (11.11%), mientras que el resto de secciones (Arquitectura, Autenticación, Sesiones, Acceso, Criptografía, Logging, etc.) están al 0%.

**Reflexión:**

Esta cobertura tan baja **no indica que la aplicación sea insegura**, sino que es **demasiado simple** para que la gran mayoría de los controles ASVS sean aplicables. ASVS está diseñado principalmente para aplicaciones web con exposición a internet, usuarios, sesiones, APIs, bases de datos, uploads, etc. — superficies de ataque que aquí simplemente no existen.

Lo positivo es que, en los pocos puntos que sí aplican (principalmente validación básica de parámetros booleanos y ausencia de deserialización o comandos peligrosos), la aplicación cumple razonablemente bien. No hay riesgo real de inyección, IDOR, deserialización insegura, etc., porque no hay entrada no confiable ni parsing de formatos complejos.

En conclusión: el bajo porcentaje refleja la naturaleza educativa y minimalista del proyecto, no una falta grave de seguridad. Para una aplicación real con usuarios y exposición, el porcentaje debería ser mucho más alto (idealmente apuntando a Nivel 2).

## 4. Herramientas automáticas que podrían ayudar

Aunque la aplicación es muy pequeña y no tiene dependencias externas ni código con patrones de alto riesgo, algunas herramientas automáticas que podrían usarse para verificar requisitos ASVS (especialmente en Input Validation, Malicious Code y malas prácticas generales en Python) son:

- **Bandit**: analizador estático de seguridad específico para Python. Detecta automáticamente usos inseguros de `pickle`, `subprocess` sin sanitizar, `os.system`, `eval`, hashlib débil, etc. Muy útil para validar contra inyecciones y deserialización insegura (5.5.x).
- **Semgrep**: motor de búsqueda de patrones con reglas ASVS integradas. Permite buscar código vulnerable (asignación masiva, comandos sin escape, etc.) y es muy potente para requisitos como 5.1.x, 5.3.x o 10.x.
- **Safety** o **pip-audit**: chequean vulnerabilidades conocidas en dependencias de PyPI. En este caso no aplicaría porque no hay requirements.txt ni paquetes externos.

No he ejecutado estas herramientas porque el código es mínimo, usa solo stdlib y no presenta patrones peligrosos evidentes. Sin embargo, serían las más recomendables para proyectos Python más complejos.

## 5. Valoración personal del OWASP ASVS

OWASP ASVS me parece un estándar **muy completo y valioso** para proyectos reales, especialmente aplicaciones web empresariales, APIs o sistemas con datos sensibles. Proporciona una lista estructurada, objetiva y actualizada de controles que ayudan a no olvidar aspectos críticos (autenticación, validación, logging, configuración segura, etc.).

**Ventajas**:
- Es independiente de tecnologías → se puede aplicar a Python, Java, .NET, etc.
- Tiene niveles progresivos (L1 básico, L2 estándar, L3 alto) que permiten adaptarlo al riesgo real.
- Incluye referencias útiles (CWE, NIST, Proactive Controls) que facilitan el aprendizaje.
- Sirve tanto para desarrollo (como checklist) como para auditorías/pentesting.

**Dificultades / limitaciones**:
- Para aplicaciones muy simples o educativas como esta (scripts locales sin red ni usuarios), se siente como **overkill** — la mayoría de requisitos no aplican y el porcentaje queda ridículamente bajo.
- Requiere tiempo y conocimiento para rellenar correctamente la hoja (sobre todo decidir "Not Applicable" vs "Non-valid").
- Está muy orientado a aplicaciones web → para apps de consola, desktop, IoT o embebidas, hay que interpretar muchos puntos con sentido común.

En definitiva: **excelente herramienta para proyectos serios**, pero en ejercicios académicos pequeños sirve más como ejercicio de reflexión que como verificación real de seguridad. Me ha ayudado a entender mejor qué vulnerabilidades "no existen" en mi app precisamente por su diseño minimalista.
