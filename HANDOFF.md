# Handoff — Tela AWS WAF Challenge (Pré-checkout)

> **Ticket:** EDO-289
> **URL servida:** `chk.eduzz.com`
> **Mockup live:** https://checkout-challenge-aws-waf.vercel.app
> **Repo:** https://github.com/eduzz-design/checkout-challenge-aws-waf

---

## Visão geral

Tela de verificação humana acionada pelo **AWS WAF Challenge** quando comportamento suspeito é detectado no checkout. O redesign substitui o widget WAF cru por uma camada Eduzz: marca, copy clara, transições suaves e estados explícitos — só caindo no captcha cru quando necessário.

### Fluxo

```
[1. Verificando] ──2.5s──▶ [2. Pronto p/ validar]
                              │
                              ▼  (clique "Sou humano")
                           [3. Captcha AWS WAF]
                              │
                              ▼  (slider arrastado até o fim)
                           [4. Sucesso] ──redirect──▶ checkout
```

Branch alternativa em qualquer ponto: **[Erro]** com CTA "Tentar novamente" → volta para `1. Verificando`.

---

## Especificações de layout

### Container principal (`.frame`)

| Propriedade | Valor |
| --- | --- |
| `max-width` | 420px |
| `min-height` | `calc(100vh - 96px - 24px - 41px)` |
| `border-radius` | `0 0 16px 16px` (cantos inferiores) |
| `background` | `var(--surface)` (#FFFFFF) |
| `box-shadow` | `0 1px 3px rgba(15,19,36,0.04), 0 8px 24px rgba(15,19,36,0.06)` |

### Top bar (`.top-bar`)

- `padding`: 20px 24px
- `border-bottom`: 1px solid `var(--border)`
- Layout: logo à esquerda, badge "Conexão segura" à direita (`space-between`)
- Logo: símbolo amarelo (24px) + wordmark navy (16px), gap 6px

### Conteúdo (`.content`)

- `padding`: 32px 24px
- Flex column, centralizado horizontalmente e em texto
- `flex: 1` (ocupa o restante do frame)

### Ícone central (`.icon-wrap`)

- 96 × 96px, círculo (`border-radius: 50%`)
- SVG interno 44 × 44px
- `margin-bottom`: 24px
- Background varia por estado (ver Cores)
- Spinner externo (`.ring`): `inset: -6px`, borda 3px (top + right na cor brand)

### Footer (`.frame-footer`)

- `padding`: 16px 24px
- `border-top`: 1px solid `var(--border)`
- `space-between`: "Protegido por Eduzz" + select de idioma

### Captcha overlay (`.captcha-overlay`)

- `position: absolute; inset: 0`
- Background: `rgba(15, 19, 36, 0.55)`
- Modal interno: 320px max-width, branco, radius 4px, box-shadow `0 10px 40px rgba(0,0,0,0.25)`
- O frame atrás recebe `filter: blur(1px)` quando o overlay está ativo

### Responsividade

Mockup desenhado para **mobile-first** (target principal: viewport mobile dentro do iframe `chk.eduzz.com`). O frame de 420px funciona como largura máxima — em telas menores ocupa 100%. Não há breakpoints adicionais nesta tela.

---

## Tipografia

**Stack:** `'Google Sans', 'Inter', -apple-system, BlinkMacSystemFont, sans-serif`

| Elemento | Tamanho | Peso | Line-height | Letter-spacing |
| --- | --- | --- | --- | --- |
| `h1` (título de estado) | 22px | 600 | default | -0.02em |
| `.lead` (descrição) | 14px | 400 | 1.5 | — |
| Texto de progresso | 12px | 400 | — | — |
| `.btn` (CTA) | 15px | 600 | — | — |
| `.btn-ghost` (secundário) | 13px | 500 | — | — |
| Badge "Conexão segura" | 11px | 400 | — | — |
| Footer trust / lang | 11px | 400 | — | — |
| Captcha modal title | 13px | 500 | — | — |

---

## Cores

### Tokens (mirror do Rootzz)

```css
--brand:        #2B4ACF;  /* primária */
--brand-dark:   #1F37A8;  /* hover */
--brand-soft:   #EAEEFB;  /* bg ícone verifying/manual */

--text:         #0F1324;
--text-muted:   rgba(15, 19, 36, 0.75);
--text-soft:    rgba(15, 19, 36, 0.55);

--bg:           #F5F6FA;  /* fundo da página */
--surface:      #FFFFFF;  /* fundo do card */

--border:       #C7C7C7;
--border-soft:  #E0E0E0;

--success:      #10B981;
--success-soft: #E6F8F2;
--danger:       #E11D48;
--danger-soft:  #FCE7EE;
--warning:      #F59E0B;
```

### Marca

- Símbolo da logo: **#FFBC00** (amarelo Eduzz)
- Wordmark: **#0D2772** (navy Eduzz)

### Mapeamento por estado

| Estado | `.icon-wrap` bg | Ícone | Acentos |
| --- | --- | --- | --- |
| Verificando | `--brand-soft` | `--brand` | progress bar `--brand` |
| Pronto p/ validar | `--brand-soft` | `--brand` | botão `--brand` |
| Sucesso | `--success-soft` | `--success` | — |
| Erro | `--danger-soft` | `--danger` | error-detail bg `--danger-soft` |

### Estados interativos

| Botão | Default | Hover |
| --- | --- | --- |
| `.btn-primary` | bg `--brand`, color white | bg `--brand-dark` |
| `.btn-ghost` | color `--text-muted` | color `--text` |
| `.lang-select` | transparent | bg `--bg` |

---

## Componentes

### 1. `TopBar`
Logo Eduzz (símbolo + wordmark) + badge "Conexão segura". Não interativo.

### 2. `IconWrap`
Círculo de 96px com ícone SVG centralizado.
**Variantes:** `verifying` | `success` | `error` | `manual`
**Modificador:** com/sem `.ring` (spinner externo)

### 3. `ProgressBar`
- Track: 4px de altura, `--border`, radius 2px
- Bar: `--brand`
- Modos: `indeterminate` (animado, 40% width deslizando) ou explícito (width fixa)

### 4. `Button`
**Variantes:** `btn-primary` | `btn-ghost`
- max-width 320px, padding 14×20, radius `--radius` (8px)
- Suporta ícone SVG à esquerda (gap 8px)

### 5. `ErrorDetail`
Caixa de explicação no estado de erro: bg `--danger-soft`, border `#F5BCCD`, padding 12×14, texto `#9F1239`.

### 6. `FrameFooter`
Linha de confiança ("Protegido por Eduzz") + select de idioma (`pt-BR`, `en`, `es`).

### 7. `CaptchaOverlay` (AWS WAF widget)
Modal renderizado pela AWS WAF — visual **fixo** pela AWS, com customização muito limitada. Estrutura:
- Cabeçalho: badge `AWS` + texto `WAF` + ícone info
- Body: prompt + área do puzzle (gradient + hole + piece) + slider
- Foot: "Powered by AWS WAF" + links Privacy/Terms

### 8. `Slider` (dentro do CaptchaOverlay)
- Track 36px de altura, `#F3F3F3`, border `#DDD`
- Handle 28 × 28px, `#FFF`, com chevron `›`
- Fill que cresce conforme o drag (`#D6E4F5` em progresso, `#C6EFCE` quando resolvido)
- Estado resolvido: handle vira verde com `✓`, label troca para "Verificado"

### 9. `DemoBar` (fora da UI)
Barra fixa no rodapé, fora da UI real. Controla estado da tela para apresentações:
- Botões de estado: `1. Verificando` / `2. Pronto p/ validar` / `3. Captcha AWS` / `4. Sucesso` / `Erro`
- Toggle: "Auto avançar para o botão após 2.5s"

> Padrão Eduzz: controles de demo **sempre** em barra fixa no rodapé, fora da UI, em todo mockup novo.

---

## Estados detalhados

### 1. Verificando (default)

- **Visual:** ícone escudo brand-soft + ring girando + h1 "Verificando seu acesso"
- **Copy lead:** "Estamos confirmando que você é uma pessoa real. Isso ajuda a proteger sua compra contra fraudes."
- **Progress:** barra indeterminada animada
- **Texto auxiliar:** "Leva só alguns segundos…"
- **Comportamento:** auto-avança para `Pronto p/ validar` em 2.5s (toggle on por padrão)

### 2. Pronto p/ validar

- **Visual:** mesmo escudo, sem ring (loading concluído)
- **h1:** "Confirme que é você"
- **Copy lead:** "Toque no botão abaixo para concluir a verificação e seguir com sua compra."
- **CTA:** botão primary "Sou humano" com ícone check → abre captcha

### 3. Captcha AWS WAF

- **Visual de fundo:** card "Verificação adicional" com escudo + copy "Complete o desafio na janela ao lado para confirmar seu acesso. Não feche esta página."
- **Frame atrás recebe blur(1px)** e bloqueia pointer-events
- **Modal AWS WAF** aparece centralizado (overlay escuro)
- **Slider interativo:** ver `Slider` em Componentes
- **Sucesso do slider:** após 600ms de pausa visual, fecha modal e vai para `Sucesso`

### 4. Sucesso

- **Visual:** ícone check verde com animação `pulse-soft` (0.6s)
- **h1:** "Tudo certo!"
- **Copy lead:** "Verificação concluída. Estamos te levando de volta para finalizar sua compra."
- **Progress:** 100% (fixo)
- **Texto auxiliar:** "Redirecionando…"
- **Comportamento real:** disparar redirect para a URL de retorno do checkout

### Erro (branch alternativa)

- **Visual:** ícone X vermelho
- **h1:** "Não conseguimos verificar"
- **Copy lead:** "A verificação de segurança falhou. Vamos tentar novamente — sua compra está esperando você."
- **Detalhe:** caixa com motivos prováveis (VPN, bloqueador, conexão instável)
- **CTAs:** "Tentar novamente" (primary, volta para `Verificando`) + "Preciso de ajuda" (ghost, abre `eduzz.com/ajuda`)

---

## Animações

| Nome | Aplicado em | Duração | Easing | Loop |
| --- | --- | --- | --- | --- |
| `spin` | `.ring` (spinner) | 1s | linear | infinite |
| `indeterminate` | `.progress-bar.indeterminate` | 1.4s | ease-in-out | infinite |
| `pulse-soft` | `.icon-wrap.success svg` | 0.6s | ease-out | 1× |
| `fadeIn` | `.captcha-overlay` | 0.2s | ease | 1× |
| `popIn` | `.captcha-modal` | 0.25s | ease | 1× |
| Slider drop-back | handle + fill + piece | 0.25s | ease | 1× |

---

## Observações para desenvolvimento

### Integração com AWS WAF
- O **widget AWS WAF não pode ser estilizado** — visual fixo definido pela AWS. Nosso controle se limita a: posicionar o modal, escurecer o frame por trás, mostrar copy contextual ("Complete o desafio…").
- O slider implementado no mockup é **representativo** — em produção é o widget AWS injetando o iframe próprio.
- Eventos de sucesso/falha do WAF devem disparar transição para `Sucesso` ou `Erro` respectivamente.

### Fluxo automático vs manual
- **Verificando** roda passivamente — se o WAF resolver sozinho (ex.: token de cookie), pula direto para `Sucesso`.
- **Pronto p/ validar** só aparece se o WAF exigir interação humana (checkbox + slider).
- A copy do botão é "**Sou humano**" para reforçar a intenção, não "Continuar" ou "Validar".

### Acessibilidade
- Modal do captcha usa `role="dialog" aria-modal="true" aria-label="Desafio AWS WAF"`.
- Frame atrás recebe `pointer-events: none` quando overlay está ativo (impede tab navigation pra trás do modal).
- Label de idioma: `aria-label="Idioma"` no `<select>`.

### Edge cases
- **JS desabilitado / bloqueador de scripts:** WAF não roda — usuário fica preso. Necessário fallback server-side (mensagem estática + link para suporte).
- **Conexão muito lenta:** estado `Verificando` pode passar dos 10s. Após X segundos sem resposta, sugerir `Erro` com motivo "conexão instável".
- **Re-tentativa após erro:** volta para `Verificando` e re-inicia a request ao WAF.
- **VPN / Tor:** WAF tende a falhar com alta frequência. Copy do `Erro` já cita VPN como possível causa.

### Internacionalização
- Idiomas previstos: `pt-BR` (default), `en`, `es`.
- Todas as strings devem virar chaves de tradução. Strings atualmente hardcoded em `index.html` linhas 528–582 (estados) e 600–663 (captcha + footer).

### Telemetria sugerida
- `waf_challenge_shown` — quando a tela carrega
- `waf_ready_shown` — após loading inicial
- `waf_user_clicked_human` — clique no "Sou humano"
- `waf_captcha_solved` — slider concluído
- `waf_error_shown` — fallback de erro
- `waf_retry_clicked` — clique em tentar novamente

---

## Tokens de origem

A paleta `--brand-*` e tokens utilitários espelham o Design System Rootzz. Em produção, substituir as CSS vars deste mockup por imports do pacote `@eduzz/rootzz` (ou equivalente do projeto checkout).

- Tokens Rootzz: https://rootzz.eduzz-design.com.br/llms/foundations/ui/tokens.txt
- Cores DS Eduzz: https://eduzz-ds.vercel.app/colors

---

## Referências

- [AWS WAF Challenge — vídeo institucional](https://aws.amazon.com/pt/video/watch/56d30d108cd/)
- Código-fonte do mockup: [`index.html`](./index.html)
- Repositório: https://github.com/eduzz-design/checkout-challenge-aws-waf
