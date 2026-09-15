# Investigação de Phishing com Análise de PCAP

## Objetivo

O objetivo deste projeto é realizar a investigação de um incidente de phishing, identificando indicadores de comprometimento (IOCs) e investigando o tráfego de rede relacionado ao ataque.

Durante a investigação foram analisados remetentes spoofed, endereços IP, domínios, arquivos maliciosos e comunicações de rede presentes em uma captura PCAP.

A análise foi realizada utilizando ferramentas como Wireshark, VirusTotal e RIPE, além do mapeamento das evidências encontradas com o framework MITRE ATT&CK.

## Ferramentas utilizadas

- Wireshark
- VirusTotal
- RIPE Database
- MITRE ATT&CK
- Excel

## Fonte do laboratório

Os dados utilizados neste projeto foram obtidos a partir de um exercício disponibilizado pelo Malware-Traffic-Analysis.net, referente a um caso de phishing com entrega de malware Upatre/Dyre.

A análise foi realizada de forma defensiva, sem execução das amostras maliciosas.

## Análise inicial do phishing

A primeira etapa da investigação foi analisar os dados relacionados aos e-mails enviados durante o incidente.

O arquivo analisado apresentava informações sobre o remetente utilizado, hosts de envio, endereços IP e o assunto das mensagens.

![Dados dos remetentes da campanha](evidence/evidence-01-phishing-sender-data.png)

Foi identificado o uso recorrente do remetente spoofed:

`bankline.administrator@nutwest.com`

Também foi possível observar diversos hosts e endereços IP diferentes utilizados para o envio das mensagens, enquanto o padrão do assunto permanecia semelhante:

`Payment Advice - Advice Ref:[...] / CHAPS credits`

A repetição do mesmo remetente e do mesmo padrão de assunto, combinada com diferentes hosts e endereços IP de origem, indicou uma campanha de envio em massa.
