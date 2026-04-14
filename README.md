# ⚡ Voltage Glitching (Fault Injection) - Knowledge Base

Este repositorio contiene una guía técnica y recursos sobre la inyección de fallas por voltaje (**Voltage Glitching**), una técnica de hardware hacking utilizada para evadir protecciones de seguridad en sistemas embebidos.

## 📌 Introducción

El **Voltage Glitching** consiste en manipular deliberadamente el suministro de energía de un microcontrolador (MCU) o procesador en intervalos de micro o nanosegundos. El objetivo es provocar un error en la lógica digital que permita saltar instrucciones críticas (`JNE`, `JZ`, etc.) o corromper datos en tránsito, eludiendo controles de seguridad como verificaciones de contraseñas o firmas digitales.

## 🎯 ¿Cómo Funciona el Ataque?

El ataque busca crear una **inestabilidad temporal en los circuitos lógicos del chip**:

- **Instrucciones saltadas:** Si el glitch ocurre en el momento exacto de una decisión crítica (ej. validación de contraseña), el procesador puede "saltarse" a la sección de código de éxito sin verificar el valor ingresado.
- **Bypass de protección de lectura:** Desactiva el bit de protección de lectura (ROP) en microcontroladores, permitiendo extraer el firmware completo.
- **Corrupción de datos:** Altera datos en memoria durante operaciones criptográficas, filtrando claves secretas.

## 🛠️ Herramientas Comunes

