# Uso de Nmap

Nmap es una herramienta esencial para encontrar puertos abiertos, realizar auditorías de seguridad y descubrir servicios en ejecución. Permite identificar sistemas operativos y versiones de software, siendo muy útil para profesionales de la seguridad informática.

---

## Comandos básicos de Nmap

```bash
nmap -sS <IP>                # Escaneo de puertos TCP SYN
nmap -sU <IP>                # Escaneo de puertos UDP
nmap -sV <IP>                # Detección de versiones de servicios
nmap -p <puerto> <IP>        # Escaneo de un puerto específico
nmap -p- <IP>                # Escaneo de todos los puertos
nmap -O <IP>                 # Detección de sistema operativo
nmap -A <IP>                 # Escaneo avanzado (OS, versiones, scripts y traceroute)
nmap -sP <IP>                # Escaneo de ping (descubre hosts activos)
nmap -Pn <IP>                # Escaneo sin ping (asume que el host está activo)
nmap -T4 <IP>                # Escaneo rápido (ajusta la velocidad)
nmap -iL <archivo.txt>       # Escaneo de múltiples hosts desde un archivo
nmap --script <script.nse> <IP> # Ejecutar script NSE específico
nmap -sV --version-all <IP>  # Detección exhaustiva de versiones
nmap -O --osscan-guess <IP>  # Detección de OS con suposiciones
nmap -A -sC <IP>             # Escaneo avanzado con scripts predeterminados
```

---

## Escaneos completos y combinados

```bash
nmap -sS -p 1-65535 <IP>         # Todos los puertos TCP
nmap -sU -p 1-65535 <IP>         # Todos los puertos UDP
nmap -sS -Pn <IP>                # TCP SYN sin ping
nmap -sU -Pn <IP>                # UDP sin ping
nmap -sS -T4 -p 1-65535 <IP>     # TCP rápido todos los puertos
nmap -sU -T4 -p 1-65535 <IP>     # UDP rápido todos los puertos
nmap -sS -iL <archivo.txt>       # Múltiples hosts TCP desde archivo
nmap -sU -iL <archivo.txt>       # Múltiples hosts UDP desde archivo
```

---

## Scripts NSE útiles

```bash
nmap --script vuln <IP>                 # Escaneo de vulnerabilidades
nmap --script discovery <IP>            # Descubrimiento de servicios
nmap --script http-enum <IP>            # Enumeración HTTP
nmap --script smb-os-discovery <IP>     # Descubrimiento OS en SMB
nmap --script ssh-brute <IP>            # Fuerza bruta SSH
```

---

## Auditorías de seguridad avanzadas

```bash
nmap -sS -p 1-65535 --script vuln <IP>             # Vulnerabilidades TCP
nmap -sU -p 1-65535 --script vuln <IP>             # Vulnerabilidades UDP
nmap -sS -p 1-65535 --script http-enum <IP>        # HTTP en todos los puertos TCP
nmap -sU -p 1-65535 --script http-enum <IP>        # HTTP en todos los puertos UDP
nmap -sS -p 1-65535 --script smb-os-discovery <IP> # OS SMB TCP
nmap -sU -p 1-65535 --script smb-os-discovery <IP> # OS SMB UDP
nmap -sS -p 1-65535 --script ssh-brute <IP>        # Fuerza bruta SSH TCP
nmap -sU -p 1-65535 --script ssh-brute <IP>        # Fuerza bruta SSH UDP
```

---

## Escaneo de red y subredes

```bash
nmap -sn <IP>/24                        # Descubrir hosts activos en subred
nmap -sP <IP>/24                        # Ping en subred
nmap -sS -p 1-65535 <IP>/24             # Todos los puertos TCP en subred
nmap -sU -p 1-65535 <IP>/24             # Todos los puertos UDP en subred
nmap -A <IP>/24                         # Escaneo avanzado en subred
nmap -sS -Pn <IP>/24                    # TCP SYN sin ping en subred
nmap -sU -Pn <IP>/24                    # UDP sin ping en subred
nmap -sS -T4 <IP>/24                    # TCP rápido en subred
nmap -sU -T4 <IP>/24                    # UDP rápido en subred
nmap -sS -iL <archivo.txt>              # Múltiples hosts TCP desde archivo en subred
nmap -sU -iL <archivo.txt>              # Múltiples hosts UDP desde archivo en subred
nmap --script vuln <IP>/24              # Vulnerabilidades en subred
nmap --script discovery <IP>/24         # Descubrimiento de servicios en subred
nmap --script http-enum <IP>/24         # HTTP en subred
nmap --script smb-os-discovery <IP>/24  # OS SMB en subred
nmap --script ssh-brute <IP>/24         # Fuerza bruta SSH en subred
```

---

## Comandos adicionales útiles para eJPTv2

```bash
nmap -f <IP>                       # Fragmenta paquetes para evadir firewalls
nmap --data-length 50 <IP>         # Añade datos aleatorios para evadir IDS
nmap --reason <IP>                 # Muestra la razón de cada resultado
nmap -oN resultado.txt <IP>        # Exporta resultados en formato normal
nmap -oX resultado.xml <IP>        # Exporta resultados en XML
nmap -oG resultado.gnmap <IP>      # Exporta resultados en formato grepable
nmap --top-ports 100 <IP>          # Escanea los 100 puertos más comunes
nmap --traceroute <IP>             # Incluye traceroute en el escaneo
nmap --script-help <script>        # Muestra ayuda sobre un script NSE
```