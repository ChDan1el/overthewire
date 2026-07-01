## [natas0](http://natas0.natas.labs.overthewire.org)
###### Usuário: natas0
###### Senha: natas0

Ao entrar no desafio nos deparamos com essa dica:

<img width="623" height="157" alt="image" src="https://github.com/user-attachments/assets/9d4d364c-21ac-4223-841c-0a47c4d0c82e" />

Então, usando o DevTools, ou seja, inspecionando o HTML da página, conseguimos a flag como um comentário HTML

<img width="1397" height="645" alt="image" src="https://github.com/user-attachments/assets/5578c4a6-aa82-45b8-aa40-539fe1899ffb" />

### **FLAG:** scfWG6qNEIdzqVyfRwEGXyNUfFZkZeQ7

## [natas1](http://natas1.natas.labs.overthewire.org)
###### Usuário: natas1
###### Senha: scfWG6qNEIdzqVyfRwEGXyNUfFZkZeQ7

Agora nesse desafio, não podemos inspecionar a página usando o botão direito do mouse

<img width="609" height="153" alt="image" src="https://github.com/user-attachments/assets/32f392ed-f508-4593-8bfc-62ed4eddec78" />

Então usaremos o atalho **Ctrl + U** para visualizar o código fonte da página. E lá já temos a flag

<img width="1070" height="331" alt="image" src="https://github.com/user-attachments/assets/858a1448-d982-4339-8deb-1e205c6899e9" />

### **FLAG:** vsDOxoXyq3wckCP1ZmTZ71ngIA606odB

## [natas2](http://natas2.natas.labs.overthewire.org)
###### Usuário: natas2
###### Senha: vsDOxoXyq3wckCP1ZmTZ71ngIA606odB

Analisando o código fonte da página, encontro a imagem "pixel.png" nela

<img width="1073" height="272" alt="image" src="https://github.com/user-attachments/assets/3e15e53c-0fdc-4c1d-ae40-f6cf15053f4f" />

Clicando no link da imagem, sou direcionado para a imagem, contendo somente a imagem de 1 pixel branco

<img width="76" height="76" alt="Captura de tela 2026-07-01 162744" src="https://github.com/user-attachments/assets/456b1c10-ae5a-46d3-be7d-e24a00b09c1c" />

Mas oque importa é a sua URL, pois nele mostra que essa imagem está armazenada no diretório "/files"

<img width="509" height="36" alt="image" src="https://github.com/user-attachments/assets/50aecd2a-dcca-4a0c-ab0a-67af44a02a6f" />

Então, acessando o diretório "/files", encontro dois arquivos: o primeiro sendo a imagem e o segundo sendo uma lista de usuários

<img width="352" height="33" alt="image" src="https://github.com/user-attachments/assets/9194516d-2efb-44e1-b2a7-d74cd6dae3b2" />

<img width="659" height="291" alt="image" src="https://github.com/user-attachments/assets/5923ad33-827b-4179-b480-9921dba3317d" />

Clicando para ver a lista de usuário, nele contém o nome e a senha de login dos usuários, dentre a senha do natas3 é a nossa flag

<img width="304" height="114" alt="image" src="https://github.com/user-attachments/assets/9fc6a198-0f11-4fe8-adc1-ba8868e40f8c" />

### **FLAG:** K30JrSRHzjxq3paUQuwozY4MNvmNFyhI

## [natas3](http://natas3.natas.labs.overthewire.org)
###### Usuário: natas3
###### Senha: K30JrSRHzjxq3paUQuwozY4MNvmNFyhI

Inspecionando a página recebo a seguinte dica:
<img width="1081" height="276" alt="image" src="https://github.com/user-attachments/assets/06fca5a1-0f68-4e3d-9ab2-5fc32195798d" />

