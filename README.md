<h1>SOC Analyst Lab</h1>

### [So you want to be a SOC Analyst?](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part)

<h2>Descrição</h2>
Esse projeto fornece experiência prática do cilco de vida de um incidente de segurança. A demonstração começa com a criação de um Ubuntu Server como adversário e um Windows 11 como vítima usando VMWare. Feito isso, cria-se o malware, infectando a VM do Windows, e feita a análise dos logs da máquina infectada no EDR LimaCharlie.
Regras de Detecção e Resposta foram criadas baseadas no malware, e as regras são aperfeiçoadas para prevenir falsos positivos.

<br />


<h2>Utilitários usados</h2>
 
- <b>PowerShell</b>
- <b>Sliver</b>
- <b>LimaCharlie</b>

<h2>Ambientes usados</h2>

- <b>Ubuntu Server</b>
- <b>Windows 11</b> (22H2)

<h2>Passo a passo do projeto:</h2>

<p align="center">
<h3>Criação do ambiente</h3> <br/>
  Primeiro, foi criado um ambiente virtual onde a máquina atacante é um Ubuntu Server e o alvo é o Windows 11. Após a instalação, foi desabilitado o Windows Defender, desabilitada a função de que o Windows entre em standby e instalado o Sysmon para possibilitar uma telemetria do endpoint. Também foi instalado o LimaCharlie EDR.
  No sistema atacante foi usado o SSH para acessar a máquina e instalado o Sliver, que é um framework de Command & Control (C2).
<br />
<br />
<h3>Criação do payload</h3> <br/>
  No Ubuntu, foi criado o payload usando o Sliver e com um servidor HTTP temporário para que fosse possível baixá-lo no Windows através do PowerShell e executá-lo. Feito isso, deu-se início a sessão de C2.
  <img src="https://i.imgur.com/40TWH8r.png" height="80%" width="80%" alt="Sliver Payload"/>
  <img src="https://i.imgur.com/koeGt6Q.png" height="80%" width="80%" alt="Python HTTP server"/>
  <img src="https://i.imgur.com/MUgsHQy.png" height="80%" width="80%" alt="Power Shell Download"/>
  <img src="https://i.imgur.com/qxHESTd.png" height="80%" width="80%" alt="Sliver Session"/>

  Através da telemetria do LimaCharlie EDR, foi possível observar os processos que estavam rodando na máquina, inclusive o malware.
  <img src="https://i.imgur.com/cGiVU4B.png" height="80%" width="80%" alt="LimaCharlie Processes"/>

  O EDR também oferece a possibilidade de escanear com o VirusTotal. No entanto, como o malware foi recém-criado, o VirusTotal o aponta como item não encontrado devido ao seu hash nunca ter sido visto.
  <img src="https://i.imgur.com/RfDY5Zc.png" height="80%" width="80%" alt="LimaCharlie Processes"/>
  
<br />
<br />
<h3>Dump de credenciais</h3> <br/>
  Através do Sliver, foi realizado o dump de credenciais lsass. Isso possibilitou a análise do evento no LimaCharlie. Durante a análise, foi criada uma regra de detecção e resposta (D&R) na detecção de SENSITIVE_PROCESS_ACCESS com o processo terminando com lsass.exe e gerando um relatório.
  <img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
