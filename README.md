# Guia de Admissão — Grupo MM

Página interna de onboarding para novos colaboradores, em formato "stories"
(uma tela por vez, com avanço por toque). Refatoração da versão original de
arquivo único, alinhada ao padrão do `agente.md`.

## Arquivos

- `admissao-grupo-mm.html` — estrutura (HTML semântico)
- `styles.css` — todos os estilos (sem CSS inline)
- `README.md` — este documento

Não há JavaScript: a navegação é feita apenas com CSS (`:target`), o que mantém
a página leve e simples de revisar. Se for publicada como raiz do site, o arquivo
pode ser renomeado para `index.html`.

## Objetivo e conversão

- **Objetivo:** orientar o colaborador recém-admitido a concluir as tarefas do
  primeiro dia.
- **Conversão** (não é envio de formulário): conclusão das ações do dia —
  assinar o contrato digital, registrar o ponto 4x/dia, baixar o app WAAPI HCM e
  acessar a UNIMM. O checklist (tela 9) resume essas ações.

## O que foi feito na refatoração

- **HTML semântico:** `main` + `section` por tela, um `h1` na capa e `h2` por
  seção. Substituídas as `div` que faziam papel de título/lista por elementos
  corretos (`h1/h2`, `ol`, `ul`).
- **Acessibilidade:**
  - Navegação por toque agora usa links **com `aria-label`** ("Avançar" /
    "Voltar") e **foco visível** (`:focus-visible`).
  - Zonas de toque movidas para as **bordas** (esquerda/direita), liberando a
    coluna central para leitura e rolagem — o conteúdo não fica mais bloqueado.
  - Slides passam a **rolar** (`overflow-y:auto`) quando o conteúdo excede a
    altura da tela (antes o excesso era cortado).
  - Emojis decorativos marcados com `aria-hidden="true"`; logo em SVG com
    `role="img"` e rótulo.
  - Respeito a `prefers-reduced-motion`.
- **CSS externo e componentizado:** removidos ~100 blocos de estilo inline;
  criadas classes reutilizáveis (callouts, cards, steps, checklist, etc.).
- **Metadados:** `meta description`, Open Graph, favicon inline, `theme-color`.
- **Segurança/higiene:** `Content-Security-Policy` via meta (`script-src 'none'`,
  `form-action 'none'`), `referrer` `no-referrer` e política de indexação
  `noindex, nofollow` (página interna, não deve aparecer em buscadores).
- **Performance:** pesos da fonte reduzidos para os realmente usados
  (400–900). A página não usa imagens (apenas emojis e SVG inline).

## Premissas adotadas

- Trata-se de **página interna de RH**, não de mídia paga — por isso `noindex`.
- As cores (vermelho `#bc0404`, amarelo `#f4c500`) e a fonte **Montserrat** são a
  identidade da marca e foram **preservadas**.
- Os textos e credenciais mostrados (ex.: `nome.sobrenome`, CPF de exemplo) são
  **placeholders ilustrativos**, não dados reais.

## Pendências / decisões do time

- **`og:image`:** não incluída — não havia ativo aprovado. Adicionar a URL de uma
  imagem oficial (1200×630) quando disponível. *Não foi inventada.*
- **Contato do RH / WhatsApp:** o texto menciona "fale com o RH", mas **nenhum
  link ou número foi inventado**. Para transformar em CTA clicável, informe o
  número oficial e eu adiciono um link `https://wa.me/55XXXXXXXXXXX` — lembrando
  que isso passa a ser um destino externo a validar.
- **Fonte Google Fonts:** carregada de `fonts.googleapis.com`, o que envia o IP
  do usuário ao Google (ponto de atenção de **LGPD** e de performance). Para
  eliminar o terceiro, recomenda-se **hospedar a Montserrat localmente**
  (`/fonts`) e ajustar o `font-src` do CSP para `'self'`.

## Cabeçalhos de segurança (configurar no servidor)

A tag `<meta http-equiv="Content-Security-Policy">` cobre o básico, mas alguns
cabeçalhos **só funcionam via HTTP** e devem ser definidos no servidor/CDN:

```
Content-Security-Policy: default-src 'self'; base-uri 'self'; img-src 'self' data:; style-src 'self' https://fonts.googleapis.com; font-src https://fonts.gstatic.com; script-src 'none'; form-action 'none'; object-src 'none'; frame-ancestors 'self'
X-Content-Type-Options: nosniff
Referrer-Policy: no-referrer
X-Frame-Options: SAMEORIGIN
```

Se a fonte for hospedada localmente, troque `style-src`/`font-src` por `'self'`.

## Riscos e limitações

- **LGPD:** a página, por si só, **não coleta dados pessoais** (sem formulário,
  cookies, analytics ou pixel). O único fluxo a terceiro é a fonte do Google
  (ver acima). Se no futuro forem adicionados formulário, rastreamento ou pixels,
  será necessário definir finalidade, base legal/consentimento, destinatários e
  vincular uma política de privacidade — nada disso foi adicionado por conta
  própria.
- **Sem testes em dispositivos reais:** a responsividade foi validada por
  inspeção de código (mobile-first, `clamp()`, rolagem de segurança). Recomenda-se
  um teste rápido em telas pequenas (≤360px de largura) e com leitor de tela.
- **Navegação por CSS `:target`:** simples e sem dependências, mas altera o hash
  da URL a cada passo (comportamento esperado para este formato).