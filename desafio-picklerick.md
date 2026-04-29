# Pickle Rick
### Sinopse
O Pickle Rick é um desafio de CTF disponível na plataforma **TryHackMe** que explora falhas comuns em serviços como listagem de diretórios e elevação de privilégios. As ferramentas utilizadas para a realização deste desafio foram o GoBuster e o Nmap, as quais fazem parte da suíte de ferramentas do Kali Linux, que serão utilizadas para enumeração de diretórios, portas e serviços.
## 1. Enumeração
O primeiro passo a ser realizado será o descobrimento de portas abertas e serviços que estão rodando na aplicação com o **Nmap**.
### Bloco de Comando:
```$ nmap -sV <endereço ip do servidor>```<br>
*# -sV : Detecta a versão dos serviços*<br>

A varredura com o nmap trouxe como resultado as portas abertas 80 e 22 (SSH) e que o servidor é um sistema Linux.

A seguir, realizarei a  listagem de diretórios existentes na aplicação web com o **GoBuster**.
### Bloco de Comando:
```$ gobuster dir -u http://<endereço ip do servidor> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt```<br>
*# dir : Busca por diretórios | -u : Define a URL | -w : Seleciona a wordlist a ser usada*<br>

Após a execução do programa, obtive os seguintes diretórios:<br>
- /robots.txt<br>
- /login.php<br>
- /portal.php<br>
- /assets<br>
## 2. Exploração
Observando o código-fonte da página inicial, pude encontrar o nome de usuário R1ckRul3s.<br>
Após isso, testando todos os diretórios encontrados anteriormente, encontrei uma página de login localizada em /login.php, /portal.php e um pequeno texto em /robots.txt que é a senha para o login.<br>
Com todos os dados encontrados até então, o atacante já possui as credenciais necessárias para validar seu acesso no painel de login.<br>
Ao realizar a validação das credenciais, é possível obter acesso ao painel de comandos em /portal.php.<br>

<img width="1091" height="132" alt="Command_Panel" src="https://github.com/user-attachments/assets/3d4dca57-48e1-4a0b-9edf-251cd4d2b5f6" />
<br>
Agora, com acesso ao painel de comandos, é possível inserir comandos no sistema.<br>
## 3. Navegação pelo Sistema
Ao inserir o comando **ls**, para listar o conteúdo disponível no diretório atual, o sistema retornou algumas pastas dentre as quais há um arquivo de texto chamado **‘Sup3rS3cretPickl3Ingred.txt’**.<br>
<br>
<img width="1092" height="246" alt="Command_Panel_1" src="https://github.com/user-attachments/assets/9bedab51-6975-4658-a739-7fffa0a75aac" />
<br>
No entanto, ao tentar ler o conteúdo do arquivo com o comando ```cat```, o sistema informa que o usuário não tem permissão para usá-lo.<br>

Sendo assim, é importante verificar se apenas cat não é permitido. Em seguida, testei com a alternativa ```tac```, e este por sua vez não estava bloqueado e trouxe o resultado desejado. Assim, a primeira resposta do desafio foi encontrada.<br>

<img width="1090" height="171" alt="Command_Panel_2" src="https://github.com/user-attachments/assets/ee2c7034-9dba-4add-8c6b-f4fa9e821128" />
<br>
Para prosseguir no desafio, é necessária a navegação por outros diretórios do sistema.<br>

A primeira a ser verificada foi a pasta **/home** com o comando ```ls /home```.<br>

E assim, o resultado retornado são os diretórios **rick** e **ubuntu**:<br>
<br>
<img width="1093" height="185" alt="Command_Panel_3" src="https://github.com/user-attachments/assets/cd3bd050-ae94-4eb0-a4c3-7db154179ed3" />
<br>
Acessando o conteúdo da pasta **rick**, é possível encontrar outra pasta cujo nome sugere que seu conteúdo seja a segunda flag do CTF.<br>
<br>
<img width="1088" height="171" alt="Command_Panel_4" src="https://github.com/user-attachments/assets/37876440-3e4b-4597-b3db-8160ca35a0fd" />
<br>
Assim, navegando até seu conteúdo '```ls /home/rick/second \ingredients```', é possível obter a segunda resposta:<br>
<br>
<img width="1088" height="171" alt="Command_Panel_5" src="https://github.com/user-attachments/assets/10296e06-5b1f-4dd0-aced-a6c1e81e678e" />
<br>
## 4. Escalação de Privilégios
Para encontrar a terceira e última flag do desafio, é necessário obtermos permissão de root, portanto ao tentar elevar a permissão do usuário atual **www-data** com o comando ```sudo -l```, o sistema informa que o usuário **www-data** possui permissão para utilizar todos os comandos do sistema, sem necessidade de ter que inserir a senha para ‘sudo’.<br>
<br>
<img width="1090" height="212" alt="Command_Panel_sudo-l" src="https://github.com/user-attachments/assets/4780b3a7-e743-4c9c-a729-74cef4000012" />
<br>
Dessa forma, ao navegar para o diretório **/root** com o comando sudo ```ls /root```, é possível encontrar as pastas:<br>
- 3rd.txt<br>
- snap<br>
O nome do arquivo **3rd.txt** sugere que este contenha a última resposta, logo, ao acessá-lo com ```sudo ls /root/3rd.txt```, é possível encontrar **‘3rd ingredients: fleeb juice’**, que é o último ítem necessário para completar o desafio.<br>
<br>
<img width="1092" height="172" alt="Command_Panel_7" src="https://github.com/user-attachments/assets/4b8dc6d4-9a2f-4cc6-be48-adf872f057bd" />
<br>
E assim, o CTF está completo! ✅
