# INE Lab - Host Discovery & Service Enumeration with Nmap

## Objetivo
Identificar hosts ativos e serviços em execução na rede `demo.ine.local`, simulando um ambiente onde o ICMP está bloqueado.

## Metodologia

### 1. Teste Inicial com Ping
```bash
ping -c 4 demo.ine.local
```
- **Resultado**: 
  - Nenhum host respondendo (bloqueio de ICMP).
  - ![Falha no ping](Images/INE/ping_scan.png)  
  *Figura 1: Falha no ping devido a filtro ICMP*

### 2. Escaneamento Básico com Nmap
```bash
nmap demo.ine.local
```
- **Resultado**: 
  - Falso negativo (sem flag `-Pn`).

### 3. Escaneamento com Ignorar Status de Host (`-Pn`)
```bash
nmap -Pn demo.ine.local
```
- **Resultado**: 
  - 3 portas abertas (22/SSH, 80/HTTP, 443/HTTPS).
  - ![Resultado do scan -Pn](Images/INE/nmap_pn_scan.png)  
  *Figura 2: Scan com -Pn revelando portas abertas*

### 4. Detecção de Versões de Serviços (`-sV`)
```bash
nmap -Pn -sV demo.ine.local
```
- **Resultado**: 
  - Serviços identificados:
    - Apache 2.4.7 (CVE-2020-11984)
    - OpenSSH 7.4
  - ![Detecção de versões](Images/INE/nmap_sv_results.png)  
  *Figura 3: Versões de serviços detectadas*

## Conclusão
- O bloqueio de ICMP não indica host offline. A flag `-Pn` do Nmap contorna essa limitação.
- A enumeração de serviços (`-sV`) revelou versões vulneráveis (ex: Apache 2.4.7).
- **Próximos passos**: Testar vulnerabilidades com `--script vuln` ou Metasploit.

## Comandos Úteis
```bash
# Scan rápido em redes com ICMP bloqueado
nmap -Pn --top-ports 100 192.168.1.1

# Detecção agressiva de serviços
nmap -Pn -sV -sC -O demo.ine.local
```

## Referências
- [Nmap Documentation](https://nmap.org/book/man.html)
- [MITRE ATT&CK: Discovery](https://attack.mitre.org/tactics/TA0007/)
