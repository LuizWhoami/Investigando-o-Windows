# Investigando-o-Windows

Apenas estou comparilhando como passei por todas as fases do laboratório 
Para começarmos temos que ter a em mente que iremos ter varias pesquisas em forums e comandos Windows para adiantarmos alguns processos

Nesta investigação, utilizamos uma série de técnicas forenses para analisar uma máquina Windows comprometida. Desde a análise de tarefas agendadas até a identificação de scripts maliciosos e envenenamento de DNS, cada etapa revelou mais detalhes sobre como o ataque ocorreu. Usando as ferramentas e comandos corretos, uma abordagem sistemática como essa pode ajudar profissionais de cibersegurança a reconstruir o que aconteceu durante a violação.

Como sempre, a prática leva à perfeição. Continue aprimorando suas habilidades de resposta a incidentes e forenses utilizando laboratórios como o TryHackMe para se manter preparado para os desafios reais de cibersegurança.

# INVESTIGANDO --

# 1. Qual é a versão e o ano da máquina de janelas?

 Comando no powershell: systeminfo

# 2. Qual usuário fez login por último?

 Comando no powershell: Get-LocalUser | Select Name, LastLogon

# 3. Quando é que o John entrou no sistema pela última vez?
 [Formato da resposta: MM/DD/AAAA H:MM:SS AM/PM]

 Comando no powershell: Net user John

# 4. A qual IP o sistema se conecta quando inicia pela primeira vez?

 Regedit > HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > Run

# 5. Quais duas contas tinham privilégios administrativos (exceto o usuário Administrador)?
  [Formato da resposta: username1, username2]

 Comando no powershell: Get-LocalGroupMember -group "Administrators"
 
  [Caso dê erro na resposta verifique os nomes em diferentes ordens =]

# 6. Qual é o nome da tarefa agendada que é máliciosa.

 Comando no powershell: Get-ScheduledTask | Where-Object {$_.TaskPath -eq "\"}

# 7. Qual arquivo a tarefa estava tentando executar diariamente?

 Comando no powershell: $task = Get-ScheduledTask | Where TaskName -EQ "Clean file system"
 Comando no powershell para verificar as ações: $task.Actions

# 8. Para que porta esse arquivo ouviu localmente?

 A porta se localiza no mesmo comando de verificação da tarefa que estava tentando ser executada

# 9. Quando foi a última vez que a Jenny fez o login?

 Comando no powershell: net user Jenny | findstr "Last"

# 10. Em que data o compromisso foi alcançado?
 [Formato da resposta: MM/DD/AAAA]

 Esta data eu identifiquei lá no net user john, mas por uma lógica de login

# 11. Durante o compromisso, em que momento o Windows primeiro atribuiu privilégios especiais a um novo logon?

 Formato da resposta: MM/DD/AAAA HH:MM:SS AM/PM

 Bom iremos em Event viewer > Windows Logs > Security
 iremos navegar nos logs de segurança do windows até ter uma demanda extrema sobre um grupo de segurança e logo em seguida verá um Login e dps um Security especial e é ele que queremos, conseguiu entender? que depois da tentativa de ataque veio a defesa acionada e pós o login

# 12. Qual ferramenta foi usada para obter senhas do Windows?

 iremos na vegar até a pasta pelo powershell C:\TMP e pós sabermos o nome do arquivo iremos dar o seguinte comando> Get-FileHash {nome do arquivo}

 # 13. Qual foi o IP dos servidores de controle e comando externos dos atacantes?

  Comando do powershell: ipconfig /displaydns | findstr "Record" | findstr "Name Host"

# 14. Qual era o nome da extensão do shell carregado através do site dos servidores?

 navegue até C:\inetpub\wwwroot e veremos o nome da extensão para abertura do arquivo

# 15. Qual foi a última porta que o atacante abriu?

 Comando do powershell: netsh firewall show state

# 16. Verifique se há envenenamento por DNS, qual site foi segmentado?

 Comando do powershell: type C:\WINDOWS\System32\drivers\etc\hosts
ira identificar o site segmentado
 
