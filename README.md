# Dashboard de Vendas Porsche

Dashboard interativa em HTML, arquivo único, sobre 100 negociações de veículos Porsche. Responde a três perguntas de negócio, tem filtros cruzados e roda publicada no GitHub Pages.

Projeto do desafio de dashboard com IA da DIO.

**Dashboard no ar:** https://richard-456.github.io/dashboard-vendas-porsche/
**Código:** [`index.html`](./index.html) — um arquivo, sem build, com os dados embutidos.

---

## As três perguntas de negócio

Escolhi perguntas que um gerente de concessionária faria numa reunião de segunda-feira, não as que a base deixa mais fácil responder.

**1. Qual família de modelo sustenta a receita?**
Não é a mesma pergunta que "qual modelo vende mais". Os 40 modelos distintos da base pulverizam a análise — com três ou quatro vendas cada, nenhum diz nada. Agrupei em seis famílias (911, 718, Cayenne, Macan, Taycan, Panamera) e aí a conta aparece: o 911 responde por um terço da receita com menos de um quarto dos carros. Decide onde colocar estoque.

**2. Onde estão os compradores?**
Receita por estado, dez maiores. Decide onde abrir ponto e onde concentrar mídia. Preferi receita a volume porque dois Macan não equivalem a um 911 Turbo S.

**3. A forma de pagamento muda o ticket médio?**
Aqui a medida é média, não soma, e a diferença importa: se quem paga por transferência leva carro mais caro que quem financia, isso muda a conversa com o cliente ainda no salão. Soma só mostraria qual forma é mais frequente.

Além das três, quatro indicadores no topo — receita, negociações, ticket médio e taxa de entrega — e uma tabela com as dez maiores negociações do recorte, que serve de prova: dá para conferir na mão se o número do gráfico faz sentido.

**Filtros:** família, estado, ano do modelo e forma de pagamento, combináveis. Tudo na tela responde a eles, inclusive os indicadores e a tabela.

---

## O tratamento da base

Parti da planilha sanitizada do desafio, que traz cada campo em duas versões — o dado cru e o tratado. Usei só as colunas sanitizadas e fiz mais quatro coisas antes de embutir os dados:

| O que fiz | Por quê |
|---|---|
| Converti preço e quilometragem para número | Vinham como texto (`"$79,500.00"`, `"9,800 miles"`), e texto não soma |
| Derivei a coluna **família** do nome do modelo | 40 modelos distintos em 100 vendas não formam grupo analisável |
| Marquei como nula a data dos 24 registros `INVALID` | Data inválida não vira data; o registro continua válido para tudo que não depende dela |
| Exportei para JSON embutido no HTML | O desafio pede arquivo único, então não pode haver CSV ao lado |

Sobre os 24 registros com data inválida: eles **entram em todos os números da tela**. Nenhum dos gráficos é série temporal, então a data não faz falta ali, e excluí-los jogaria fora um quarto da receita por causa de um campo que ninguém está usando. Deixei isso escrito no rodapé da dashboard, porque quem lê um número precisa saber o que está dentro dele.

Conferência: a soma dos preços dá US$ 12.827.800,50 nas 100 linhas, e bate com a soma das seis famílias.

---

## A ferramenta e o prompt

Usei o **Claude**, em conversa, não o ChatGPT com Canvas nem um agente com skill. O desafio cita esses dois caminhos como referência; o que vale é o processo, e o processo foi o mesmo: descrever o que eu queria, olhar o resultado, pedir mudança.

### O que pedi

Anexei a planilha e pedi uma dashboard em HTML de arquivo único sobre as vendas, com três perguntas de negócio respondidas por gráfico, filtros por modelo, estado, ano e forma de pagamento, indicadores no topo e referência visual da Porsche.

### O que mudou entre a primeira versão e esta

- **A unidade de análise.** O primeiro corte era por modelo. Vendo o gráfico com 40 barras de três vendas cada, ficou claro que não respondia nada. Virou família de modelo.
- **A medida do gráfico de pagamento.** Começou como soma, que só mostrava que transferência bancária é a forma mais comum. Trocar para ticket médio mudou a pergunta de "quantos" para "quanto cada um gasta", que é a útil.
- **A referência visual.** Minha primeira ideia era fundo preto com vermelho, que é a leitura óbvia de "Porsche". O resultado parecia qualquer dashboard escuro de template. A segunda versão foi para o painel de instrumentos: prata quente de fundo, tinta grafite e o vermelho Guards (`#D5001C`) só no que é dado. A taxa de entrega virou um mostrador circular, e é o único elemento com forma — o resto é placa numérica e barra reta.
- **O tratamento dos registros inválidos**, que na primeira versão simplesmente sumia da conta sem avisar ninguém.

### Decisões técnicas

- **Sem biblioteca de gráficos.** As barras são `div` com largura percentual e o mostrador é um `circle` SVG com `stroke-dasharray`. Sem CDN, a dashboard abre offline e não quebra se um serviço sair do ar. O único recurso externo é a fonte, que tem fallback.
- **Um acento só.** Vermelho exclusivo para dado; texto nunca é vermelho. Assim a cor significa alguma coisa.
- **Estado vazio tratado.** Um recorte sem vendas mostra uma frase dizendo o que fazer, não um gráfico em branco.
- **Acessibilidade:** foco visível no teclado, barras alcançáveis por `Tab` com a dica aparecendo no foco, `prefers-reduced-motion` respeitado e os valores escritos ao lado de cada barra, não só codificados no comprimento.

---

## Como publicar (o que fiz)

1. Salvei o arquivo como `index.html` na raiz do repositório.
2. Subi para um repositório público na minha conta.
3. Settings → Pages → Source: `Deploy from a branch`, branch `main`, pasta `/ (root)`.
4. Esperei cerca de um minuto e o endereço apareceu na própria página de Settings.

---

## Prints

![Dashboard sem filtro](./images/dashboard-geral.png)

![Dashboard filtrada pela família 911](./images/dashboard-filtro-911.png)

---

## Um achado dos dados

O 911 responde por US$ 4,27 milhões dos US$ 12,83 milhões de receita — 33% do total com 23% dos carros vendidos. O Taycan vem em segundo, à frente do Cayenne, o que contraria a intuição de que SUV sustenta concessionária. Se a meta fosse receita e não volume, o estoque de elétricos mereceria mais espaço que o de utilitários.

---

## Tecnologias

HTML, CSS e JavaScript puros, arquivo único, sem dependências de build. Dados embutidos como JSON. Tipografia Barlow e Barlow Condensed. Publicação no GitHub Pages.
