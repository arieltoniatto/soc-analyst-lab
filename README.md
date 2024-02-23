<h1>SOC Analyst Lab</h1>

### [So you want to be a SOC Analyst?](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-part)

<h2>Descrição</h2>
Esse projeto fornece experiência prática do ciclo de vida de um incidente de segurança. A demonstração começa com a criação de um Ubuntu Server como adversário e um Windows 11 como vítima usando VMWare. Feito isso, cria-se o malware, infectando a VM do Windows, e feita a análise dos logs da máquina infectada no EDR LimaCharlie.
Regras de Detecção e Resposta foram criadas baseadas no malware, e as regras são aperfeiçoadas para prevenir falsos positivos.

<br />


<h2>Utilitários usados</h2>

- <b>VMWare</b>
- <b>PowerShell</b>
- <b>Sliver</b>
- <b>LimaCharlie</b>

<h2>Ambientes usados</h2>

- <b>Ubuntu Server</b>
- <b>Windows 11</b> (22H2)

<h2>Passo a passo do projeto:</h2>

<p align="center">
<b>Criação do ambiente</b> <br/>
  Primeiro, foi criado um ambiente virtual onde a máquina atacante é um Ubuntu Server e o alvo é o Windows 11. Após a instalação, foi desabilitado o Windows Defender, desabilitada a função de que o Windows entre em standby e instalado o Sysmon para possibilitar uma telemetria do endpoint. Também foi instalado o LimaCharlie EDR.
  No sistema atacante foi usado o SSH para acessar a máquina e instalado o Sliver, que é um framework de Command & Control (C2).
<br />
<br />

<p align="center">
<b>Criação do payload</b> <br/>
  No Ubuntu, foi criado o payload usando o Sliver e com um servidor HTTP temporário para que fosse possível baixá-lo no Windows através do PowerShell e executá-lo. Feito isso, deu-se início a sessão de C2.
  <img src="https://i.imgur.com/40TWH8r.png" height="80%" width="80%" alt="Sliver Payload"/>
  <img src="https://i.imgur.com/koeGt6Q.png" height="80%" width="80%" alt="Python HTTP server"/>
  <img src="https://i.imgur.com/MUgsHQy.png" height="80%" width="80%" alt="Power Shell Download"/>
  <img src="https://i.imgur.com/qxHESTd.png" height="80%" width="80%" alt="Sliver Session"/>

<p align="center">
  Através da telemetria do LimaCharlie EDR, foi possível observar os processos que estavam rodando na máquina, inclusive o malware.
  <img src="https://i.imgur.com/cGiVU4B.png" height="80%" width="80%" alt="LimaCharlie Processes"/>

<p align="center">
  O EDR também oferece a possibilidade de escanear com o VirusTotal. No entanto, como o malware foi recém-criado, o VirusTotal o aponta como item não encontrado devido ao seu hash nunca ter sido visto.
  <img src="https://i.imgur.com/RfDY5Zc.png" height="80%" width="80%" alt="LimaCharlie Processes"/>
  
<br />
<br />

<p align="center">
<b>Dump de credenciais</b> <br/>
  Através do Sliver, foi realizado o dump de credenciais lsass.
  <img src="https://i.imgur.com/v1ffc5O.png" height="80%" width="80%" alt="Credential dump"/>

<p align="center">
   Isso possibilitou a análise do evento no LimaCharlie.
  <img src="https://i.imgur.com/1C7AxNb.png" height="80%" width="80%" alt="Report Sensitive Process Access"/>

  <p align="center">
  Durante a análise, foi criada uma regra de detecção e resposta (D&R) na detecção de SENSITIVE_PROCESS_ACCESS com o processo terminando com lsass.exe e gerando um relatório. 
  <img src="https://i.imgur.com/Mngjz70.png" height="80%" width="80%" alt="D&R rule lssas"/>
<br />
<br />

<p align="center">
<b>Ataque ao Volume Shadow e Contramedidas</b> <br/>
  Foi feito um ataque visando deletar todas as cópias do Volume Shadow. Como o VSS é usado para a recuperação de dados, é muito comum que um ataque de ransomware vise esse serviço.
  <img src="https://i.imgur.com/R2zXzYU.png" height="80%" width="80%" alt="D&R rule lssas"/>

<p align="center">
  O ataque foi identificado pelo EDR e, a partir disso, foi criada uma regra para detectar a atividade em vssadmin.exe em conjunto com o comando Delete Shadows /All. Também foi criada uma resposta para reportar essa atividade e matar o processo pai responsável.
  <img src="https://i.imgur.com/nj3xXSk.png" height="80%" width="80%" alt="D&R rule lssas"/>
  <img src="https://i.imgur.com/6BmzZXI.png" height="80%" width="80%" alt="D&R rule lssas"/>
<br />
<br />

<p align="center">
<b>Conclusão</b> <br />
  A simulação de ataques reais é uma ferramenta valiosa para melhorar a cibersegurança. Através dela, as organizações podem testar suas defesas e identificar pontos fracos.
  Neste caso, a simulação permitiu que eu desenvolvesse novas regras de detecção e resposta que podem ajudar a proteger as organizações contra ataques reais.

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
