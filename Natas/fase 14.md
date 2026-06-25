# Writeup — OverTheWire Natas14

**Desafio:** Natas14  
**Categoria:** Web Security / SQL Injection  
**Objetivo:** Bypass de autenticação via SQL Injection para obter a senha do natas15

---

## Descrição do Desafio

O natas14 apresenta um formulário de login simples. O código-fonte está disponível na página e revela que as credenciais são verificadas diretamente via query SQL **sem nenhuma sanitização** dos inputs do usuário.

---

## Análise do Código-Fonte

```php
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";

if(mysqli_num_rows(mysqli_query($link, $query)) > 0) {
    echo "Successful login! The password for natas15 is <censored>";
} else {
    echo "Access denied!";
}
```

O input do usuário é concatenado diretamente na query SQL. Se `mysqli_num_rows()` retornar mais de 0 linhas, o login é considerado bem-sucedido.

A query base é:
```sql
SELECT * from users where username="[INPUT]" and password="[INPUT]"
```

---

## Exploit — Passo a Passo

### Tentativa 1 — Payload clássico (falhou)

A primeira tentativa foi o payload mais comum de SQL Injection:

**Username:**
```
" OR 1=1 -- 
```

**Resultado:**
```
Warning: mysqli_num_rows() expects parameter 1 to be mysqli_result, bool given
Access denied!
```

O erro `bool given` indica que a query retornou `false` — ou seja, a query estava com **sintaxe inválida**. O problema foi que o `--` para comentário não funcionou corretamente nesse contexto do MySQL, quebrando a query ao invés de comentar o restante.

---

### Tentativa 2 — Comentário com `#` (sucesso)

No MySQL, o caractere `#` também funciona como comentário de linha. Trocando o `--` por `#`:

**Username:**
```
" OR 1=1#
```

**Password:** (qualquer valor)

A query resultante ficou:
```sql
SELECT * from users where username="" OR 1=1#" and password="qualquer"
```

O `#` comentou todo o restante da query, e `OR 1=1` sempre retorna verdadeiro — fazendo a query retornar todas as linhas da tabela, bypassando o login.

---

### Resultado

Login bem-sucedido! A senha do natas15 foi exibida:

```
GB6USCJYJjwLyYhZUNkE1NwDueiTow6g
```

---

## Por que `--` falhou e `#` funcionou?

| Caractere | Comportamento no MySQL |
|---|---|
| `--` | Requer um **espaço após** (`-- `) para ser reconhecido como comentário |
| `#` | Comenta tudo até o fim da linha sem precisar de espaço |

No primeiro payload o espaço após `--` pode ter sido removido ou mal interpretado pelo PHP/MySQL, corrompendo a query. O `#` é mais robusto nesse contexto.

---

## Conceitos Explorados

| Conceito | Descrição |
|---|---|
| SQL Injection | Input não sanitizado é interpretado como código SQL |
| Authentication Bypass | `OR 1=1` força a query a sempre retornar linhas |
| Comentário MySQL (`#`) | Ignora o restante da query após o payload |
| Concatenação insegura | Usar `.$_REQUEST["input"].` diretamente na query é a raiz da vulnerabilidade |

---

## Conclusão

O natas14 demonstra um dos ataques mais clássicos da segurança web — SQL Injection para bypass de autenticação. A ausência completa de sanitização permite que qualquer usuário faça login sem credenciais válidas.

**Mitigações corretas:**
- Usar **Prepared Statements** com parâmetros vinculados (`mysqli_prepare` + `bind_param`)
- Nunca concatenar input do usuário diretamente em queries SQL
- Usar um ORM que abstraia e sanitize as queries automaticamente
