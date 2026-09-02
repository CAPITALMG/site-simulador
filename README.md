# Simulador de Quitação Antecipada — CG Colombo Assessoria

Site estático de uma página: calcula quanto o cliente economiza quitando o financiamento
antecipadamente. Usado pelas consultoras no atendimento.

- **Produção:** https://simulador.cgcolombo.com.br (e https://simulador-cgcolombo.netlify.app)
- **Hospedagem:** Netlify, projeto `simulador-cgcolombo`, conta `cgcolomboassessoria@gmail.com`
- **Publicação:** automática — todo push na `main` publica.

## Metodologia do cálculo

Valor presente de cada parcela vincenda pela taxa do contrato:

    VP = parcela / (1 + i) ^ (dias / 30)

`dias` = dias corridos entre a data da quitação e o vencimento de cada parcela. A soma é feita
**sem arredondar** e só o total é arredondado. As parcelas contam a partir do primeiro vencimento
igual ou posterior à data da quitação.

Base legal: art. 52, § 2º do Código de Defesa do Consumidor; art. 7º da Resolução CMN n. 5.004/2022.

**Conferido contra a calculadora pública do MPSC**, parcela a parcela, em 31/08/2026:

| parcela | taxa a.m. | dia venc. | quitação | parcelas | total MPSC | nosso |
|---|---|---|---|---|---|---|
| R$ 500,00 | 1,80% | 10 | 10/09/2026 | 84 | R$ 21.785,16 | R$ 21.785,16 |
| R$ 1.000,00 | 2,15% | 05 | 25/09/2026 | 12 | R$ 10.611,95 | R$ 10.611,95 |

## O que a página faz (versão de 01/09/2026)

- Formulário à esquerda, resultado à direita (empilha no celular). Enter calcula; depois do primeiro
  cálculo, qualquer campo alterado recalcula sozinho.
- Resultado: economia em destaque, valor para quitar, total sem desconto, juros médios por parcela,
  barra "paga hoje × deixa de pagar".
- **Quanto custa esperar**: desembolso total se a quitação ficar para +1, +3, +6 e +12 meses
  (paga cheias as parcelas que vencem até lá e quita o resto na nova data).
- Parcela a parcela com coluna de juros descontados e linha de total.
- Ações: **Enviar no WhatsApp** (abre o app com o resumo pronto), copiar resumo, **copiar link**
  (a URL carrega a simulação preenchida: `#p=500&t=1.8&n=84&d=10&q=2026-09-10&c=Nome`), imprimir/PDF
  em A4 com cabeçalho, dados do contrato e rodapé.
- Nome do cliente (opcional) e nome da consultora (lembrado no aparelho) entram no resumo e na impressão.
- Histórico das 6 últimas simulações, só no navegador de quem usa (`localStorage`). Nada vai para servidor.
- Instalável na tela inicial do celular (`manifest.webmanifest` + ícones).

## Estrutura

- `index.html` — a página inteira (HTML, CSS e JS embutidos; sem dependência externa além da fonte)
- `logo.svg` / `logo-dark.svg` — logo do cabeçalho (tema claro / escuro). **Trocar a marca = trocar estes dois arquivos.**
- `icon.svg`, `icon-180.png`, `icon-192.png`, `icon-512.png`, `manifest.webmanifest` — ícone e instalação no celular
- `_headers` — cabeçalhos de segurança aplicados pelo Netlify
- `robots.txt`

## Se mexer no cálculo

A função `calcular()` é a única que importa. Revalide com os dois casos da tabela acima antes de
publicar (a calculadora do MPSC aceita POST em `intranet.mpsc.mp.br/scq/calculadora/seleciona`).
