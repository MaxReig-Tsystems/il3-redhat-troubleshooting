# Día 2 – Comandos esenciales (software, almacenamiento y servicios)

Curso **Red Hat Enterprise Linux 8 – Diagnostics & Troubleshooting**  
Material de apoyo – Día 2

Este documento recoge los **comandos más utilizados** durante el Día 2 del curso.  
Está pensado como **chuleta rápida para admins junior**, tanto en laboratorio como en incidencias reales.

---

## 0) Checks rápidos antes de empezar (sanity check)

```bash
cat /etc/redhat-release
hostnamectl
uptime
timedatectl

ip a
ip r
ping -c 2 8.8.8.8 || true
ping -c 2 google.com || true

df -h
free -h
lsblk
```

---

## 1) Gestión de software: DNF / YUM (y repos)

> En RHEL 8 / Rocky 8/9, **DNF** es el gestor moderno. `yum` suele ser un wrapper/alias compatible.

### Operaciones básicas

```bash
sudo dnf search paquete
sudo dnf info paquete
sudo dnf install -y paquete
sudo dnf remove -y paquete
sudo dnf reinstall -y paquete
sudo dnf list installed | head
sudo dnf provides /ruta/binario
```

Actualizar / sincronizar:

```bash
sudo dnf check-update || true
sudo dnf -y update
sudo dnf -y upgrade
sudo dnf -y distro-sync
sudo dnf -y autoremove
```

### Repositorios (listar / ver detalles)

```bash
sudo dnf repolist
sudo dnf repolist --all
sudo dnf repoinfo
sudo dnf repoinfo nombre_repo
```

Cache (cuando “no encuentra paquetes” o hay cosas raras):

```bash
sudo dnf clean all
sudo dnf makecache
```

### Historial (muy útil para deshacer “la he liado”)

```bash
sudo dnf history
sudo dnf history info ID
sudo dnf history undo ID
sudo dnf history rollback ID
```

### Ficheros de repos (dónde viven y cómo revisarlos)

```bash
ls -lah /etc/yum.repos.d/
grep -R "^\[.*\]" -n /etc/yum.repos.d/*.repo
grep -R "^enabled=" -n /etc/yum.repos.d/*.repo
```

> Tip: si un repo está deshabilitado (`enabled=0`), dnf no lo usará.

### Habilitar / deshabilitar repos (si tienes dnf-plugins-core)

```bash
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --set-enabled nombre_repo
sudo dnf config-manager --set-disabled nombre_repo
```

---

## 2) Parches / updates de seguridad (visión práctica)

> No es teoría aquí: estos comandos ayudan a decidir “qué actualizar” cuando hay incidencias.

Ver advisories (si están disponibles en tu distro/repos):

```bash
sudo dnf updateinfo list
sudo dnf updateinfo list security
sudo dnf updateinfo info --list | head
```

Aplicar solo parches de seguridad (si tu entorno lo soporta):

```bash
sudo dnf -y update --security
```

> Nota: la disponibilidad de `updateinfo` y advisories depende de repos / metadatos.

---

## 3) Red Hat: modelo de suscripción (comandos útiles)

> En **RHEL** real, la suscripción controla acceso a repos oficiales/soporte.  
> En **Rocky/Alma**, no se usa `subscription-manager` para repos oficiales de Red Hat.

Estado de suscripción (RHEL):

```bash
sudo subscription-manager status
sudo subscription-manager identity
sudo subscription-manager list --available | head
sudo subscription-manager repos --list | head
sudo subscription-manager repos --list-enabled
```

> Si no tienes acceso a repos en RHEL, muchas veces el root cause es **suscripción/repos no habilitados**.

---

## 4) Discos y almacenamiento: descubrir / entender el layout

### Ver discos, particiones y FS

```bash
lsblk
lsblk -f
sudo fdisk -l
sudo parted -l
blkid
```

Ver montajes y origen (muy troubleshooting):

```bash
df -hT
mount | column -t | head
findmnt
findmnt -T /ruta
```

Cuando acabas de “presentar” un disco en VirtualBox y no aparece:

```bash
dmesg | tail -n 80
lsblk
sudo udevadm settle
```

> Si el disco es nuevo, suele aparecer como `/dev/sdb`, `/dev/sdc`, etc.

---

## 5) Particionado básico (si lo necesitas)

> En el lab de LVM puedes usar el disco “entero” como PV (sin particionar).  
> Si tu criterio es particionar, estos comandos son los de supervivencia.

```bash
sudo fdisk /dev/sdX
sudo parted /dev/sdX
```

Ver tabla de particiones:

```bash
lsblk
sudo fdisk -l /dev/sdX
```

---

## 6) LVM básico (PV → VG → LV)

### Comandos de inspección

```bash
pvs
vgs
lvs

sudo pvdisplay
sudo vgdisplay
sudo lvdisplay
```

### Crear estructura (básico)

```bash
sudo pvcreate /dev/sdX
sudo vgcreate vgdata /dev/sdX
sudo lvcreate -n lv01 -L 2G vgdata
sudo lvcreate -n lv02 -L 2G vgdata
sudo lvcreate -n lv03 -L 2G vgdata
```

Extender (típico en troubleshooting por falta de espacio):

```bash
sudo lvextend -L +1G /dev/vgdata/lv01
# Si quieres extender y redimensionar FS automáticamente (XFS/EXT4 con -r):
sudo lvextend -r -L +1G /dev/vgdata/lv01
```

---

## 7) Filesystems (crear / comprobar)

Crear FS (elige uno; en RHEL suele usarse mucho XFS):

