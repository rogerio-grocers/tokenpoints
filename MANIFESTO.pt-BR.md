# O Manifesto TokenPoints

> Story points estão mortos. Longa vida aos TokenPoints.

Por duas décadas, equipes de software estimaram trabalho em story points — uma unidade abstrata e não falseável inventada para evasionar os fracassos óbvios das estimativas em horas. Funcionou, mais ou menos, quando humanos escreviam cada linha de código.

Esse mundo se foi.

Em 2026, a primeira versão da maioria do código de produção é escrita por um LLM. O gargalo não é mais "quanto tempo um desenvolvedor vai demorar para digitar isso?" É "quantos turnos de inferência, com qual modelo, contra qual base de código, levará para obter uma mudança funcionando, revisada e mergeada?"

Essa pergunta tem uma resposta real em dólares. Propomos usar isso.

---

## Os 6 pilares

### 1. Dólares são mais honestos que horas.

Um dólar de inferência é um fato mensurável, falseável e não negociável. Uma estimativa de hora é um contrato social negociado sob pressão. Um story point é uma vibe com fantasía.

Quando o agente escreve o código, o custo de produzir esse código não é mais um palpite — é um número na fatura da API. Use-o.

Isso **não** significa que o tempo humano deixa de importar (veja o pilar 6). Significa que, para a parte do trabalho que o LLM realmente faz, agora temos uma unidade honesta. Devemos ancorar nisso.

---

### 2. Variância é informação, não ruído.

Quando o custo real de uma tarefa é 5x sua estimativa, isso não é "o desenvolvedor foi lento". Isso é o sistema te dizendo que a tarefa continha complexidade, ambiguidade ou fricção na base de código que você não viu no planejamento.

Story points treinaram equipes para *normalizar a variância* — tratar o desvio da estimativa como fracasso. TokenPoints trata a variância como o sinal que vale a pena investigar. A pergunta interessante de retro não é mais *"por que isso levou mais tempo do que dissemos?"* É *"o que aprendemos sobre esta base de código, este prompt ou este modelo com o gap entre estimativa e realidade?"*

Uma equipe cujas estimativas são perfeitamente precisas é uma equipe que não está tentando nada novo.

---

### 3. Resultado acima de saída.

Um desenvolvedor que roteia uma tarefa difícil para Opus e gasta $30 em vez de $5 no Sonnet não está sendo desperdiçador se a sessão Opus entregar em dois turnos e a sessão Sonnet teria loopado por quinze.

Custo-por-token é uma métrica para a conta. **Custo-por-feature-mergeada** é uma métrica para o negócio. Não são o mesmo número, e confundí-los é como você arruína uma equipe.

Otimize o resultado. A conta segue depois.

---

### 4. Calibre localmente.

Este framework vem com uma escala de dimensionamento. Esses números são âncoras baseadas em comportamento observado em 2026 em várias equipes usando modelos de fronteira em bases de código de tamanho médio.

**Eles estão errados para você.**

Uma monólito Rails de 100kloc com fronteiras de domínio nítidas não é uma base de código Java enterprise de 4Mloc, e nenhum deles se assemelha a um projeto Next.js greenfield. Sua base de código, seu mix de modelos, suas ferramentas, a maturidade de prompting da sua equipe — todas essas coisas mudam a curva.

A escala é um ponto de partida. Após dois sprints de rastreamento, você substitui nossos números pelos seus. A metodologia é universal; os números são locais.

---

### 5. Multidimensional, não unidimensional.

O maior erro que story points cometeu foi colapsar toda a "esforço" em um único número. Equipes então otimizaram esse número e quebraram tudo mais.

Não repita o erro. Rastreie no mínimo:

- **Custo em USD** (a manchete)
- **Tokens in / tokens out** (para que você possa re-derivar custo conforme preços mudam)
- **Modelo(s) usado(s)** (Opus é ~5x Sonnet em input, isso importa)
- **Turnos para conclusão** (um proxy para ambiguidade)
- **Tempo humano** (revisão, planejamento, integração — veja pilar 6)
- **Resultado** (mergeado? revertido? bugs em prod nos primeiros 30 dias?)

Uma tarefa que custou $4 e entregou limpa é um animal diferente de uma tarefa que custou $4 e foi revertida. Não deixe um número esconder o outro.

---

### 6. Tempo humano ainda existe.

LLMs atualmente não fazem descoberta de produto, alinhamento de stakeholders, revisão de código, orquestração de deployment, resposta on-call, ou as dezenas de outras coisas que transformam um diff funcionando em valor entregue. fingir o contrário produz estimativas perigosamente baixas.

TokenPoints estima o *custo de inferência de produzir a mudança.* Ele **não** estima:

- Tempo gasto em planejamento, refinamento ou discussão de arquitetura
- Revisão de código e round-trips com revisores
- QA, testes manuais, validação em staging
- Deployment, rollout, monitoramento
- Documentação, comunicação, handoff

Rastreie o tempo humano **separadamente** do custo de inferência. Ambos são reais. Nenhum dos dois sozinho diz quanto uma tarefa "custa". Qualquer um que venda a você uma estimativa de número único está vendendo a você um story point com um sinal de dólar pintado nele.

---

## O que isto não é

- **Não é uma métrica de produtividade para indivíduos.** Comparar $/tarefa entre desenvolvedores é a versão moderna de comparar linhas de código. Será manipulado, danificará a confiança e medirá a coisa errada. Não faça isso.
- **Não é uma forma de tornar estimativas "objetivas".** É uma forma de ancorar estimativas em algo mensurável. A *previsão* ainda é uma previsão — é o *número retrospectivo* que agora é honesto.
- **Não é um substituto para pensar.** Uma equipe que adota TokenPoints e para de perguntar "isso é a coisa certa para construir?" perdeu completamente o ponto.

---

## Um pequeno pedido

Se este framework ajudar sua equipe, compartilhe o que aprendeu. Os números no guia de dimensionamento ficam mais precisos cada vez que outra equipe contribui com sua calibração. Veja [CONTRIBUTING.md](CONTRIBUTING.md).

Se não ajudar sua equipe, nos diga por quê. Isso é mais valioso do que outra história de sucesso.

---

*Versão 0.1 — aberto para revisão. Os pilares são a parte estável. Os números não são.*
