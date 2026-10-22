# Práctica 8 – Automatización con Ansible y Docker

Se crean 5 servidores Ubuntu con Docker y se automatiza su configuración con Ansible.

## Archivos

| Archivo | Descripción |
|---|---|
| `Dockerfile` | Imagen Ubuntu 24.04 con servidor SSH, usuario `ansible` (password `ansible`) con sudo sin password |
| `docker-compose.yml` | 5 servidores (`server1`–`server5`) en la red `172.30.0.0/24` + nodo de control `ansible` |
| `controller/Dockerfile` | Ubuntu con `ansible` y `sshpass` instalados con `apt` (alternativa a WSL) |
| `inventario.ini` | Nombres, IPs, usuario y clave de los servidores |
| `ansible.cfg` | Usa `inventario.ini` y desactiva la verificación de llaves SSH (`host_key_checking = False`) |
| `playbook.yml` | Tareas de automatización |
| `salida-playbook.txt` | Salida de la ejecución del playbook |

## Servidores

| Nombre | IP | Puerto SSH en el host |
|---|---|---|
| server1 | 172.30.0.11 | 2221 |
| server2 | 172.30.0.12 | 2222 |
| server3 | 172.30.0.13 | 2223 |
| server4 | 172.30.0.14 | 2224 |
| server5 | 172.30.0.15 | 2225 |

## Tareas del playbook

1. Actualizar los paquetes de APT del sistema operativo
2. Crear el usuario `itla`
3. Crear la carpeta `app` (`/home/itla/app`)
4. Crear el archivo `hola.txt` dentro de `app`
5. Instalar `cowsay` y `htop`
6. Probar `cowsay` y mostrar el resultado

## Ejecución

```bash
# 1. Levantar los 5 servidores
docker compose up -d

# 2. Probar conexión con Ansible
docker compose run --rm ansible ansible all -m ping

# 3. Ejecutar el playbook
docker compose run --rm ansible ansible-playbook playbook.yml
```

### Usando WSL

```bash
wsl --install          # en Windows
sudo apt update && sudo apt install -y ansible sshpass
```

Las IPs `172.30.0.x` son de la red interna de Docker. Desde WSL se usa el contenedor
`ansible` (comandos de arriba), que está dentro de esa red.

## Verificación

```bash
docker exec server1 cat /home/itla/app/hola.txt
docker exec server1 /usr/games/cowsay hola
docker exec server1 which htop
```

## Resultado

```
PLAY RECAP *********************************************************************
server1  : ok=8    changed=5    unreachable=0    failed=0
server2  : ok=8    changed=5    unreachable=0    failed=0
server3  : ok=8    changed=5    unreachable=0    failed=0
server4  : ok=8    changed=5    unreachable=0    failed=0
server5  : ok=8    changed=5    unreachable=0    failed=0
```