Ela quer dizer que nem mesmo o google poderá achar, ou seja, a página que procuramos não está indexada. Nos levando a pasta [robots.txt](https://www.cloudflare.com/pt-br/learning/bots/what-is-robots-txt/)

<img width="650" height="126" alt="image" src="https://github.com/user-attachments/assets/71014ea0-67e1-4b26-800d-cc52e9051f4d" />

Nessa página se encontra um diretório não indexado chamado "/s3cr3t/".

Acessando esse caminho nos deparamos com outro diretório com arquivos, só que agora contento apenas o "users.txt"

<img width="645" height="323" alt="Captura de tela 2026-07-01 165038" src="https://github.com/user-attachments/assets/4a59ed77-411b-4f6d-9531-57d13c6251a8" />

Acessando esse arquivos conseguimos a flag

<img width="687" height="119" alt="image" src="https://github.com/user-attachments/assets/4c5e558a-e2a1-4b43-8af5-d1fb820de25a" />

### **FLAG:** JDrPnuZAKyl6MkiqQGFIddrqpvgOASth

## [natas4](http://natas4.natas.labs.overthewire.org)
###### Usuário: natas4
###### Senha: JDrPnuZAKyl6MkiqQGFIddrqpvgOASth

Ao entrar no desafio nos deparamos com essa exigência: "Os usuários só terão acesso à flag se vierem do natas5"

<img width="586" height="158" alt="image" src="https://github.com/user-attachments/assets/6a1dc169-3157-4113-8f2f-8ea392489b8a" />

Então para contornar esse problema, usaremos o **Burp Suite**.

Nós vamos interceptar a [requisição HTTP](https://www.hostinger.com/br/tutoriais/servidor-proxy) do desafio para alterar a origem da requisição, fazendo o
**Referer** mudar de "natas4" para "natas5"

Então vamos lá: Primeiro vamos abrir o **Burp Suite**, ir em **Proxy** e abrir o browser. Depois logamos no natas4

<img width="964" height="479" alt="image" src="https://github.com/user-attachments/assets/ab06ea80-d6d0-47e4-bb01-0e2a23f15d86" />

Agora vamos ligar o "Intercept Off" e recarregar a página para interceptar a requisição HTTP. Logo depois mudaremos o **Referer** de "**natas4.natas.labs.overthewire.org**" para "**natas5.natas.labs.overthewire.org**"

<img width="1026" height="724" alt="image" src="https://github.com/user-attachments/assets/c271635d-43c2-4feb-b995-0a074c5229bc" />

E para enviar a requisição para o servidor clicamos em "Foward", agora retornando ao desafio conseguiremos a flag

<img width="962" height="449" alt="image" src="https://github.com/user-attachments/assets/e4a4a2f0-4276-4814-a734-156021396ade" />

### **FLAG:** e4z2Noy3oqwPJUWzJH0dseN67Cn1sy2M

## [natas5](http://natas5.natas.labs.overthewire.org)
###### Usuário: natas5
###### Senha: e4z2Noy3oqwPJUWzJH0dseN67Cn1sy2M

Ao entrar no desafio temos essa mensagem: "Acesso negado. Você não está logado". Isso dá uma dica sobre os [cookies](https://www.kaspersky.com.br/resource-center/definitions/cookies) do site

<img width="475" height="105" alt="image" src="https://github.com/user-attachments/assets/ff5bd92b-47fb-4b49-ad5b-271e982ee533" />

Ao acessar os cookies temos que o "loggedin" está com valor 0, oque em liguagem binária significa negação.

<img width="945" height="179" alt="image" src="https://github.com/user-attachments/assets/4a71fbac-34b5-408a-a9e6-338e617efeb8" />

Então mudaremos o valor de "loggedin" de 0 para 1, onde o 1 significa positivo. Fazendo isso já ganhamos a flag

<img width="1027" height="473" alt="image" src="https://github.com/user-attachments/assets/5143ef0d-d0be-4115-8c84-2f92db53b8d7" />

### **FLAG:** 7mhjtShJAcld2NYbKHEadnhEwRn2P8VT

## [natas6](http://natas6.natas.labs.overthewire.org)
###### Usuário: natas6
###### Senha: 7mhjtShJAcld2NYbKHEadnhEwRn2P8VT

Ao entrar no desafio já é nos disponibilizado visualizar o código da página. Onde teremos que achar uma chave para receber a flag

<img width="556" height="145" alt="image" src="https://github.com/user-attachments/assets/01b9ef9c-b28c-40b8-af90-ec165a0889f3" />

Acessando "sourcecode" do desafio, temos um código php nele

<img width="519" height="191" alt="image" src="https://github.com/user-attachments/assets/6f2ad455-58b9-41ce-92d7-1a444e881758" />

Onde a função "include" chama um outro arquivo presente no servidor para o código da página.

Acessando o arquivo chamado, nós conseguimos a chave para o desafio

<img width="776" height="144" alt="image" src="https://github.com/user-attachments/assets/802ef4cc-5cf9-43cd-b17a-d917a7a19e14" />

Ao inserir a chave **FOEIUWGHFEEUHOFUOIU** no input, conseguimos a flag

<img width="584" height="203" alt="image" src="https://github.com/user-attachments/assets/4e300005-6684-4ec8-829a-7f13bc0b3f49" />

**FLAG:** B1szg95UcTnrzwnF3i3TzYHlyYh8iBV0

## [natas7](http://natas7.natas.labs.overthewire.org)
###### Usuário: natas7
###### Senha: B1szg95UcTnrzwnF3i3TzYHlyYh8iBV0 

Na página inicial há apenas dois botões, *home* e *about*. Ao acessá-los, noto um padrão na URL: ela sempre utiliza o formato

"index.php?page=nome_da_pagina"

<img width="682" height="280" alt="Captura de tela 2025-12-01 205528" src="https://github.com/user-attachments/assets/94ad4c1b-ef1c-4527-ad4e-ec2a17d589a2" />

<img width="654" height="261" alt="Captura de tela 2025-12-01 205540" src="https://github.com/user-attachments/assets/5e532944-ed3a-4269-b50d-c61144d346da" />

Inspecionando o código-fonte da página, encontro uma dica indicando que a *flag* está localizada em /etc/natas_webpass/natas8

<img width="912" height="730" alt="Captura de tela 2025-12-01 205558" src="https://github.com/user-attachments/assets/13767eb7-29b0-49a4-9639-f74ec10c8b5b" />

Como a URL carrega páginas com base no parâmetro informado, substituo o nome da página pelo caminho da *flag* diretamente na URL

<img width="1304" height="376" alt="Captura de tela 2025-12-01 205436" src="https://github.com/user-attachments/assets/eb8f0721-4420-444b-99a1-1d2a3b832f04" />

E aqui já está a *flag*

**FLAG:** ugXL95KQmUAJJj6bMezOlBNDyI9Imwkc 

## [natas8](http://natas8.natas.labs.overthewire.org)
###### Usuário: natas8
###### Senha: ugXL95KQmUAJJj6bMezOlBNDyI9Imwkc 

Assim que acesso a página inicial, já tenho a opção de visualizar o código por trás do desafio

<img width="1125" height="350" alt="Captura de tela 2025-12-01 212128" src="https://github.com/user-attachments/assets/3d541c7a-67d9-4345-ab7e-02a79ac85bce" />

Analisando um pouco, já identifico que a senha está exposta, porém criptografada

<img width="500" height="228" alt="Captura de tela 2025-12-01 212116" src="https://github.com/user-attachments/assets/c07fc9f5-911e-4f63-a9fd-b2f7ddacbea1" />

A forma como o código criptografa minha entrada está na função de return, aplicando, nesta ordem: Base64, strrev (que inverte a ordem dos caracteres) e, por fim, hexadecimal. Depois disso, o resultado é comparado com a chave $encodedSecret

Para obter a senha, faço o processo inverso. O primeiro passo é decodificar o hexadecimal: "3d3d516343746d4d6d6c315669563362"

<img width="1480" height="862" alt="image" src="https://github.com/user-attachments/assets/c4e32c3b-d6d5-46a5-b741-d4bb04be4b96" />

Percebo que estou no caminho certo, pois o valor obtido tem o formato típico de uma string em Base64, só que invertida: '==QcCtmMml1ViV3b'. Com isso, peço ao ChatGPT para reverter a ordem dos caracteres

<img width="1003" height="198" alt="image" src="https://github.com/user-attachments/assets/2f888094-b45f-4485-bd08-e136d25e9844" />

Invertido fica: "b3ViV1lmMmtCcQ=="

Depois disso, basta decodificar o resultado em Base64 para finalmente obter a senha

<img width="1345" height="869" alt="image" src="https://github.com/user-attachments/assets/cd812e9f-d512-42cf-ae4b-4dab24a9bcc0" />

A senha é: "oubWYf2kBq"

Então agora é só colocar essa senha na pagina inicial e ganhar a *flag*

<img width="626" height="205" alt="image" src="https://github.com/user-attachments/assets/d092a827-e620-4e77-929d-737be5413cf4" />

<img width="1297" height="385" alt="image" src="https://github.com/user-attachments/assets/18b2ea82-b4da-4004-96c4-2d2afbe287f5" />

**FLAG:** UdxmI27dTaXmnd1rxKQTfws6jihTdcQ9

## [natas9](http://natas9.natas.labs.overthewire.org)
###### Usuário: natas9
###### Senha: UdxmI27dTaXmnd1rxKQTfws6jihTdcQ9

A função da página inicial basicamente retorna uma palavra semelhante à string que você digita

<img width="1295" height="472" alt="image" src="https://github.com/user-attachments/assets/1d22d594-4a1d-44d2-a396-075d7546842d" />

O código responsável pela busca das palavras já é disponibilizado na página, então começo analisando seu funcionamento

<img width="370" height="220" alt="Captura de tela 2025-12-02 140246" src="https://github.com/user-attachments/assets/cd8ef165-5148-4107-bb38-c6cfa60d8a85" />

Percebo que não existe qualquer filtro na entrada, e ela ainda é inserida diretamente no comando de shell da página — uma vulnerabilidade evidente de **command injection**

Para confirmar a vulnerabilidade, começo testando com um ";", permitindo inserir meu próprio comando logo após o comando original da página

<img width="627" height="276" alt="Captura de tela 2025-12-02 140331" src="https://github.com/user-attachments/assets/b2619c70-1382-43ad-9f3e-19fbc3502b01" />

É retornado a lista de itens disponiveis

Então, utilizo uma informação que o OverTheWire fornece logo no começo do CTF

<img width="1540" height="64" alt="image" src="https://github.com/user-attachments/assets/063e4c47-9d30-4657-9af3-c38d23312e87" />

Uso essa dica e procuro pelo arquivo oculto: ;ls /etc/natas_webpass/natas10

<img width="624" height="287" alt="image" src="https://github.com/user-attachments/assets/f43691fe-be64-4993-8616-57abc8ccb8d3" />

Como o diretório realmente existe, sigo para ler seu conteúdo utilizando o comando **cat**: ;cat /etc/natas_webpass/natas10

<img width="505" height="226" alt="Captura de tela 2025-12-02 145140" src="https://github.com/user-attachments/assets/d37e0476-305a-48e6-881a-a3e770378cba" />

E aí já está a *flag*

**FLAG:** EgjlkzB6E8LJyf2Obt4q7q4ewt5ZWSNv

## [natas10](http://natas10.natas.labs.overthewire.org)
###### Usuário: natas10
###### Senha: EgjlkzB6E8LJyf2Obt4q7q4ewt5ZWSNv

O desafio 10 é basicamente o natas9, mas com filtro no *input*. 

<img width="1385" height="503" alt="Captura de tela 2025-12-02 151046" src="https://github.com/user-attachments/assets/8a0de51a-db22-47c3-87fd-87a3846a393a" />

Como o comando não funcionou, volto ao código da função de busca para entender o que está acontecendo

<img width="446" height="289" alt="Captura de tela 2025-12-02 151120" src="https://github.com/user-attachments/assets/ca8b8ce5-e346-46ea-bacf-b3e56d7e15b0" />

O filtro bloqueia apenas os caracteres /[;|&]/, mas o $ não está entre eles. Isso significa que ainda é possível injetar comandos usando o operador $

<img width="610" height="563" alt="Captura de tela 2025-12-02 151149" src="https://github.com/user-attachments/assets/64c36d7a-191f-43fe-ac52-11e4a0f4f6ff" />

Com isso funcionando, utilizo o operador $ para acessar o arquivo da flag e lê-lo diretamente:

$ cat /etc/natas_webpass/natas11

<img width="612" height="274" alt="Captura de tela 2025-12-02 151210" src="https://github.com/user-attachments/assets/6c4101b5-f409-4052-b2cf-224a2b383b71" />

E aqui já está a *flag*

**FLAG:** VUMQDmuITOEHzhviLE5V0VG9cPMQkyxd

## [natas11](http://natas11.natas.labs.overthewire.org)
###### Usuário: natas11
###### Senha: VUMQDmuITOEHzhviLE5V0VG9cPMQkyxd

Após acessar a página já tenho algumas informções, os cookies do site estão encriptados em XOR
e o input faz o fundo mudar de cor de acordo com o código

<img width="1010" height="353" alt="Captura de tela 2025-12-02 220852" src="https://github.com/user-attachments/assets/5cf0c312-ff36-4400-868c-3547f446252b" />

Apesar disso, vou direto olhar o código do sistema

<img width="797" height="753" alt="Captura de tela 2025-12-02 220941" src="https://github.com/user-attachments/assets/7faa8022-870f-47ca-9c6e-1282c7fda495" />

Analisando ele percebo que o objetivo é fazer o "showpassword" ter o valor "yes" para conseguir a *flag*

Usando a dica do cookie, já vou direto nele

<img width="1220" height="513" alt="Captura de tela 2025-12-02 222230" src="https://github.com/user-attachments/assets/c3c07acd-743c-47bf-8a02-cb4290c5d17c" />

O cookie vale: "HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D", mas no final tem os caracters %3D, oque normalmente significa "=" na base64.

Corrigindo fica: "HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg="

Após decodificar parcialmente a string base64 para bytes: 1e 66 24 07 0a 33 27 0e 16

*Infelizmente não consegui concluir o desafio a tempo de entrega, mas continuarei estudando o natas até a última fase para melhorar meu conhecimento acerca 
do web hacking, já que agora não tenho mais provas pra me preocupar*


## Conclusão

O desafio Natas é um dos melhores jeitos de estudar segurança ofensiva, permitindo aprender coisas novas e mais complexas a cada fase que se avança, sempre apresentando uma nova vulnerabilidade para ser explorada. Contudo, é extremamente necessário estudar cybersec além do desafio, pois assim é possível entender o que cada parte do CTF significa e saber como usar as vulnerabilidades a seu favor.
