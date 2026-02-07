# Día 1 – Instalación, sistema de archivos y usuarios

Bienvenido/a al **Día 1** del curso **Red Hat Enterprise Linux 8 – Diagnostics & Troubleshooting**.  
El objetivo de esta sesión es que salgas con un **laboratorio funcional** desde el primer día y con la base necesaria para afrontar el troubleshooting del resto del curso.

---

## Objetivos del alumno (al finalizar el día 1)

- Tener una VM RHEL/Rocky **instalada y operativa** (GUI/CLI) y lista para trabajar.
- Realizar **checks básicos** del sistema (versión, red, memoria, disco, hora).
- Moverte con soltura por el **sistema de archivos** y entender su estructura.
- Ejecutar operaciones habituales con ficheros y directorios (**crear, copiar, mover, borrar**) con seguridad.
- Entender **permisos, propietarios y umask** y su impacto directo en incidencias típicas (“Permission denied”).
- Editar ficheros de configuración con **vim** en modo supervivencia.
- Crear y administrar **usuarios y grupos** con criterio (incluyendo verificación en ficheros clave).
- Completar un lab de cierre: **usuarios + grupo + carpeta compartida + permisos correctos**.

---

## Temas que tocamos

### 1) Enfoque de troubleshooting (cómo trabajaremos)
- Metodología “aprender haciendo”: explicación corta → demo → lab → mini-repaso
- Checklist para evitar bloqueos en remoto

### 2) Instalación / laboratorio (RHEL/Rocky)
- Creación de VM (VirtualBox) y primeros pasos
- Checks tras el primer arranque
- (Opcional) Guest Additions y actualización del sistema

### 3) Sistema de archivos
- Estructura de directorios y mapa mental mínimo
- Navegación y comandos de supervivencia
- Gestión básica de ficheros y directorios

### 4) Permisos y propietarios
- `chmod`, `chown`, `umask`
- Diagnóstico rápido de “Permission denied”
- Conceptos clave en directorios (permiso `x`)

### 5) Edición con vim (lo imprescindible)
- Modos, guardar/salir, buscar
- Chuleta mínima para admins

### 6) Usuarios y grupos
- `useradd`, `groupadd`, `usermod`, `passwd`
- Ficheros clave: `/etc/passwd`, `/etc/shadow`, `/etc/group`
- Verificación con `id` / `getent`

---

## Material asociado

- `comandos-dia1.md` → chuleta de comandos del día 1
- (Opcional) `labs-dia1.md` → guía de laboratorio y validaciones

---

## Cierre y preparación del día 2

Antes de terminar el día 1, verifica:
- La VM arranca y tienes terminal operativa
- Red / DNS OK
- Usuarios y grupo creados
- Permisos del lab correctos

En el **día 2** entraremos con:
- Gestión de software (dnf/yum y repositorios)
- Almacenamiento (montajes persistentes y LVM básico)
- Servicios con `systemctl`

