# Análise de Incidente de Segurança com Wireshark – Infecção por Malware em Cliente Windows

## Visão Geral

Neste projeto, simulo a atuação de um analista em um Security Operations Center (SOC), investigando um incidente de segurança envolvendo a possível infecção de uma máquina Windows.

O caso começou a partir de um alerta de um usuário, informando que um colaborador havia baixado um arquivo suspeito ao procurar por **Google Authenticator**. A partir disso, foi disponibilizada uma captura de tráfego de rede (PCAP) para análise.

Com base nesse material, realizei a investigação correlacionando o tráfego com possíveis indicadores de comprometimento, seguindo uma abordagem prática de análise de rede.



## Cenário do Incidente

* Um usuário reportou comportamento suspeito após o download de um arquivo
* A busca realizada estava relacionada ao **Google Authenticator**
* Foram considerados indicadores baseados em informações públicas de threat intelligence
* A infecção da máquina foi confirmada
* Um arquivo PCAP foi fornecido para investigação



## Objetivo da Análise

O objetivo principal foi identificar os dados do host comprometido:

* Endereço IP
* Endereço MAC
* Hostname



## Metodologia

### 1. Identificação do Endereço IP

Filtro utilizado:

```
http.request.method == "GET"
```

Ao aplicar esse filtro, observei que apenas um endereço IP aparecia como origem das requisições HTTP:

**10.1.17.215**

Esse comportamento chamou atenção porque malwares frequentemente realizam requisições HTTP para comunicação externa. Por isso, considerei esse IP como o principal candidato a host infectado.


### 2. Identificação do Hostname

Filtro utilizado:

```
dns.flags.response == 1
```

A partir das respostas DNS:

* Analisei os pacotes retornados
* Utilizei a função **Follow UDP Stream**
* Extraí informações diretamente do fluxo de comunicação

Com isso, identifiquei o hostname:

**DESKTOP-L8C5GSJ**

Esse tipo de informação aparece naturalmente em processos de resolução de nomes feitos pelo próprio sistema.


### 3. Identificação do Endereço MAC

Utilizando o mesmo filtro DNS:

```
dns.flags.response == 1
```

Segui os seguintes passos:

* Selecionei um pacote DNS
* Naveguei até a camada **Ethernet II**
* Identifiquei o endereço MAC de origem

Resultado:

**00:d0:b7:26:4a:74**

O endereço MAC permite identificar o dispositivo de forma única dentro da rede local.


## Indicadores de Comprometimento (IoCs)

* IP suspeito: 10.1.17.215
* Hostname: DESKTOP-L8C5GSJ
* MAC Address: 00:d0:b7:26:4a:74


## Análise e Conclusão

A análise do tráfego mostrou que toda a atividade suspeita partia de um único host na rede. Ao correlacionar dados de diferentes camadas (HTTP, DNS e Ethernet), foi possível identificar de forma consistente a máquina comprometida.

O padrão de comportamento observado é compatível com infecção por malware, especialmente pela presença de comunicação externa via HTTP, que pode estar relacionada a download de payloads adicionais ou comunicação com servidores maliciosos.


## Ferramentas Utilizadas

* Wireshark


## Evidências

As evidências da análise estão disponíveis na pasta `/screenshots`, incluindo:

* Aplicação do filtro HTTP
* Análise de tráfego DNS
* Identificação do endereço MAC


## Habilidades Demonstradas

* Análise de tráfego de rede (Network Traffic Analysis)
* Investigação de incidentes (Incident Response)
* Identificação de indicadores de comprometimento (IoCs)
* Correlação de dados entre diferentes camadas de rede
* Raciocínio analítico aplicado a cenários de SOC
