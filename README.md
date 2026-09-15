[README.md](https://github.com/user-attachments/files/32262633/README.md)
# StreamLog

Registro de leituras de sonda multiparamétrica em campo, direto no navegador, com checagem imediata de consistência e visualização interativa.

Funciona offline, em celular ou notebook, sem instalação e sem servidor.

**Acesse:** https://kauanfonseca.github.io/streamlog/

---

## Por que existe

Em campanhas de campo em riachos, erros de leitura (sonda descalibrada, dígito trocado na anotação, eletrodo sujo) costumam ser descobertos só semanas depois, na digitação da planilha — quando voltar ao ponto já não é viável. O StreamLog sinaliza o valor suspeito no momento da digitação, ainda com a equipe no riacho, permitindo repetir a leitura na hora.

Resultado esperado: menos retrabalho, menos deslocamento repetido e dados mais confiáveis ao fim da campanha.

## O que faz

- **Registro por ponto** — riacho, ponto, data/hora e as variáveis da sonda.
- **Checagem imediata** — valor fora da faixa esperada fica destacado no campo, na tabela e num aviso agregado no topo.
- **Gráfico interativo** — variável no eixo Y, hora ou ponto no eixo X, em linha, dispersão ou boxplot por riacho, com a faixa esperada sombreada ao fundo.
- **Exportação em CSV** — codificado em UTF-8 com BOM, abre direto no Excel e no R.
- **Armazenamento local** — as leituras ficam salvas no navegador do próprio aparelho; fechar a aba não apaga nada.

## Variáveis registradas

| Variável | Unidade | Faixa esperada (padrão) |
|---|---|---|
| Temperatura | °C | 14 – 34 |
| pH | — | 6,0 – 9,2 |
| Oxigênio dissolvido | mg/L | 2 – 14 |
| Condutividade | µS/cm | 20 – 900 |
| Turbidez | NTU | 0 – 150 |
| ORP | mV | −200 – 600 |

As faixas não são limites universais: foram calibradas para riachos cársticos da Bacia do Alto Paraguai (ecótono Cerrado–Pantanal). **Ajuste antes de usar em outra bacia** — ver abaixo.

## Como usar em campo

### Antes de sair (ainda com internet)

O primeiro acesso exige conexão, porque a biblioteca do gráfico é carregada de um servidor externo nessa primeira vez. Depois disso a página funciona sem sinal — mas quem abrir o link pela primeira vez já dentro do riacho não vai conseguir usar.

1. Abra o link com internet, em cada aparelho que for para o campo.
2. Recomendado: instale na tela inicial. No menu do navegador, escolha **Adicionar à tela de início** (Android) ou **Adicionar à Tela de Início** (iPhone, no menu de compartilhar). O app passa a abrir em tela cheia, sem depender de achar o link no histórico.
3. Toque em **Dados de exemplo** para checar que a tabela e o gráfico aparecem — se aparecerem, o aparelho está pronto. Depois use **Apagar tudo** antes da coleta real.

### No riacho

1. Preencha riacho, ponto e as leituras da sonda; toque em **Adicionar leitura**.
2. Se algum campo ficar vermelho, confira a sonda e repita a leitura antes de deixar o ponto.
3. Ao fim do dia, toque em **Exportar CSV** e envie o arquivo para si mesmo assim que houver sinal.

O conjunto de **Dados de exemplo** traz um erro plantado (condutividade fora da faixa), útil para treinar a equipe a reconhecer o alerta antes da campanha.

## Como adaptar a outra bacia ou a outra sonda

Todas as variáveis e faixas estão na constante `VARS`, no início do bloco `<script>` do `index.html`:

```js
const VARS = [
  {k:"temp", lab:"Temperatura", un:"°C", min:14, max:34, step:"0.1"},
  ...
];
```

Para mudar uma faixa, edite `min` e `max`. Para acrescentar uma variável, adicione uma linha seguindo o mesmo formato — a coluna na tabela, o campo no formulário e a opção no gráfico aparecem sozinhos. `k` é o nome da coluna no CSV e não deve ter acentos nem espaços.

## Análise posterior no R

```r
dados <- read.csv("sonda_2026-09-15.csv", encoding = "UTF-8")
dados$data <- as.POSIXct(dados$data, format = "%Y-%m-%dT%H:%M")
```

## Limitações

- Os dados ficam em cada aparelho separadamente: duas pessoas coletando geram dois CSVs, unidos depois na análise. Não há base compartilhada em tempo real.
- Limpar os dados de navegação do aparelho apaga as leituras. **Exporte o CSV ao fim de cada dia.**
- A checagem é de plausibilidade por faixa, não substitui a calibração da sonda nem a conferência do caderno de campo.

## Desenvolvimento

Arquivo único em HTML, sem dependências além do [Plotly.js](https://plotly.com/javascript/) via CDN. Para rodar localmente, basta abrir o `index.html` no navegador. Contribuições e adaptações são bem-vindas.

## Licença

MIT.

## Como citar

> Fonseca, K. N. StreamLog: interface para registro e checagem de dados de sonda multiparamétrica em campo. Disponível em: https://github.com/SEU-USUARIO/streamlog

---

Desenvolvido no âmbito do projeto *Biodiversidade que Constrói Paisagens: Rios que Criam Pedras e Peixes que Moldam Cachoeiras* (PCI/INPP), com apoio do Laboratório de Rios e Córregos da UERJ, em colaboração com a UFMT e o SESC Pantanal.