```bash
sudo mkfs.xfs /dev/vgdata/lv01
sudo mkfs.ext4 /dev/vgdata/lv02
```

Info rápida:

```bash
sudo xfs_info /dev/vgdata/lv01 2>/dev/null || true
sudo tune2fs -l /dev/vgdata/lv02 2>/dev/null | head || true
```

---

## 8) /boot y swap (checks y comandos)

### Swap

Ver swap actual:

```bash
swapon --show
free -h
```

Crear y activar swap (si hiciera falta en lab/ejemplo):

```bash
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
swapon --show
```

Desactivar swap:

```bash
sudo swapoff -a
```

> Nota: swap persistente se configura también en `/etc/fstab`.

### /boot (checks típicos)

```bash
df -h /boot
ls -lah /boot | head
```

---

## 9) Montajes persistentes: /etc/fstab (lo que se rompe en la vida real)

Backup antes de tocar:

```bash
sudo cp -a /etc/fstab /etc/fstab.bak
```

Obtener UUID (recomendado para fstab):

```bash
blkid
lsblk -f
```

Montar manualmente (prueba antes de persistir):

```bash
sudo mkdir -p /mnt/datos1
sudo mount /dev/vgdata/lv01 /mnt/datos1
df -hT /mnt/datos1
findmnt /mnt/datos1
```

Validar `fstab` SIN reiniciar:

```bash
sudo mount -a
findmnt
```

> Si `mount -a` falla, revisa el error: suele ser UUID mal, punto de montaje inexistente o tipo de FS incorrecto.

---

## 10) Servicios con systemctl (systemd)

Estado y control básico:

```bash
systemctl status nombre.service
sudo systemctl start nombre.service
sudo systemctl stop nombre.service
sudo systemctl restart nombre.service
sudo systemctl reload nombre.service 2>/dev/null || true
```

Arranque automático:

```bash
sudo systemctl enable nombre.service
sudo systemctl disable nombre.service
systemctl is-enabled nombre.service
```

Listados útiles:

```bash
systemctl list-units --type=service --state=running
systemctl list-unit-files --type=service | head
```

Ver definición y overrides:

```bash
systemctl cat nombre.service
systemctl show nombre.service | head
sudo systemctl edit nombre.service
sudo systemctl daemon-reload
```

Logs del servicio:

```bash
journalctl -u nombre.service -n 100 --no-pager
journalctl -u nombre.service -xe --no-pager
```

Servicios comunes (según entorno):

```bash
systemctl status sshd
systemctl status NetworkManager
systemctl status firewalld
systemctl status chronyd
```

---

## 11) Lab Día 2 (referencia) – LVM + FS + fstab (resumen de comandos)

> Ejemplo típico (ajusta `sdX`, tamaños y puntos de montaje).  
> Supongamos que el disco nuevo es `/dev/sdb` y crearemos 3 FS.

### 1) Detectar el disco nuevo

```bash
lsblk
sudo fdisk -l | grep -E "Disk /dev/sd"
dmesg | tail -n 60
```

### 2) Crear LVM (PV/VG/LV)

```bash
sudo pvcreate /dev/sdb
sudo vgcreate vg_lab /dev/sdb

sudo lvcreate -n lv_data1 -L 2G vg_lab
sudo lvcreate -n lv_data2 -L 2G vg_lab
sudo lvcreate -n lv_data3 -L 2G vg_lab

pvs
vgs
lvs
```

### 3) Crear filesystems

```bash
sudo mkfs.xfs /dev/vg_lab/lv_data1
sudo mkfs.xfs /dev/vg_lab/lv_data2
sudo mkfs.xfs /dev/vg_lab/lv_data3
```

### 4) Crear puntos de montaje y montar

```bash
sudo mkdir -p /data/data1 /data/data2 /data/data3

sudo mount /dev/vg_lab/lv_data1 /data/data1
sudo mount /dev/vg_lab/lv_data2 /data/data2
sudo mount /dev/vg_lab/lv_data3 /data/data3

df -hT | grep -E "/data/data"
findmnt | grep -E "/data/data"
```

### 5) Persistencia en /etc/fstab (con UUID)

1) Obtener UUID:

```bash
lsblk -f | grep vg_lab
blkid | grep vg_lab
```

2) Editar `/etc/fstab` (recomendado con backup):

```bash
sudo cp -a /etc/fstab /etc/fstab.bak
sudo vim /etc/fstab
```

Ejemplo de líneas (ajusta UUIDs y tipo `xfs`):

```text
UUID=XXXX-XXXX  /data/data1  xfs  defaults  0  0
UUID=YYYY-YYYY  /data/data2  xfs  defaults  0  0
UUID=ZZZZ-ZZZZ  /data/data3  xfs  defaults  0  0
```

3) Validar sin reiniciar:

```bash
sudo umount /data/data1 /data/data2 /data/data3
sudo mount -a
findmnt | grep -E "/data/data"
```

> Si `mount -a` falla: revisa que existan los directorios, el UUID sea correcto y el tipo de FS coincida.

---

## 12) Comandos “salvavidas” (orden recomendado – Día 2)

Cuando algo falla relacionado con software / discos / servicios:

```bash
# Software/repos
sudo dnf repolist
sudo dnf check-update || true

# Discos/montajes
lsblk -f
df -hT
findmnt

# Servicios + logs
systemctl status nombre.service
journalctl -u nombre.service -xe --no-pager
```

🧠 Consejo final: en troubleshooting, **primero valida** (estado/repos/disco/montajes),
y **luego cambias cosas** (updates, fstab, lvm, systemctl).
