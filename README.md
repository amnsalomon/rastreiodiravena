# Rastreio diRavena

Página estática de rastreamento de pedidos da loja diRavena. Um único arquivo `index.html`, sem build e sem dependências além da fonte Manrope (Google Fonts).

## Como publicar no GitHub Pages

1. Crie um repositório (ex.: `rastreio-diravena`) e envie o `index.html` para a raiz da branch `main`.
2. No repositório: **Settings → Pages → Source: Deploy from a branch**, branch `main`, pasta `/ (root)`.
3. A página fica em `https://SEU-USUARIO.github.io/rastreio-diravena/`.
4. Domínio próprio (ex.: `rastreio.diravena.com.br`): crie um arquivo `CNAME` na raiz com o domínio dentro, e no DNS aponte um registro CNAME para `SEU-USUARIO.github.io`. Depois marque **Enforce HTTPS** em Settings → Pages.
5. Na Shopify, troque o link "Rastrear Pedido" do menu para esse endereço.

## Como a busca funciona

O campo é único e a transportadora é identificada pelo formato do que a cliente digita:

| O que foi digitado | Destino |
|---|---|
| `AA123456789BR` (2 letras + 9 números + 2 letras) | Correios — abre o rastreamento e copia o código para a área de transferência |
| 11 dígitos com dígito verificador válido | Jadlog — POST no campo `cte` para `tracking.jad` |
| 6 a 20 dígitos | Jadlog — tratado como código/CTE |

O POST para a Jadlog é feito por um formulário oculto com `target="_blank"`, então a cliente não perde a página de rastreio. Os Correios não aceitam código por URL no site oficial, por isso a página copia o código e abre o site; há também um botão secundário com atalho que já preenche.

## Link direto

Dá para mandar o link já preenchido por e-mail ou WhatsApp:

- `...?codigo=AA123456789BR` — preenche o campo
- `...?codigo=AA123456789BR&auto=1` — preenche e busca sozinho
- `...?cpf=12345678909` — também funciona

## O que ajustar

No topo do `<script>`, no objeto `CONFIG`, ficam as URLs das transportadoras, o número do WhatsApp e quantas buscas recentes guardar. Cores e tipografia estão nas variáveis CSS em `:root` (`--gold: #ebb114` é o dourado da loja).

As buscas recentes ficam só no navegador da cliente (`localStorage`), nada é enviado para nenhum servidor além do próprio site da transportadora.
