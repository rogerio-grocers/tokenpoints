# TokenPoints

*Read this in [English](README.md).*

> **Story points estão mortos. Longa vida aos TokenPoints.**

Um framework para estimar trabalho de software em **dólares de inferência de LLM**, não em horas ou story points.

---

## A mudança

Quando humanos escreviam 100% do código, as horas eram um (mau) proxy para esforço. Story points tentaram corrigir isso e, na maioria das vezes, apenas tornaram tudo mais estranho — abstratos, não falseáveis e trivialmente manipuláveis.

Em 2026, quando um agente escreve a primeira versão da maioria do seu código, a pergunta mudou:

> **"Quanto vai custar para entregar esta tarefa?"**

Esse custo agora é um *número mensurável e falseável em USD* — o preço dos tokens que o modelo gasta para realizar o trabalho. Rastreá-lo não é mais difícil do que rastrear tempo, e diferente do tempo, ele não mente.

TokenPoints é um vocabulário de planejamento construído em torno desse número.

---

## O que você obtém

- **Um manifesto de 6 pilares** — por que dólares vencem pontos, e onde estão os limites.
- **Uma escala de dimensionamento (XS → XL)** — calibrada para sessões reais de codificação com agentes em 2026, ancorada em faixas de USD, não em vibes.
- **Um playbook de calibração** — como sua equipe transforma a escala em *seus* números em dois sprints.
- **Modelos de rastreamento** — os dados mínimos para capturar para que a calibração realmente aconteça.
- **Guias de integração ágil** — como isso se encaixa no Scrum, Kanban ou qualquer metodologia que você já use.
- **Anti-padrões** — as formas óbvias de usar isso incorretamente e como evitá-las.

---

## Início rápido

1. Leia o **[Manifesto](MANIFESTO.pt-BR.md)** (5 min). Se você discordar, este framework não é para você e tudo bem.
2. Folheie o **[Guia de Dimensionamento](docs/sizing-guide.md)** para internalizar a escala XS–XL.
3. Use o **[modelo de estimativa](templates/estimation-template.md)** nas suas próximas 10 tarefas. Não mude mais nada ainda.
4. Após dois sprints, execute a **[calibração](docs/calibration.md)** com seus dados reais.
5. Uma vez calibrado, integre ao planejamento — **[guia de integração](docs/integration-agile.md)**.

> **Tempo para primeira estimativa útil: ~10 minutos.**
> **Tempo para linha de base calibrada da equipe: ~2 sprints.**

---

## A escala de dimensionamento (linha de base não calibrada)

| Tamanho | Custo (USD) | Padrão típico | Tempo humano |
|---------|--------------|-------------------|--------------|
| **XS** | < $1 | Edição pontual, pesada em autocompletar | < 30 min |
| **S** | $1 – $8 | Feature/bug de arquivo único, 5–15 turnos | 30 min – 2h |
| **M** | $8 – $40 | Feature multi-arquivos, 15–40 turnos | 2 – 8h |
| **L** | $40 – $160 | Refatoração, debug profundo, cross-module | 1 – 3 dias |
| **XL** | $160 – $400 | Mudança arquitetural, multi-sistema | 3+ dias |
| **??** | desconhecido | Spike primeiro — investigue antes de dimensionar | time-boxed |

Qualquer coisa acima de XL **deve ser decomposta.** Se você não consegue decompô-la, você ainda não entende ela — isso é um spike.

Essas faixas são **âncoras iniciais**, não leis. Sua equipe divergirá baseada no tamanho da base de código, mix de modelos e ferramentas. Veja **[Calibração](docs/calibration.md)**.

---

## Os 6 pilares

1. **Dólares são mais honestos que horas.**
2. **Variância é informação, não ruído.**
3. **Resultado acima de saída.**
4. **Calibre localmente.**
5. **Multidimensional, não unidimensional.**
6. **Tempo humano ainda existe.**

Elaboração completa: **[MANIFESTO.pt-BR.md](MANIFESTO.pt-BR.md)**.

---

## Mapa do repositório

```
tokenpoints/
├── README.md                       ← versão em inglês
├── README.pt-BR.md                  ← você está aqui
├── MANIFESTO.md                    ← os 6 pilares (inglês)
├── MANIFESTO.pt-BR.md               ← os 6 pilares (português)
├── docs/
│   ├── framework.md                ← visão geral end-to-end
│   ├── sizing-guide.md             ← XS–XL com exemplos trabalhados
│   ├── calibration.md              ← transforme a escala em seus números
│   ├── tracking.md                 ← o que medir, como
│   ├── integration-agile.md        ← ajuste para Scrum / Kanban
│   └── anti-patterns.md            ← como usar isso incorretamente
├── templates/
│   ├── estimation-template.md
│   ├── tracking-sheet.csv
│   └── retrospective-template.md
├── examples/
│   ├── frontend-feature.md
│   ├── backend-refactor.md
│   └── debug-session.md
└── CONTRIBUTING.md
```

---

## Contribuindo

A coisa mais valiosa que você pode contribuir é **os dados de calibração da sua equipe** — médias anonimizadas, mix de modelos, contexto da base de código. Com o tempo, isso transforma o repositório de um framework de uma pessoa em uma referência empírica.

Veja **[CONTRIBUTING.md](CONTRIBUTING.md)** para como fazer.

---

## Status

**v0.1 — rascunho inicial.** Nomes, faixas e pilares estão abertos para contribuição da comunidade. Se você tem uma opinião forte, abra uma issue ou um PR. A escala evoluirá conforme mais equipes reportarem dados.

---

## Licença

[CC BY 4.0](LICENSE) — use, faça fork, construa em cima. Apenas credite a fonte.

---

*Criado por [Rogério](https://github.com/rogerio-grocers) — proposta, não prescrição.*
