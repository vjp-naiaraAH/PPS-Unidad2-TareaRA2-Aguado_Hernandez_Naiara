# Trazado de la Vulnerabilidad CVE-2024-0204 en GoAnywhere MFT

# Índice
- [Objetivos del Trazado](#objetivos-del-trazado) 🎯  
- [Punto de Partida: Artículo de INCIBE](#punto-de-partida-articulo-de-incibe) 🇪🇸  
  - [¿Qué es INCIBE?](#que-es-incibe) ℹ️  
  - [Productos afectados](#productos-afectados) ⚠️  
- [Información del Fabricante (Fortra)](#informacion-del-fabricante-fortra) 🏭  
  - [¿Qué es Fortra?](#que-es-fortra)  
  - [Descripción detallada](#descripcion-detallada) 🧩  
  - [Impacto real](#impacto-real) 💥  
  - [Solución recomendada](#solucion-recomendada) 🛡️  
- [Información sobre la Vulnerabilidad (CVE y NVD)](#informacion-sobre-la-vulnerabilidad-cve-y-nvd) 📚  
  - [Página CVE.org](#pagina-cveorg)  
  - [Página NVD (NIST)](#pagina-nvd-nist)  
- [Criticidad y Vector CVSS](#criticidad-y-vector-cvss) 🔥  
- [Debilidades Explotadas (CWE)](#debilidades-explotadas-cwe) 🧠  
  - [¿Qué es CWE?](#que-es-cwe)  
  - [CWE-425](#cwe-425)  
- [Patrones de Ataque Relacionados (CAPEC)](#patrones-de-ataque-relacionados-capec) 🎯  
  - [CAPEC-143](#capec-143)  
  - [CAPEC-127](#capec-127)  
- [Registro CVE en JSON](#registro-cve-en-json) 🗂️  
- [Conclusiones / Resumen Final](#conclusiones--resumen-final) 📝
---

## Objetivos del Trazado
- Conocer las listas y organismos clave de ciberseguridad (CVE, NVD, CWE, CAPEC, CVSS).
- Realizar el trazado completo de una vulnerabilidad desde el aviso inicial hasta las debilidades y patrones de ataque.
- Analizar el riesgo, consecuencias y posibles mitigaciones.

Esta vulnerabilidad permite a un atacante no autenticado crear un usuario administrador en GoAnywhere MFT, con grave riesgo de compromiso total del sistema.

---

## Punto de Partida: Artículo de INCIBE

**¿Qué es INCIBE?**  
INCIBE (Instituto Nacional de Ciberseguridad) es la agencia española encargada de la ciberseguridad, dependiente del Ministerio de Asuntos Económicos y Transformación Digital. Publica avisos, alertas y recomendaciones para empresas y ciudadanos.

El artículo de INCIBE [enlace](https://www.incibe.es/empresas/avisos/vulnerabilidad-critica-de-omision-de-autenticacion-en-goanywhere-mft-de-fortra) nos alerta de una vulnerabilidad **crítica** de omisión de autenticación en GoAnywhere MFT (Fortra). Un atacante puede crear un usuario administrador sin credenciales previas accediendo directamente al portal de administración.

No se menciona el CVE en el aviso, por lo que el siguiente paso lógico es consultar la referencia principal: el advisory del fabricante.

**Productos afectados**: versiones 6.0.1 a 7.4.1 (incluidas).  
**Severidad**: Crítica (nivel 5/5 según clasificación de INCIBE).  
**Riesgo principal**: Posible toma de control completa del servidor MFT, robo masivo de archivos transferidos o implantación de malware.

![Pantallazo principal de la página INCIBE](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img1.png)

![Pantallazo del listado de referencias en INCIBE](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img2.png)

---

## Información del Fabricante (Fortra)

**¿Qué es Fortra?**  
Fortra (anteriormente HelpSystems) es una empresa estadounidense especializada en software de automatización, transferencia segura de archivos (MFT) y ciberseguridad. GoAnywhere MFT es su solución principal para gestión segura de archivos.

En el advisory oficial [enlace](https://www.fortra.com/security/advisories/product-security/fi-2024-001) se identifica la vulnerabilidad como **CVE-2024-0204**.

**Descripción detallada**:  
Existe un endpoint de configuración inicial (`InitialAccountSetup.xhtml`) que no requiere autenticación después de la instalación. Un atacante puede enviarle una petición POST con datos de un nuevo usuario administrador y tomar el control del sistema sin credenciales previas.

**Debilidad principal**: CWE-425 (Direct Request / Forced Browsing).  
**CVSS**: 9.8 Crítico → vector CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H.

**Impacto real**:  
- Creación de cuentas privilegiadas → acceso total a transferencias de archivos, credenciales almacenadas y configuración.  
- Posible uso en campañas de ransomware (GoAnywhere ya fue explotado masivamente por Cl0p en 2023).  
- Robo de datos sensibles de empresas que usan MFT para intercambio con proveedores.

**Solución recomendada**:  
- Actualizar inmediatamente a 7.4.1 o superior.  
- En instalaciones existentes sin actualizar: eliminar o renombrar el archivo `InitialAccountSetup.xhtml`.

![Página principal de Fortra con gravedad y vulnerabilidad](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img3.png)
![Página de Fortra con notas y vulnerabilidades](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img4.png)
![Notas de vulnerabilidad de Fortra y referencias](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img5.png)

---

## Información sobre la Vulnerabilidad (CVE y NVD)

### Página CVE.org

**¿Qué es CVE.org?**  
CVE (Common Vulnerabilities and Exposures) es un catálogo público mantenido por MITRE que asigna identificadores únicos (CVE-XXXX-XXXXX) a vulnerabilidades conocidas. Sirve como estándar global para referenciar fallos de seguridad.

En [https://www.cve.org/CVERecord?id=CVE-2024-0204](https://www.cve.org/CVERecord?id=CVE-2024-0204) encontramos el registro oficial: descripción básica, estado (Published) y enlaces a fuentes primarias (Fortra, NVD).  
Desde aquí se puede descargar el **CVE Record** en JSON.

![Página de CVE.org con el botón "View JSON" destacado](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img7.png)

### Página NVD (NIST)

**¿Qué es NVD?**  
NVD (National Vulnerability Database) es la base de datos oficial del NIST (EE.UU.) que enriquece los CVE con análisis detallado: puntuación CVSS, CWE asociada, referencias y métricas de impacto.

En [https://nvd.nist.gov/vuln/detail/CVE-2024-0204](https://nvd.nist.gov/vuln/detail/CVE-2024-0204) se confirma:  
- CVSS 9.8 Crítico  
- CWE-425  
- Vector completo y descripción ampliada  
- Esto indica que la aplicación expone funcionalidades críticas sin control de acceso adecuado.

![Pantallazo principal de la página NVD](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img8.png)
![Descripción, métricas y base de puntuación en NVD](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img9.png)  
![Enumeración de vulnerabilidades en NVD](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img10.png)

---

## Criticidad y Vector CVSS

**Puntuación CVSS**: 9.8 / 10 (Crítico)  
**Vector CVSS:3.1**:
CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H

**Desglose de métricas**:

| Métrica              | Valor       | Significado                                                                 |
|----------------------|-------------|-----------------------------------------------------------------------------|
| Attack Vector (AV)   | Network     | Explotable remotamente por internet sin necesidad de acceso local.         |
| Attack Complexity (AC) | Low       | No requiere condiciones especiales ni race conditions.                     |
| Privileges Required (PR) | None    | No necesita credenciales previas.                                           |
| User Interaction (UI) | None       | No requiere que la víctima haga clic o interactúe.                         |
| Scope (S)            | Unchanged   | El impacto no sale del componente vulnerable.                               |
| Confidentiality (C)  | High        | Posible exposición total de datos transferidos y almacenados.              |
| Integrity (I)        | High        | Posible modificación de archivos, configuración o inserción de backdoors.  |
| Availability (A)     | High        | Posible interrupción del servicio MFT (DoS o eliminación de datos).        |

Esta combinación hace que la vulnerabilidad sea extremadamente peligrosa en entornos expuestos a internet.

---

## Debilidades Explotadas (CWE)

**¿Qué es CWE?**  
CWE (Common Weakness Enumeration) es un catálogo mantenido por MITRE que clasifica tipos de debilidades de software (fallos de diseño o implementación) que pueden dar lugar a vulnerabilidades.

**CWE-425** [enlace](https://cwe.mitre.org/data/definitions/425.html): *Direct Request* o *Forced Browsing*.  
La aplicación permite acceder directamente a URLs internas o de configuración que deberían estar protegidas por autenticación o sesión activa.

**Consecuencias típicas**:  
- Acceso no autorizado a paneles administrativos  
- Creación/eliminación de usuarios  
- Exposición o modificación de datos sensibles  
- Escalada a control total del sistema

**Mitigaciones recomendadas**:
- **Arquitectura y diseño**: Requerir autenticación en todas las rutas administrativas desde el primer acceso.
- **Implementación**: Eliminar o deshabilitar endpoints de setup una vez completada la instalación inicial.
- **Buenas prácticas**: Usar frameworks con controles de autorización integrados (Spring Security, Django auth, etc.).

**Relaciones**: Hijo de CWE-862 (Missing Authorization).  
**CAPECs relacionados**: CAPEC-127, CAPEC-143, CAPEC-144, entre otros.

![Página principal de CWE-425](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img11.png)
![Posibles mitigaciones en CWE-425](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img12.png)
![Relaciones en CWE-425](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img13.png)

---

## Patrones de Ataque Relacionados (CAPEC)

**¿Qué es CAPEC?**  
CAPEC (Common Attack Pattern Enumeration and Classification) es otro catálogo de MITRE que describe patrones comunes de ataque que explotan debilidades como las CWE.

- **CAPEC-143** [enlace](https://capec.mitre.org/data/definitions/143.html): *Detect Unpublicized Web Pages*  
  Consiste en adivinar o enumerar URLs no enlazadas públicamente (como páginas de setup o admin ocultas).  
  **Requisitos**: Acceso a la aplicación expuesta (muy bajo).  
  **Consecuencias**: Bypass de autenticación, acceso a funcionalidades críticas.  
  **Mitigación**: No exponer endpoints innecesarios, usar autenticación en profundidad.

![Página principal de CAPEC-143](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img14.png)

- **CAPEC-127** [enlace](https://capec.mitre.org/data/definitions/127.html): *Directory Indexing*  
  Similar, pero enfocado en listar contenidos de directorios mal configurados.  
  **Habilidades**: Muy bajas (herramientas automáticas o fuzzing simple).  
  **Mitigación**: Deshabilitar directory listing en el servidor web.

![Página principal de CAPEC-127](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img15.png)

---

## Registro CVE en JSON

**¿Qué es el CVE Record en JSON?**  
Es la representación estructurada (formato JSON) del registro CVE, mantenida por MITRE/CNA. Se usa para alimentación automática de herramientas de seguridad (scanners, SIEM, bases de datos internas).

Enlace: [https://cveawg.mitre.org/api/cve/CVE-2024-0204](https://cveawg.mitre.org/api/cve/CVE-2024-0204)

Contiene:
- Metadatos (fecha de publicación, estado)
- Descripción oficial
- Referencias
- Weaknesses (CWE-425)
- Posible CPE (plataformas afectadas)

![Vista del JSON con secciones clave](https://raw.githubusercontent.com/vjp-naiaraAH/PPS-Unidad2-TareaRA2-Aguado_Hernandez_Naiara/refs/heads/main/images/img16.png)

---

## Conclusiones / Resumen Final
- **CVE-2024-0204** es una vulnerabilidad **crítica** (CVSS 9.8) de omisión de autenticación que permite takeover administrativo remoto sin credenciales.
- **Cadena de trazado**: INCIBE → Fortra → CVE.org → NVD → CWE-425 → CAPEC-143/127 → JSON estructurado.
- **Riesgo real**: Muy alto en entornos expuestos (historial de explotación masiva en GoAnywhere por Cl0p).
- **Lección principal**: Los endpoints de configuración inicial deben desactivarse inmediatamente tras la instalación.
- **Recomendaciones prácticas**:
  - Aplicar parche o workaround lo antes posible.
  - Monitorear accesos al puerto 8000/8443 (puertos por defecto de GoAnywhere).
  - Implementar WAF con reglas anti-forced browsing.
  - Revisar si se expone innecesariamente a internet.

Este trazado ilustra perfectamente cómo las diferentes listas y bases de datos están interconectadas para un análisis completo de cualquier vulnerabilidad.