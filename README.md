<p align="center">
  <img src="logo.png" width="110" alt="SSTool">
</p>

<h1 align="center">SSTool</h1>

<p align="center">
  Lanzador de herramientas de screenshare para Windows: más de 100 tools forenses en una sola ventana,<br>
  que se descargan al momento, se ejecutan y se borran solas al cerrarlas.
</p>

<p align="center">
  <img alt="Windows 10/11" src="https://img.shields.io/badge/Windows-10%20%7C%2011-3b82f6">
  <img alt=".NET Framework 4.8" src="https://img.shields.io/badge/.NET%20Framework-4.8-8b5cf6">
  <img alt="x64" src="https://img.shields.io/badge/arch-x64-22d3ee">
</p>

---

## Qué es

SSTool reúne en un solo programa las herramientas que se usan en un screenshare (Prefetch, BAM, Amcache, USN Journal, MFT, memoria, registros de eventos…). En vez de ir buscando cada exe por GitHub y webs, eliges la herramienta y SSTool:

1. La **descarga** a una carpeta temporal (siempre la última versión conocida).
2. La **ejecuta**.
3. La **borra** cuando la cierras, y limpia todo lo que quede al salir de SSTool.

No deja nada instalado en el PC revisado.

## Funciones

- **115 herramientas** organizadas en secciones:

  | Sección | Herramientas |
  |---|---|
  | OrbDiff | 17 |
  | Spokwn | 11 |
  | tonynoh | 5 |
  | Scripts (PowerShell) | 30 |
  | RedLotus | 4 |
  | NirSoft | 18 |
  | Eric Zimmerman | 10 |
  | Otros | 9 |
  | Dependencias (.NET 9 / 10, VC++) | 3 |
  | Webs | 8 |

- **Búsqueda global** (`Ctrl+F`) en todas las secciones; `Enter` abre el primer resultado.
- **Comprobación de versiones**: consulta GitHub y marca en verde las herramientas que tienen una versión más nueva.
- **Tamaño antes de descargar**, en el tooltip de cada herramienta y en la descarga múltiple (con el total).
- **Descarga múltiple**: elige varias herramientas de una sección y se guardan en `Descargas\SSTool`.
- **Comandos listos para las tools de consola** (Eric Zimmerman y otras): eliges qué analizar, puedes editar el comando, y los resultados van a `Descargas\SSTool\Resultados`.
- **Aviso de runtime**: si una herramienta necesita .NET 9 y no está instalado, la tarjeta lo indica y te lleva a *Dependencias*.
- **Barra "En ejecución"** con las herramientas abiertas y un botón para cerrar cada una.
- **Autoactualización**: avisa cuando hay una versión nueva de SSTool y se actualiza con un clic.
- **Español / English**, animaciones completas o reducidas (se reducen solas en equipos con GPU débil).

## Requisitos

- Windows 10 u 11 de 64 bits.
- .NET Framework 4.8 (ya viene con Windows 10 1903+ y Windows 11).
- Permisos de administrador: SSTool los pide al abrirse, y las herramientas que lanza los heredan.
- Conexión a internet para descargar las herramientas.

Algunas herramientas necesitan además el runtime de .NET 9 (las de Eric Zimmerman). Se instala desde la pestaña **Dependencias**.

## Descarga

Descarga `SSTool.exe` desde la página de [Releases](../../releases/latest) y ábrelo. No necesita instalación.

> Windows SmartScreen puede avisar porque el exe no está firmado: *Más información → Ejecutar de todas formas*.

## Uso

| Acción | Cómo |
|---|---|
| Abrir una herramienta | Clic en su tarjeta |
| Más opciones (GitHub, copiar enlace, guardar en Descargas…) | Clic derecho en la tarjeta |
| Buscar en todo | `Ctrl+F` |
| Abrir el primer resultado | `Enter` |
| Cambiar de sección | `Ctrl+1` … `Ctrl+9` |
| Cerrar búsqueda o ventana emergente | `Esc` |
| Descargar varias a la vez | Botón **Descargar ▾** de la sección |
| Ajustes (idioma, animaciones, tooltips, actualizaciones) | Engranaje de la barra de título |

### Dónde guarda las cosas

| Qué | Dónde |
|---|---|
| Herramientas que se ejecutan | `%TEMP%\SSTool\<id>` (se borra al cerrarlas) |
| Descargas guardadas | `Descargas\SSTool\<sección>` |
| Resultados de los comandos | `Descargas\SSTool\Resultados` |
| Ajustes y caché de versiones | `%AppData%\SSTool` |

## Compilar desde el código

No hace falta Visual Studio: se compila con el compilador de C# que trae .NET Framework 4.8.

```powershell
git clone https://github.com/ScaanSs/SSTool.git
cd SSTool
powershell -ExecutionPolicy Bypass -File .\build.ps1
```

Genera `SSTool.exe` en la misma carpeta.

### Estructura

| Archivo | Contenido |
|---|---|
| `SSTool.cs` | Interfaz (WPF), descargas, actualizaciones y ajustes |
| `Data.cs` | Lista de herramientas: sección, nombre, descripción, URL y comandos |
| `scripts/` | Scripts de PowerShell incluidos dentro del exe |
| `app.ico`, `app.manifest` | Icono y manifiesto (pide administrador) |
| `logo.png` | Logo para el README |
| `build.ps1` | Script de compilación |

### Añadir una herramienta

En `Data.cs`, dentro de su sección:

```csharp
t.Add(new Tool("OrbDiff", "MiTool", "Descripción en español", "English description",
               gh("Usuario", "Repo", "v1.0", "MiTool.exe"), "dl"));
```

Tipos: `dl` (descargar y ejecutar), `ps` (script de PowerShell remoto), `local` (script incluido en `scripts/`), `web` (abrir página).

### Publicar una versión nueva

1. Sube el número en `AppVersion` dentro de `SSTool.cs` (por ejemplo `"1.0.1"`).
2. Compila con `build.ps1`.
3. Crea un release en GitHub con la etiqueta `v1.0.1` y adjunta `SSTool.exe`.

Las copias de SSTool que ya estén instaladas lo detectarán al abrirse y ofrecerán actualizar.

## Créditos

SSTool solo descarga y lanza las herramientas; cada una pertenece a su autor:
[Orbdiff](https://github.com/Orbdiff) ·
[spokwn](https://github.com/spokwn) ·
[tonynoh](https://github.com/MeowTonynoh) ·
[RedLotus](https://github.com/itzicehere) ·
[NirSoft](https://www.nirsoft.net) ·
[Eric Zimmerman](https://ericzimmerman.github.io) ·
y el resto de desarrolladores enlazados en cada tarjeta.

Hecho por **Scaan**.

---

<details>
<summary><b>English</b></summary>

**SSTool** is a screenshare tool launcher for Windows. It bundles 115 forensic tools (Prefetch, BAM, Amcache, USN Journal, MFT, memory, event logs…) in one window: pick a tool and SSTool downloads it to a temp folder, runs it, and wipes it when it closes. Nothing is left installed on the checked PC.

- Global search (`Ctrl+F`), update checks against GitHub, file sizes before downloading, bulk downloads, ready-made commands for console tools, missing .NET 9 warnings, a "running" bar, self-update, Spanish / English UI.
- Requires Windows 10/11 x64, .NET Framework 4.8 and administrator rights.
- Download `SSTool.exe` from [Releases](../../releases/latest).
- Build: `powershell -ExecutionPolicy Bypass -File .\build.ps1`.
- Every tool belongs to its author; SSTool only downloads and launches them.

</details>
