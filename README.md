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

## Estrutura

- `index.html` — a página inteira (HTML, CSS e JS embutidos; sem dependência externa além da fonte)
- `_headers` — cabeçalhos de segurança aplicados pelo Netlify
- `robots.txt`

## Como trocar a logo

No `index.html`, na constante `MARCA` (perto do fim do arquivo): `logoDataURI` recebe a imagem em
data URI; enquanto estiver vazia, a página mostra as iniciais.
