<div align="center">

# ⚖️ ScaleCore
### *Software Keyboard Wedge de Alto Rendimiento para Balanzas POS en Tiempo Real*

[![Python Version](https://img.shields.io/badge/Python-3.10%20%7C%203.11%20%7C%203.12%20%7C%203.14-blue?logo=python&logoColor=white)](https://python.org)
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%20%7C%2011%20%7C%20POSReady-0078D6?logo=windows&logoColor=white)](https://microsoft.com)
[![License](https://img.shields.io/badge/License-Proprietary%20%2F%20EULA-red.svg)](LICENSE.md)
[![DNDA Status](https://img.shields.io/badge/Registro%20DNDA-En%20Trámite%20Radicado%202026-success)](#-propiedad-intelectual-y-registro-legal)
[![Brand](https://img.shields.io/badge/Brand-Moraxsoft-6366F1)](https://wa.me/573143927295)

<p align="center">
  <b>Conexión plug-and-play entre balanzas electrónicas industriales (RS-232 / USB Serial) y cualquier sistema POS web o de escritorio.</b>
</p>

---

</div>

## 📌 Visión General

**ScaleCore** es un software comercial diseñado para actuar como un puente de hardware (*Keyboard Wedge*) de latencia cero. Captura el peso continuo emitido por básculas electrónicas y lo inyecta directamente en la casilla activa de cualquier navegador web o software de Punto de Venta (Next.js, React, Electron, Java, Odoo, ERPs) con solo presionar una tecla configurable (por defecto `F2`).

Incluye una interfaz **HUD flotante (Always-on-Top)** táctil y arrastrable, algoritmo de estabilización por **mayoría de votos** (evita oscilaciones en 3 decimales) y un motor de **auto-reconexión en caliente**.

---

## 🚀 Características Principales

| Característica | Descripción |
| :--- | :--- |
| 🪟 **HUD Flotante Táctil** | Overlay minimalista con Tkinter que permanece siempre visible sobre cualquier ventana sin bordes molestos. Haz clic y arrástralo a cualquier esquina de la pantalla. |
| ⚡ **Inyección Virtual SendInput** | Escribe el peso directamente en el campo activo del cajero emulando pulsaciones físicas a bajo nivel de Windows (`SendInput` Unicode). |
| 🎯 **Filtro de Mayoría de Votos** | Algoritmo *Sliding Window* que procesa las tramas continuas y bloquea saltos erróneos de dígitos causados por vibración mecánica o ruido eléctrico. |
| 🔄 **Reconexión en Caliente** | Si el cajero apaga la balanza o desconecta el cable serial, el sistema no se bloquea y reanuda la lectura automáticamente al detectar señal nuevamente. |
| ⚙️ **Configuración Flexible** | Modifica puerto COM, baudrate, tecla de inyección (`F2`, `F4`, `Espacio`) y separador decimal (`.` o `,`) desde un archivo `config.json`. |

---

## 🖼️ Interfaz Gráfica (HUD Overlay)

```text
┌──────────────────────────────────────────────┐
│  SCALECORE [F2]                       🔄  🟢 │
│                                              │
│                  0.305 kg                    │
│             ENCENDIDA (COM1)                 │
└──────────────────────────────────────────────┘
   ▲                    ▲                   ▲
   │                    │                   └─ Estado de Conexión
   │                    └─ Peso en Vivo (3 decimales)
   └─ Tecla de Inyección Rápida
```

* 🟢 **Verde:** Báscula conectada transmitiendo datos estables.
* 🟡 **Amarillo:** Buscando balanza o reconectando puerto.
* 🔴 **Rojo:** Báscula apagada o cable desconectado.
* 🖱️ **Clic Derecho:** Menú contextual para forzar reconexión o salir.
* ✌️ **Doble Clic:** Inyecta el peso manualmente sin usar el teclado.

---

## 🏛️ Propiedad Intelectual y Registro Legal

> [!IMPORTANT]
> **SOFTWARE PROPIETARIO — TODOS LOS DERECHOS RESERVADOS**
> 
> Esta obra de software ha sido radicada formalmente ante la **Dirección Nacional de Derecho de Autor (DNDA)** de la República de Colombia bajo el título **ScaleCore** por su único autor y productor:
> 
> * **Titular y Autor:** Jorge Rafael Buitrago Morantes
> * **Seudónimo Profesional:** RafxDev
> * **Marca Comercial:** Moraxsoft
> * **Cédula de Ciudadanía:** No. 1052837407 (Colombia)
> * **Año de Creación:** 2026
> * **Marco Legal:** Ley 23 de 1982, Ley 44 de 1993, Decisión 351 de la Comunidad Andina y Convenio de Berna (OMPI).

> [!CAUTION]
> **PROHIBICIÓN ESTRICTA DE LUCRO Y REVENTA**
> Queda terminantemente prohibido a terceros vender, sublicenciar, alquilar, descompilar, realizar ingeniería inversa o lucrarse directa o indirectamente con este software sin un contrato formal firmado por el autor. Consulte los términos completos en [LICENSE.md](LICENSE.md).

---

## 📂 Arquitectura del Repositorio

```text
ScaleCore/
│
├── diagnostics/                 # 🛠️ Suite de diagnóstico y calibración de básculas
│   ├── check_all_coms.py        # Sondeo de puertos COM1-COM20 y registro de Windows
│   ├── debug_com1.py            # Pruebas con líneas de control de flujo DTR/RTS
│   ├── query_win32_ports.py     # Detección de dispositivos a nivel Kernel Windows
│   ├── scan_ports.py            # Listador dinámico de puertos seriales activos
│   ├── test_parse_bbg.py        # Validador de expresiones regulares para tramas BBG
│   ├── test_scale.py            # Monitor de tramas crudas (RAW bytes)
│   └── README.md                # Documentación interna de herramientas
│
├── .gitignore                   # Exclusión de ejecutables y temporales para Git
├── build.bat                    # Script automático para compilar dist/ScaleCore.exe
├── config.json                  # Parametrización activa (puerto, velocidad, atajo)
├── LICENSE.md                   # Contrato formal de licencia comercial propietaria EULA
├── README.md                    # Documentación técnica y comercial del proyecto
├── requirements.txt             # Dependencias de Python requeridas
├── scale_wedge.py               # Código fuente principal de la aplicación
└── ScaleWedgePOS.spec           # Especificación de empaquetado de PyInstaller
```

---

<!--
## ⚙️ Parametrización (`config.json`)

El archivo config.json permite ajustar el comportamiento sin necesidad de modificar el código fuente:

```json
{
  "scale_model": "BBG Market 30 / IPBG",
  "port": "COM1",
  "baudrate": 9600,
  "trigger_key": "f2",
  "decimal_separator": ".",
  "hud_position": "+1150+20",
  "hud_width": 220,
  "hud_height": 80
}
```

| Parámetro | Tipo | Descripción |
| :--- | :---: | :--- |
| `port` | `string` | Puerto serial asignado a la balanza (`"COM1"`, `"COM3"`, etc.). |
| `baudrate` | `number` | Velocidad de transmisión en baudios (habitualmente `9600`). |
| `trigger_key` | `string` | Tecla global de inyección (`"f2"`, `"f4"`, `"space"`, `"enter"`). |
| `decimal_separator` | `string` | Separador de decimales (`"."` o `","`) según el formato del POS. |
| `hud_position` | `string` | Coordenadas en pantalla (`"+X+Y"`) donde se ubica el HUD. |
-->

---

<!--
## 💻 Instalación y Compilación

### 1. Modo Desarrollo
```powershell
# 1. Clonar el repositorio
git clone <URL_DEL_REPOSITORIO>
cd ScaleCore

# 2. Instalar dependencias
python -m pip install -r requirements.txt

# 3. Iniciar en modo desarrollo
python scale_wedge.py
```

### 2. Generar el Ejecutable Autónomo (`.exe`)
Para entregar al cliente sin código fuente y sin requerir que tenga Python instalado:
1. Ejecuta el archivo build.bat (doble clic o por terminal).
2. El compilador generará automáticamente el ejecutable en: dist\ScaleCore.exe

### 3. Puesta en Marcha en el Punto de Venta
1. Copia únicamente el archivo dist\ScaleCore.exe y un archivo config.json en el equipo del cliente.
2. Presiona Win + R, escribe shell:startup y pega un acceso directo de ScaleCore.exe.
3. Cada vez que el computador o equipo All-in-One encienda, el widget flotante estará listo para inyectar peso.
-->

---

## 📞 Licenciamiento Comercial y Soporte

Para adquirir licencias comerciales por terminal, homologaciones de nuevos modelos de básculas (Torrey, Systel, Mettler Toledo) o soporte técnico:

<div align="center">

**Desarrollado y Distribuido por Moraxsoft**  
*Jorge Rafael Buitrago Morantes (RafxDev)*  
Duitama, Boyacá — Colombia 🇨🇴  

[![WhatsApp](https://img.shields.io/badge/WhatsApp-Contactar%20por%20WhatsApp-25D366?logo=whatsapp&logoColor=white)](https://wa.me/573143927295)
[![Email](https://img.shields.io/badge/Email-jrafaelmorantess%40gmail.com-D14836?logo=gmail&logoColor=white)](mailto:jrafaelmorantess@gmail.com)

</div>

---

<div align="center">
  <sub>Copyright © 2026 Jorge Rafael Buitrago Morantes / Moraxsoft. Todos los derechos reservados.</sub>
</div>
