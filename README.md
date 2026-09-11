# Rastreio diRavena

Página estática de rastreamento de pedidos da loja diRavena. Um único arquivo `index.html`, sem build e sem dependências além da fonte Manrope (Google Fonts). As consultas são feitas no **Melhor Rastreio**, que cobre Correios, Jadlog, Loggi, Azul Cargo, LATAM Cargo e Buslog em uma busca só.

## Antes de publicar: confirme o formato do link

O Melhor Rastreio é uma aplicação JavaScript, então o formato da URL de um rastreio precisa ser conferido na prática:

1. Abra `melhorrastreio.com.br` e pesquise um código real de um pedido já postado.
2. Copie a URL que aparece na barra de endereço.
3. Se ela não for `https://melhorrastreio.com.br/rastreio/CODIGO`, ajuste a linha `urlRastreio` no objeto `CONFIG`, dentro do `<script>`, mantendo o marcador `{codigo}` no lugar do código.

Se o link direto não existir, deixe `urlRastreio` apontando para a home (`https://melhorrastreio.com.br/`): a página continua funcionando, porque copia o código para a área de transferência e mostra o botão de copiar no painel de resultado.

## Como publicar no GitHub Pages

1. Envie o `index.html` para a raiz da branch `main`.
2. **Settings → Pages → Source: Deploy from a branch**, branch `main`, pasta `/ (root)`.
3. Domínio próprio: **Settings → Pages → Custom domain** com `rastrear.diravena.com.br` e, no Cloudflare, um CNAME `rastrear` apontando para `SEU-USUARIO.github.io` em modo DNS only.

## Como a busca funciona

Um campo só. O texto é normalizado (maiúsculas, sem espaços e pontuação) e classificado:

| O que foi digitado | O que acontece |
|---|---|
| `AA123456789BR` | Abre o rastreio e informa que é dos Correios |
| 11 a 14 dígitos | Abre o rastreio e informa que é da Jadlog |
| Outro código de 8 a 30 caracteres | Abre o rastreio sem nomear a transportadora |
| `AA123456789` (sem o BR final) | Avisa que faltam as duas letras finais |
| 11 dígitos que formam um CPF válido | Explica que a busca é pelo código e oferece pedir o código no WhatsApp |

O rastreio abre em nova aba, então a cliente não perde esta página nem os botões de ajuda.

## Fallback da Jadlog por CPF

A busca por CPF na Jadlog continua no código, desligada. Para reativá-la como opção secundária na tela de CPF, mude `fallbackJadlog` para `true` em `CONFIG`. O formulário oculto que faz o POST em `tracking.jad` está no fim do HTML.

## Link direto

- `...?codigo=AA123456789BR` — preenche o campo
- `...?codigo=AA123456789BR&auto=1` — preenche e busca sozinho

Serve para o e-mail de postagem: a cliente clica e cai direto no rastreio, passando pela sua marca no caminho.

## O que ajustar

`CONFIG`, no topo do `<script>`: URLs do Melhor Rastreio, WhatsApp, fallback da Jadlog e quantas buscas recentes guardar. Cores e tipografia nas variáveis CSS em `:root` (`--gold: #ebb114` é o dourado da loja).

As buscas recentes ficam só no navegador da cliente (`localStorage`).
