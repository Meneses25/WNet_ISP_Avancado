# 🌐 WNET — ISP Avançado

Projeto de simulação de um Provedor de Serviços de Internet (ISP) desenvolvido no Cisco Packet Tracer, contemplando infraestrutura de backbone, redundância, roteamento dinâmico, conectividade IPv4/IPv6, acesso FTTH residencial e atendimento a cliente corporativo.

O projeto busca representar diferentes etapas envolvidas na operação de uma rede de provedor, desde a conectividade com upstreams até a entrega do serviço ao cliente final.

---

## 🎯 Objetivos

O projeto WNET foi desenvolvido com o objetivo de aplicar, de forma prática, conceitos relacionados à infraestrutura e operação de provedores de Internet.

Entre os principais objetivos estão:

- Construir uma arquitetura hierárquica e redundante de ISP;
- Implementar roteamento interno utilizando OSPF;
- Implementar OSPFv3 para a infraestrutura IPv6;
- Simular conectividade externa utilizando eBGP;
- Implementar conectividade IPv4 e IPv6;
- Representar uma infraestrutura de acesso FTTH;
- Simular atendimento a clientes residenciais;
- Implementar NAT/PAT e um cenário de CGNAT;
- Aplicar Traffic Shaping para representar diferentes planos de acesso;
- Centralizar eventos da infraestrutura através de Syslog;
- Utilizar SNMP para monitoramento dos equipamentos;
- Implementar e integrar um cliente corporativo à infraestrutura da provedora;
- Trabalhar com redundância e caminhos alternativos;
- Realizar troubleshooting de problemas de roteamento e NAT.

---

## 🗺️ Topologia

<img width="1767" height="2525" alt="Topologia do provedor" src="https://github.com/user-attachments/assets/b814095d-b6ec-435c-930d-11dbe163a859" />


A infraestrutura foi dividida em diferentes camadas, permitindo separar as funções de borda, núcleo, distribuição e acesso.

De forma simplificada:

UPSTREAM-A / UPSTREAM-B  
↓  
R-BORDA01 / R-BORDA02  
↓  
R-CORE01 / R-CORE02  
↓  
R-DA01 / R-DA02  
↓  
S-POP  
↓  
OLT-CENTRAL  
↓  
Clientes residenciais / Cliente corporativo

A utilização de equipamentos redundantes em diferentes pontos da infraestrutura permite a existência de caminhos alternativos no backbone.

---

## 🏗️ Arquitetura da WNET

### Borda

A camada de borda é composta pelos roteadores `R-BORDA01` e `R-BORDA02`.

Cada roteador possui conectividade com um upstream independente, permitindo simular uma arquitetura multihomed.

A WNET utiliza o AS:

`AS 65010`

Os upstreams utilizados na simulação são:

- UPSTREAM-A — `AS 65001`
- UPSTREAM-B — `AS 65002`

O prefixo utilizado para representar o bloco anunciado pela WNET é:

`203.0.113.0/24`

A conectividade externa IPv4 é estabelecida através de eBGP.

---

### Core

Os roteadores `R-CORE01` e `R-CORE02` formam a camada de núcleo da infraestrutura.

Essa camada realiza o transporte do tráfego entre a borda e a camada de distribuição, utilizando enlaces ponto a ponto e roteamento dinâmico.

---

### Distribuição

A distribuição é composta pelos equipamentos:

- `R-DA01`
- `R-DA02`

Eles conectam o núcleo da WNET ao `S-POP`, mantendo dois caminhos possíveis através da infraestrutura.

---

### POP e acesso

O `S-POP` concentra a conexão entre o backbone e a rede de acesso.

A partir dele, a infraestrutura segue para a `OLT-CENTRAL`, responsável pela representação da entrega dos serviços aos clientes FTTH.

A rede de acesso inclui:

- OLT;
- CTOs;
- ONUs/ONTs representadas por bridges;
- roteadores residenciais;
- clientes cabeados e Wi-Fi;
- cliente corporativo NEXUCORP.

---
