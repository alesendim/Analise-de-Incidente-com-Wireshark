# Incident Report – Malware Infection on Windows Client

Este incidente foi identificado a partir do reporte de um usuário, que informou que um colaborador havia baixado um arquivo suspeito ao procurar por “Google Authenticator”.

A partir desse alerta, foi realizada a análise de um arquivo de captura de rede (PCAP). Durante a investigação, foi possível identificar um host específico apresentando comportamento compatível com atividade maliciosa, principalmente por meio de requisições HTTP e resolução de nomes via DNS.

Ao final da análise, o dispositivo comprometido foi identificado com sucesso, junto com seus principais indicadores.


## Detalhes do incidente

* **Tipo de incidente:** Possível infecção por malware
* **Origem do alerta:** Reporte de usuário
* **Método de análise:** Network Traffic Analysis (Wireshark)
* **Ambiente:** Rede interna


## Objetivo da Investigação

A análise teve como foco:

* Identificar qual máquina estava comprometida
* Entender o comportamento de rede desse host
* Extrair indicadores de comprometimento (IoCs)


## Resumo da Análise

A investigação foi conduzida analisando o tráfego de rede em diferentes níveis, buscando padrões típicos de malware.

### 1. Atividade HTTP

Ao filtrar requisições HTTP GET, foi possível observar que apenas um endereço IP gerava esse tipo de tráfego.

Isso chamou atenção, já que malwares frequentemente utilizam HTTP para comunicação com servidores externos.


### 2. Tráfego DNS

A análise das respostas DNS permitiu entender como esse host estava resolvendo nomes na rede.

Ao inspecionar os pacotes e utilizar o recurso de análise de fluxo (Follow UDP Stream), foi possível identificar o hostname da máquina.


### 3. Identificação do Dispositivo

Por fim, analisando a camada Ethernet (Ethernet II), foi possível obter o endereço MAC do dispositivo, confirmando sua identidade dentro da rede local.


## Indicadores de Comprometimento (IoCs)

* **IP Address:** 10.1.17.215
* **Hostname:** DESKTOP-L8C5GSJ
* **MAC Address:** 00:d0:b7:26:4a:74


## Constatações

* Apenas um host apresentou atividade HTTP relevante
* O comportamento observado é compatível com comunicação externa típica de malware
* A correlação entre HTTP, DNS e Ethernet permitiu identificar o dispositivo de forma consistente


## Possível Impacto

* Comprometimento do endpoint
* Comunicação com possíveis servidores externos maliciosos


## Ações Recomendadas

* Isolar o host da rede o mais rápido possível
* Realizar análise mais aprofundada no endpoint (forense)
* Bloquear conexões suspeitas identificadas


## Conclusão

Com base na análise do tráfego de rede, foi possível identificar um único host com comportamento suspeito e compatível com infecção por malware.

A investigação permitiu correlacionar diferentes tipos de tráfego e chegar de forma consistente ao dispositivo comprometido.

Esse tipo de análise reforça como o monitoramento de rede pode ser essencial para detectar e responder rapidamente a incidentes de segurança.

