# Checkout Challenge AWS WAF — Redesign

Mockup HTML standalone do redesign da tela de AWS WAF Challenge servida em `chk.eduzz.com`.

## Contexto

Quando o AWS WAF detecta comportamento suspeito no checkout, o usuário é redirecionado para uma tela de verificação humana. A versão atual quebra a jornada de compra, gera insegurança e aumenta abandono. Este mockup propõe uma versão alinhada à identidade Eduzz + Design System Rootzz.

## Estados desenhados

- **Verificando** (default, auto-start): escudo + spinner + barra indeterminada
- **Sucesso**: checkmark + redirecionando
- **Manual (fallback)**: checkbox "Sou uma pessoa real" + CTA
- **Erro**: motivo provável (VPN, bloqueador) + tentar novamente + ajuda

## Como visualizar

```sh
open index.html
```

Use a barra de demo no rodapé (fora da UI) para alternar entre estados e ativar o auto-avanço.

## Tokens aplicados

- Paleta de UI: Rootzz (`--ant-color-primary: #2B4ACF`, `--ant-color-success: #10B981`, `--ant-color-error: #E11D48`)
- Marca no logo: amarelo `#FFBC00` (símbolo) + azul marinho `#0D2772` (texto)
- Tipografia: Google Sans
- Border radius: 8px

## Referências

- [AWS WAF Challenge](https://aws.amazon.com/pt/video/watch/56d30d108cd/)
- [Eduzz Design System — Cores](https://eduzz-ds.vercel.app/colors)
- [Rootzz Tokens](https://rootzz.eduzz-design.com.br/llms/foundations/ui/tokens.txt)
