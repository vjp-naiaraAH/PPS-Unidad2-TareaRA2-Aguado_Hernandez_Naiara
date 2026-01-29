# Reflexión sobre los riesgos de las aplicaciones

Durante esta unidad hemos trabajado con aplicaciones que presentan vulnerabilidades reales o intencionadas, como **GoAnywhere MFT** (en el trazado de vulnerabilidad del Apartado 1) y una aplicación de laboratorio vulnerable (utilizada en el Apartado 3 para aplicar el estándar OWASP ASVS). Estas prácticas me han permitido entender no solo cómo se explotan fallos de seguridad, sino también por qué algunos riesgos son tolerables en entornos de pruebas y absolutamente inaceptables en producción.

---

### Principales riesgos identificados en la aplicación analizada
En el análisis del Apartado 3, al evaluar los requisitos ASVS (principalmente Nivel 1 y partes del Nivel 2), detectamos fallos críticos que se alinean directamente con varias categorías del **OWASP Top 10** (2021):

- **A01:2021 - Broken Access Control**: La aplicación permitía accesos no autorizados a funciones administrativas simplemente modificando parámetros en la URL o cookies.
- **A03:2021 - Injection**: Falta total de parametrización en consultas a base de datos y ejecución de comandos del sistema, similar a la vulnerabilidad CVE que trazamos en GoAnywhere (donde un atacante podía ejecutar código arbitrario mediante inyección en parámetros de archivo).
- **A02:2021 - Cryptographic Failures**: Transmisión de datos sensibles (credenciales, tokens de sesión) en HTTP plano, sin TLS, y almacenamiento de contraseñas sin hash o con algoritmos débiles.
- **A07:2021 - Identification and Authentication Failures**: Ausencia de MFA, políticas de contraseñas débiles, sesiones persistentes sin timeout y cookies sin atributos Secure, HttpOnly ni SameSite=Strict.
- **A05:2021 - Security Misconfiguration**: Exposición de información del servidor en cabeceras y páginas de error, directorios listables y configuraciones por defecto inseguras.

Estos fallos convierten la aplicación en un blanco fácil: un atacante con conocimientos básicos podría escalar privilegios, robar datos de usuarios o ejecutar código remoto (RCE). En un entorno de pruebas como DVWA o similar, esto es educativo; en producción, sería desastroso.
---

### Comparación con aplicaciones de alto riesgo: ¿son los mismos riesgos?

Aunque las vulnerabilidades técnicas subyacentes son muy parecidas (casi todas derivan de malas prácticas de desarrollo), el **nivel de riesgo real** cambia radicalmente según el contexto de la aplicación:

1. **Aplicaciones bancarias / financieras** (ej. CaixaBank, BBVA, Revolut):
   - Exigen **ASVS Nivel 3** (el máximo) y cumplimiento de PCI-DSS, PSD2 y RGPD.
   - Un fallo de inyección o broken access control podría permitir **transferencias fraudulentas masivas**, blanqueo de capitales o robo de identidad a escala.
   - Impacto: pérdidas económicas directas (millones de euros), multas regulatorias enormes (hasta 4% de facturación global por RGPD) y pérdida irreversible de confianza de los clientes.
   - Controles adicionales: monitoreo en tiempo real de transacciones, behavioral biometrics, WAF con ML, auditorías externas obligatorias cada pocos meses.

2. **Aplicaciones de salud / sanitarias** (ej. Mi Salud, historiales clínicos en hospitales, apps de telemedicina):
   - Nivel ASVS 3 + normativas como RGPD + LOPDGDD + ISO 27001.
   - Exposición de datos de salud (diagnósticos, tratamientos, historial genético) → posibles consecuencias graves: chantaje, discriminación (seguros, empleo), estigma social o incluso daños físicos si se manipulan datos (ej. alterar prescripciones).
   - Riesgo diferencial: no solo económico, sino **impacto en la salud y privacidad íntima** de las personas.

3. **Redes sociales y plataformas de gran escala** (ej. Instagram, TikTok, X/Twitter):
   - Nivel ASVS 2 en la mayoría de funcionalidades, Nivel 3 en auth, pagos y datos privados.
   - Vulnerabilidades comunes: account takeover, data scraping masivo, XSS/CSRF, broken auth.
   - Impacto a escala: difusión de desinformación, campañas de acoso coordinado, manipulación de elecciones, robo de datos de millones de usuarios para phishing o venta en dark web.
   - Riesgos específicos: algoritmos que amplifican contenido dañino, privacidad de datos comportamentales (perfiles psicológicos inferidos), y exposición a deepfakes o doxxing.
---

### Conclusión personal

En mi opinión, esta tarea ha sido una de las más reveladoras del curso. Ver cómo una aplicación aparentemente “simple” puede tener decenas de fallos graves me ha hecho darme cuenta de que **la seguridad no es un añadido opcional**, sino una responsabilidad ética y profesional fundamental.

He aprendido que:
- El nivel ASVS debe elegirse desde la fase de requisitos, en función del **valor de los activos** (datos, dinero, reputación) y del **contexto de uso**.
- Vulnerabilidades que en un laboratorio parecen “divertidas” de explotar (como SQL Injection básica), en producción pueden destruir vidas, empresas o incluso influir en sociedades enteras.
- Como futura desarrolladora, mi meta es aplicar **Security by Design** y **Shift Left Security**: integrar controles de seguridad desde el primer commit, usar ASVS como checklist en cada sprint, y nunca asumir que “ya lo arreglaremos después”.

En resumen, los riesgos técnicos pueden ser similares en todas las aplicaciones, pero su **magnitud y consecuencias** dependen directamente de las características del negocio y de los usuarios. Ignorar esto no es solo un error técnico: es poner en peligro a personas reales.
