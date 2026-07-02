# Writeup — OverTheWire Natas11

**Desafio:** Natas11  
**Categoria:** Web Security / Criptografia  
**Objetivo:** Manipular um cookie protegido por XOR para exibir a senha do natas12

---

## Descrição do Desafio

O natas11 apresenta uma página simples que permite alterar a cor de fundo via parâmetro `bgcolor`. Os dados do usuário são armazenados em um cookie protegido por **XOR encryption + base64**. O código-fonte está disponível na própria página.

---

## Análise do Código-Fonte

```php
$defaultdata = array("showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';
    for($i=0;$i<strlen($text);$i++) {
        $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }
    return $outText;
}

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}
```

O fluxo de criação do cookie é:

```
array → json_encode → XOR encrypt → base64_encode → cookie
```

E para leitura:

```
cookie → base64_decode → XOR decrypt → json_decode → array
```

A senha só é exibida se `showpassword == "yes"`, portanto precisamos **forjar um cookie** com esse valor.

---

## Vulnerabilidade

O XOR possui uma propriedade matemática fundamental:

```
plaintext XOR chave = cifrado
cifrado   XOR plaintext = chave   ← vulnerabilidade
```

Como o **plaintext é conhecido** (o `$defaultdata` está público no código) e o **cookie cifrado está visível no navegador**, podemos recuperar a chave.

---

## Exploit — Passo a Passo

### Passo 1 — Capturar o cookie

Acessei o natas11 no navegador e copiei o valor do cookie `data` via DevTools:

```
EGAgHwQ1IxYYMSQYGSZxTUksPFVHYDEQCC0%2FGBlgaVVIJDURDSQ1VRY=
```

---

### Passo 2 — Descobrir a chave XOR

Com o cookie em mãos, fiz XOR do cookie decodificado com o plaintext conhecido para extrair a chave:

```python
import base64
import json
import urllib.parse

cookie = "EGAgHwQ1IxYYMSQYGSZxTUksPFVHYDEQCC0%2FGBlgaVVIJDURDSQ1VRY="
defaultdata = {"showpassword": "no", "bgcolor": "#ffffff"}

cookie_decoded = base64.b64decode(urllib.parse.unquote(cookie))
string_xor = json.dumps(defaultdata, separators=(',', ':'))

chave = ""
for i in range(len(cookie_decoded)):
    chave += chr(cookie_decoded[i] ^ ord(string_xor[i % len(string_xor)]))

print("Chave:", chave)
```

**Resultado:** A chave se repete em loop, revelando o padrão: `kBSw`

---

### Passo 3 — Forjar o cookie malicioso

Com a chave descoberta, criei um novo cookie com `showpassword: yes`:

```python
import base64
import json

hack = {"showpassword": "yes", "bgcolor": "#ffffff"}
chave = "kBSw"

codificar = json.dumps(hack, separators=(',', ':'))

string = b""
for i in range(len(codificar)):
    string += bytes([ord(codificar[i]) ^ ord(chave[i % len(chave)])])

resultado = base64.b64encode(string)
print(resultado.decode())
```

**Resultado:** um novo valor de cookie base64 com `showpassword = yes`.

---

### Passo 4 — Substituir o cookie no navegador

1. Abri o DevTools (F12) → aba **Application** → **Cookies**
2. Substituí o valor do cookie `data` pelo novo valor gerado
3. Recarreguei a página

## **FLAG:** EAGkE8uzFTxeoTT2mMst9Xy7PX6guEng

---

## Conceitos Explorados

| Conceito | Descrição |
|---|---|
| XOR com chave fixa | Permite recuperar a chave se o plaintext for conhecido |
| Plaintext conhecido | O `$defaultdata` está exposto no código-fonte público |
| Cookie forjado | Com a chave em mãos, qualquer payload pode ser cifrado e aceito |

---
```php
<?php
$biscoito = "EGAgHwQ1IxYYMSQYGSZxTUksPFVHYDEQCC0/GBlgaVVIJDURDSQ1VRY=";

function xor_encrypt($in) {
    $key = 'kBSw';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

$chave = xor_encrypt(base64_decode($biscoito));

echo "Chave:" , $chave;

$visual = array( "showpassword"=>"yes", "bgcolor"=>"#ffffff");

$hackeado = base64_encode(xor_encrypt(json_encode($visual)));

echo "Cookie: ",$hackeado;
?>
```

## Conclusão

A vulnerabilidade do natas11 está no uso de **XOR com chave estática** para proteger dados sensíveis em cookies. Como o plaintext padrão é público, a chave pode ser trivialmente recuperada e qualquer dado pode ser forjado.

**Mitigação correta:** assinar cookies com HMAC ou usar criptografia autenticada (ex: AES-GCM), nunca XOR simples com chave fixa.
