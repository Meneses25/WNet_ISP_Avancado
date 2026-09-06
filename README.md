# 🌐 WNET — Simulação de ISP Avançado

## 📌 Sobre o projeto

A **WNET** é uma simulação de infraestrutura de um Provedor de Serviços de Internet (ISP), desenvolvida no **Cisco Packet Tracer** com o objetivo de aplicar conceitos de redes em um ambiente inspirado na arquitetura de uma operadora.

O projeto contempla desde a conectividade da provedora com múltiplos upstreams até a entrega do serviço para clientes residenciais e corporativos, passando por backbone redundante, roteamento dinâmico, IPv4/IPv6, FTTH, NAT, CGNAT, controle de banda e monitoramento.

Além da implementação da infraestrutura, o projeto envolveu testes de conectividade, análise de rotas e troubleshooting de problemas encontrados durante a construção da rede.

---

## 🎯 Objetivos

O projeto foi desenvolvido para consolidar conhecimentos relacionados à infraestrutura e operação de redes de provedores, incluindo:

- construção de uma arquitetura hierárquica de ISP;
- implementação de um backbone com caminhos redundantes;
- roteamento interno utilizando OSPFv2;
- roteamento IPv6 utilizando OSPFv3;
- conectividade externa através de eBGP;
- implementação de IPv4 e IPv6;
- representação de uma rede de acesso FTTH;
- atendimento de clientes residenciais e corporativos;
- utilização de NAT/PAT e simulação de CGNAT;
- aplicação de Traffic Shaping;
- implementação de DHCP e DNS;
- monitoramento utilizando SNMP e Syslog;
- análise e resolução de problemas de conectividade.

---

## 🏗️ Arquitetura da rede

A infraestrutura da WNET foi organizada em diferentes camadas.

### Borda

A camada de borda é formada por dois roteadores:

- `R-BORDA01`
- `R-BORDA02`

Cada roteador possui conexão com um upstream independente.

A WNET utiliza o **AS 65010**, enquanto os provedores externos simulados utilizam:

- `UPSTREAM-A` — AS 65001
- `UPSTREAM-B` — AS 65002

O prefixo `203.0.113.0/24` é utilizado para representar o bloco IPv4 anunciado pela WNET.

Essa arquitetura permite representar um cenário de **multihoming**, no qual a provedora possui mais de um caminho para conectividade externa.

### Core

O núcleo da rede é composto por:

- `R-CORE01`
- `R-CORE02`

Os equipamentos são responsáveis pelo transporte do tráfego entre as camadas de borda e distribuição.

### Distribuição

A camada de distribuição é formada por:

- `R-DA01`
- `R-DA02`

Ela conecta o core ao POP da rede, mantendo caminhos alternativos através da infraestrutura.

### POP e acesso

O `S-POP` concentra a conexão entre o backbone e a infraestrutura de acesso.

A partir dele, a rede segue para a `OLT-CENTRAL`, responsável pela representação da entrega dos serviços FTTH.

---

## 🧰 Tecnologias utilizadas

### OSPFv2

O **OSPFv2** foi utilizado como protocolo de roteamento interno IPv4 da WNET.

Ele permite que os roteadores do backbone aprendam dinamicamente as redes existentes na infraestrutura, reduzindo a necessidade de rotas estáticas e possibilitando a utilização de caminhos alternativos.

---

### OSPFv3

O **OSPFv3** foi utilizado no backbone IPv6.

A implementação permitiu criar uma infraestrutura dual-stack e fornecer roteamento dinâmico para os prefixos IPv6 utilizados no projeto.

---

### BGP

O **eBGP** foi utilizado para representar a comunicação da WNET com provedores upstream.

Foram utilizados três sistemas autônomos:

| Rede | ASN |
|---|---:|
| WNET | 65010 |
| UPSTREAM-A | 65001 |
| UPSTREAM-B | 65002 |

A WNET anuncia o prefixo:

`203.0.113.0/24`

A utilização de duas bordas permite representar uma provedora conectada a múltiplos sistemas autônomos externos.

---

### IPv4 e IPv6

A infraestrutura foi desenvolvida utilizando **dual-stack** em parte do ambiente.

O IPv4 é utilizado no backbone, clientes residenciais, cliente corporativo e conectividade com a Internet simulada.

O IPv6 foi implementado no backbone utilizando OSPFv3 e também nos segmentos residenciais.

A conectividade IPv6 fim a fim foi validada utilizando um endereço pertencente ao prefixo residencial e um servidor IPv6 externo simulado.

---

## 🏠 Acesso FTTH residencial

A rede de acesso foi construída para representar, dentro das limitações do Packet Tracer, uma infraestrutura FTTH.

Foram utilizados:

- OLT;
- CTOs;
- bridges representando ONUs/ONTs;
- roteadores residenciais;
- clientes cabeados;
- clientes Wi-Fi.

Os clientes foram distribuídos em diferentes VLANs residenciais.

A infraestrutura permite representar o caminho percorrido pelo tráfego desde a residência do assinante até o backbone da provedora.

---

## 🔄 NAT e CGNAT

Os clientes residenciais utilizam endereçamento pertencente ao espaço `100.64.0.0/10`, reservado para cenários de Carrier-Grade NAT.

No projeto, o comportamento de **CGNAT** foi representado através de NAT/PAT nos roteadores de borda.

O fluxo simplificado é:

Cliente residencial → roteador residencial → rede da WNET → NAT/PAT na borda → Internet simulada.

Essa implementação permite demonstrar o compartilhamento de endereços de saída entre múltiplos clientes.

---

## 🚦 Traffic Shaping

O **Traffic Shaping** foi utilizado para representar o controle de velocidade dos planos contratados pelos clientes.