### Hardware:
- **[ChipWhisperer](https://newae.com)** - Estándar de la industria para investigación de ataques de canal lateral e inyección de fallas
- **Pico Glitcher** - Alternativa de bajo costo basada en Raspberry Pi Pico con MOSFET
- **Bus Pirate 5** - Herramienta multipropósito con capacidades de glitching de energía
- **Transistores MOSFET** - Para cortocircuito controlado a tierra
- **Osciloscopio** - Para visualizar la precisión del glitch

### Software:
- **Python** - Automatización del timing de ataques
- **Jupyter Notebooks** - Análisis de datos de falla

## 🚀 Metodología del Ataque

1. **Identificación:** Localizar el pin `VCC` del procesador y capacitores de desacoplo
2. **Preparación:** Remover capacitores que suavicen el glitch (opcional pero recomendado)
3. **Sincronización (Trigger):** Identificar eventos (flanco de UART, consumo de energía específico) para disparar el glitch
4. **Brute-forcing:** Probar variaciones de:
   - `Offset`: Tiempo desde trigger hasta glitch
   - `Width`: Duración del pulso de bajo voltaje
5. **Explotación:** Confirmar si el sistema saltó la verificación de seguridad

## 📂 Escenarios de Uso

- [x] **Bypass de Checksums** - Saltarse verificación de firma de firmware
- [x] **Dump de Memoria** - Desactivar bits de protección de lectura
- [x] **Escalada de Privilegios** - Forzar estados de administrador en consolas o IoT
- [x] **Extracción de Claves** - Obtener semillas (seeds) y PINs de billeteras hardware

## 📋 Tipos de Ataques de Fallos Técnicos

| Método | Complejidad | Objetivos Típicos |
|--------|------------|------------------|
| **Fallo de Voltaje** | Baja | Dispositivos IoT, microcontroladores |
| **Fallo del Reloj** | Media | Consolas, sistemas embebidos |
| **Fallos Electromagnéticos (EM)** | Alta | Tarjetas inteligentes, módulos criptográficos |
| **Fallos Térmicos** | Baja | Dispositivos de consumo |

## 🔴 Ejemplos Reales Famosos

- **Consolas de Videojuegos:** Modding de Xbox 360 y PlayStation 3 para ejecutar código no firmado
- **Tesla:** Hackeo del sistema de infoentretenimiento y Autopilot para activar funciones premium sin pagar
- **Apple AirTags:** Bypass de protección del chip nRF52832 para clonar dispositivos
- **Billeteras de Criptomonedas:** Extracción de semillas y PINs de Trezor mediante inyección de fallas en autenticación

## 🛡️ Contramedidas y Defensas

Los diseñadores de hardware implementan varias estrategias defensivas:

- **Detectores de Brownout:** Sensores que reinician el chip ante caídas inusuales de voltaje
- **Lógica Redundante:** Ejecutar comprobaciones críticas dos veces en diferentes momentos
- **Retrasos Aleatorios (Jitter):** Añadir código "basura" o esperas aleatorias para impedir predicción del timing
- **Monitores de Voltaje y Reloj:** Detección de interrupciones maliciosas basada en hardware
- **Diseños a Prueba de Manipulaciones:** Encapsulado con sensores de malla integrados
- **Bucles de Arranque Seguros:** Revalidar criptografía múltiples veces durante boot
- **Certificaciones de Seguridad:** Cumplir FIPS 140-3 para diseño de elementos seguros

## 🔐 Seguridad Ofensiva Legítima

El voltage glitching también es una herramienta crucial en:

- **Pruebas de Penetración:** Evaluación autorizada de vulnerabilidades
- **Red Teaming:** Simulaciones de ataques para mejorar defensas
- **Investigación Académica:** Avances en seguridad de hardware
- **Ingeniería Inversa:** Análisis de componentes de sistemas embebidos
- **Evaluación de Dispositivos IoT e Industriales**

## ⚖️ Consideraciones Legales y Éticas

⚠️ **Descargo de Responsabilidad:** Este material tiene fines **exclusivamente educativos**. El uso de estas técnicas en dispositivos de terceros sin autorización es **ilegal**.

### Uso Ético:
- Investigación académica de seguridad (con permiso)
- Competiciones autorizadas
- Pruebas de penetración con consentimiento explícito
- Divulgación responsable de vulnerabilidades a fabricantes

## 📊 Diferencia entre Fallos Técnicos y Otros Ataques Hardware

| Tipo de Ataque | Enfoque | Ejemplo |
|---|---|---|
| **Glitching** | Manipulación precisa de condiciones | Ataques basados en voltaje/reloj |
| **Ataques de Canal Lateral** | Observar información indirecta | Análisis de potencia/EM |
| **Rowhammer** | Inyección de fallos en RAM | Invertir bits mediante acceso repetido |
| **Ataques de Boot Frío** | Robo de datos post-reinicio | Extracción de claves de DRAM |

## 🚨 Por Qué Importa el Voltage Glitching

- **Revela vulnerabilidades ocultas:** Elude defensas de software atacando directamente el hardware
- **Amenaza a infraestructura crítica:** Riesgo potencial en sistemas de control industrial y dispositivos médicos
- **Evolución de la ciberseguridad:** Requiere incorporar defensas a nivel de hardware en marcos de seguridad
- **Creciente relevancia:** Con proliferación de IoT y sistemas embebidos, la resiliencia a fallos es crítica

## 📚 Recursos y Referencias

- [NewAE - ChipWhisperer](https://newae.com)
- [PicoGlitcher - GitHub](https://github.com)
- Documentación de Bus Pirate 5
- Papers académicos sobre fault injection
- Competiciones de seguridad de hardware

## 🎓 Recomendaciones de Seguridad

Para proteger sistemas críticos:

1. **Evalúe** su tecnología en busca de vulnerabilidades a nivel de hardware
2. **Manténgase informado** sobre nuevos vectores de ataque
3. **Invierta** en medidas de protección sólidas para sistemas críticos
4. **Implemente** redundancia y detección en componentes sensibles
5. **Realice** auditorías de seguridad física regularmente

---

⭐ *Creado para la comunidad de Hardware Hacking y Seguridad Ofensiva*

**Última actualización:** 2025 | **Licencia:** Educativa | **Autor:** Comunidad de Seguridad Hardware
