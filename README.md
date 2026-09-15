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

## Investigação do IP de envio

Durante a análise dos dados da campanha, o endereço IP `2.110.26.249` foi selecionado para enriquecimento e consulta em fontes de Threat Intelligence.

A primeira consulta foi realizada no VirusTotal.

![Consulta do IP no VirusTotal](evidence/evidence-02-ip-virustotal.png)

Na consulta atual, nenhum dos mecanismos de segurança disponíveis no VirusTotal classificou o endereço como malicioso.

Como o incidente analisado ocorreu em 2015, a reputação atual do IP não foi utilizada como confirmação de que o endereço era legítimo ou malicioso na época da campanha.

Também foi realizada uma consulta no RIPE Database para obter informações sobre o bloco de rede associado ao endereço.

![Consulta do IP no RIPE](evidence/evidence-03-ip-ripe-lookup.png)

A consulta atual mostrou que o endereço pertence ao bloco 2.110.0.0 - 2.110.255.255, associado à Dinamarca, e que a rota observada possui origem no ASN AS8999.

## Análise do domínio relacionado ao incidente

Após a análise do IP de envio, foi realizada uma consulta ao domínio `waveonnord.com`, identificado na infraestrutura associado ao incidente.

A primeira verificação foi feita no VirusTotal.

![Consulta do domínio no VirusTotal](evidence/evidence-04-domain-virustotal.png)

A URL associada ao domínio apresentava histórico de submissão desde 2014. Na consulta atual, apenas um mecanismo de segurança apresentou detecção.

Como o incidente analisado ocorreu em 2015, a reputação atual foi utilizada apenas como enriquecimento do indicador e não como prova isolada da atividade maliciosa.

Também foram analisadas as relações do domínio no VirusTotal.

![Relações do domínio no VirusTotal](evidence/evidence-05-domain-relations.png)

O histórico de resolução DNS apresentou o endereço IP `62.149.177.11`, que também aparece associado à infraestrutura documentada para o incidente analisado.

Foram observadas relações com arquivos detectados em anos posteriores. Como essas relações são muito mais recentes que o incidente analisado, elas não foram atribuídas diretamente ao incidente de 2015.

## Análise do arquivo malicioso

Após a identificação da infraestrutura utilizada no phishing, foi realizada a análise do arquivo associado ao incidente sem realizar sua execução.

O arquivo `doc255_pdf.exe` foi investigado através de seu hash no VirusTotal.

![Detecção do malware no VirusTotal](evidence/evidence-06-malware-virustotal.png)

Na consulta realizada, o arquivo apresentou detecção 63 de 68 mecanismos de segurança no momento da consulta e foi classificado principalmente como Trojan/Downloader, com referências à família Upatre.

Apesar do nome conter a palavra `pdf`, a análise mostrou que o arquivo era na realidade um executável do Windows.

Também foram consultadas as propriedades do arquivo para coletar seus principais identificadores.

![Detalhes do arquivo malicioso](evidence/evidence-07-malware-file-details.png)

Os principais hashes identificados foram:

- **MD5:** `b4157a9f819a9aeb401a4b7784f7916e`
- **SHA-1:** `fe384a3c1cfa7727f7f07227e5556ae716a55a217`
- **SHA-256:** `8cd4a12cf21a4e1f9bf2da069be51b52c777328ae6ed87ce29b495412773cd72`

Esses hashes podem ser utilizados como indicadores de comprometimento para buscas em ferramentas como SIEM, EDR e soluções de Threat Intelligence.
