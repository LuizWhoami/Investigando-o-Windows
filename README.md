# Investigando-o-Windows

Apenas estou comparilhando como passei por todas as fases do laboratório 
Para começarmos temos que ter a em mente que iremos ter varias pesquisas em forums e comandos Windows para adiantarmos alguns processos

# INVESTIGANDO --

# 1. Qual é a versão e o ano da máquina de janelas?

 Comando no powershell: systeminfo

# 2. Qual usuário fez login por último?

 Comando no powershell: Get-LocalUser | Select Name, LastLogon

# 3. Quando é que o John entrou no sistema pela última vez?
 [Formato da resposta: MM/DD/AAAA H:MM:SS AM/PM]

 Comando no powershell: Net user John

# 4. A qual IP o sistema se conecta quando inicia pela primeira vez?

 Se refere ao primeiro IP que abre o CMD no inicio Da maquina

# 5. Quais duas contas tinham privilégios administrativos (exceto o usuário Administrador)?
  [Formato da resposta: username1, username2]

 Comando no powershell: Get-LocalGroupMember -group "Administrators"
 
  [Caso dê erro na resposta verifique os nomes em diferentes ordens =]
