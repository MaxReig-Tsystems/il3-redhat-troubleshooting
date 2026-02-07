# Día 1 – Comandos esenciales (cheatsheet)

Curso **Red Hat Enterprise Linux 8 – Diagnostics & Troubleshooting**  
Material de apoyo – Día 1

Este documento recoge los **comandos más utilizados** durante el Día 1 del curso.  
Está pensado como **chuleta rápida para admins junior**, tanto en laboratorio como en incidencias reales.

---

## 1) Checks básicos tras la instalación / primer arranque

Usa esto para validar rápido: **versión**, **red**, **memoria**, **disco**, **hora**.

```bash
cat /etc/redhat-release
hostname
hostnamectl
uptime
date
timedatectl

ip a
ip -4 a
ip r

free -h
df -h
df -hTP
```

```bash
#### Pruebas rápidas de conectividad / DNS
ping -c 3 8.8.8.8
ping -c 3 google.com
curl -I https://www.google.com
```

## 2) Atajos útiles en terminal y documentación

```bash
clear
history | tail
man "command"

### Atajos de teclado:
Ctrl + R → buscar en el histórico
Ctrl + C → cortar proceso
Ctrl + L → limpiar pantalla
```

## 3) Orientación en el sistema de archivos

```bash
pwd
ls
ls -l
ls -lah
cd /etc
cd ~
cd -
cd ..

### Trucos rápidos:
cd - → vuelve al directorio anterior
TAB → autocompletar rutas y comandos
```

## 4) Gestión básica de ficheros 

```bash
mkdir -p ~/lab1
cd ~/lab1
touch a.txt b.txt

### Copiar / mover / borrar:
cp a.txt copia_a.txt
mv b.txt renombrado.txt
rm copia_a.txt

### Copiar directorios preservando atributos (recomendado)
cp -a dir1 dir2
```

### 5) Buscar ficheros y comandos (orientación rápida)

```bash
### Encontrar ficheros (ejemplo):
find /etc -maxdepth 2 -name "*.conf" 2>/dev/null | head

### Saber dónde está un comando:
which ls
which vim
```

### 6) Permisos y propietarios (chmod / chown / umask)

```bash
### Leer permisos
ls -l
ls -ld directorio

### Ejemplo típico:
-rw-r----- 1 ana dev 120 Jan 1 10:00 informe.txt
### Tres conjuntos: usuario (u), grupo (g), otros (o)
### Tres permisos: lectura (r), escritura (w), ejecución (x)
```

### 7) Diagnóstico rápido: “Permission denied”
```bash
### Método en 6 preguntas:

1 - ¿Qué usuario ejecuta la acción? (whoami / id)
2 - ¿Sobre qué ruta exacta falla? (ruta absoluta)
3 - ¿Propietario y grupo del recurso? (ls -l / ls -ld)
4 - ¿Permisos del recurso y del directorio padre? (x para entrar)
5 - ¿Grupo efectivo del usuario? (id, groups)
6 - ¿Afecta umask a ficheros nuevos?

### Comandos
whoami
id
groups
namei -l /ruta/objetivo
ls -ld /ruta/objetivo
getfacl /ruta/objetivo 2>/dev/null || true
umask
```