As políticas foram aplicadas aos segmentos residenciais, permitindo simular diferentes limites de banda dentro da infraestrutura da provedora.

---

## 🏢 Cliente corporativo — NEXUCORP

Além dos clientes residenciais, foi implementado um cliente corporativo denominado **NEXUCORP**.

A empresa recebe conectividade da WNET através de uma VLAN de serviço dedicada.

A entrega utiliza:

- `VLAN 310 — NEXUCORP`;
- ONT dedicada;
- rede WAN `172.16.100.0/30`;
- `172.16.100.1` no lado da WNET;
- `172.16.100.2` no roteador da NEXUCORP.

A rede interna da empresa permanece sob administração do próprio cliente e possui segmentação independente da infraestrutura da provedora.

O roteador corporativo realiza NAT/PAT antes de encaminhar o tráfego para a WNET.

Dessa forma, a provedora não precisa conhecer as sub-redes internas utilizadas pela empresa.

---

## 📡 DHCP e DNS

O DHCP foi utilizado para fornecer automaticamente parâmetros de rede aos clientes.

O serviço distribui informações como:

- endereço IPv4;
- máscara;
- gateway padrão;
- servidor DNS.

O DNS permite que os clientes utilizem nomes em vez de acessar os serviços apenas através de endereços IP.

A resolução de nomes foi validada tanto em clientes residenciais quanto no ambiente corporativo.

---

## 📊 Monitoramento

### SNMP

O **SNMP** foi configurado nos equipamentos da infraestrutura para representar a coleta de informações de monitoramento da rede.

### Syslog

Os equipamentos enviam eventos para um servidor central de gerenciamento (`SRV-NMS`).

O Syslog permite concentrar mensagens relacionadas a eventos da infraestrutura, incluindo alterações de estado e adjacências de protocolos de roteamento.

Essa centralização facilita atividades de operação e troubleshooting.

---

## 🔁 Redundância

A WNET possui redundância em diferentes pontos da infraestrutura.

Foram utilizados:

- dois roteadores de borda;
- dois roteadores de core;
- dois roteadores de distribuição;
- dois upstreams;
- caminhos alternativos no backbone.

O OSPF permite a convergência do roteamento IPv4 diante de alterações na topologia.

O backbone IPv6 também possui caminhos internos alternativos utilizando OSPFv3.

A saída IPv6 para a Internet simulada foi mantida através de uma borda principal devido às limitações encontradas no Packet Tracer.

---

## 🔧 Troubleshooting realizado

Durante a construção do projeto foram encontrados e solucionados diferentes problemas.

Entre eles:

- inconsistências de gateway;
- problemas de roteamento;
- ausência de rotas;
- configuração incorreta de NAT inside/outside;
- redes ausentes das ACLs utilizadas pelo NAT;
- perda parcial de conectividade causada por diferenças de configuração entre as duas bordas;
- problemas de retorno de tráfego;
- limitações relacionadas ao IPv6 em roteadores residenciais do Packet Tracer;
- comportamento inconsistente do HTTP IPv4 através de determinados cenários de NAT no simulador.

Um dos casos ocorreu durante a integração da NEXUCORP.

Inicialmente, os dispositivos internos conseguiam alcançar o próprio roteador corporativo, mas não conseguiam atravessar corretamente a WAN. A análise das interfaces, rotas, ACLs e estatísticas de NAT permitiu identificar uma interface WAN configurada incorretamente como `ip nat inside`.

Posteriormente, foi observada perda parcial de pacotes para a Internet simulada. O problema foi isolado à segunda borda da WNET, cuja ACL de NAT ainda não contemplava a rede WAN do cliente corporativo.

Após a padronização das duas bordas, a conectividade passou a apresentar sucesso integral nos testes realizados.

---

## 🧪 Validação

Foram realizados testes envolvendo:

- conectividade IPv4;
- conectividade IPv6;
- resolução DNS;
- comunicação através do backbone;
- roteamento OSPF;
- roteamento OSPFv3;
- sessões e rotas BGP;
- NAT/PAT;
- CGNAT;
- acesso residencial;
- acesso corporativo;
- redundância;
- monitoramento por Syslog.

Os testes foram realizados a partir de diferentes pontos da infraestrutura para validar o funcionamento fim a fim.

---

## ⚠️ Limitações da simulação

O projeto foi desenvolvido no Cisco Packet Tracer e, portanto, algumas funcionalidades apresentam limitações em relação a equipamentos e sistemas operacionais utilizados em redes de produção.

Entre as limitações encontradas estão:

- suporte limitado a determinadas funcionalidades BGP para IPv6;
- limitações de IPv6 nos roteadores residenciais disponíveis no simulador;
- comportamento inconsistente do HTTP IPv4 em determinados cenários envolvendo NAT/PAT;
- simplificações necessárias para representar elementos de uma rede FTTH.

Essas limitações foram consideradas durante os testes e documentadas sem alterar o objetivo principal da simulação.

---

## 📚 Conhecimentos aplicados

Durante o desenvolvimento foram aplicados conceitos relacionados a:

`IPv4` • `IPv6` • `Subnetting` • `VLAN` • `802.1Q` • `DHCP` • `DNS` • `NAT` • `PAT` • `CGNAT` • `OSPFv2` • `OSPFv3` • `BGP` • `SNMP` • `Syslog` • `Traffic Shaping` • `FTTH` • `Redundância` • `Troubleshooting`

---

## 🛠️ Ferramenta

Projeto desenvolvido utilizando **Cisco Packet Tracer**.

---

## 👤 Autor
Nalberty Meneses
**Nalberty Meneses**

Projeto desenvolvido para estudo, prática e portfólio na área de Redes de Computadores e Infraestrutura.
