# Agente de busca de estágios

Agente de IA que procura vagas de estágio em **IA** e em **finanças + tecnologia** em São Paulo (e remoto/híbrido no Brasil), filtra pelo meu perfil e me manda um resumo por email três vezes por semana.

Feito por Thomas Martins (Economia, FGV EESP · Vice Presidente da FGV Quant).

## Como funciona

```
Agendamento (seg, qua, sex às 17h43, horário de Brasília)
        │
        ▼
Agente Claude (tarefa agendada, roda na nuvem)
        │
        ├── Gupy (connector MCP): busca por termos como "IA", "dados", "machine learning", "quant", "fintech"
        ├── Plataformas de vagas de startups: Ashby, Greenhouse, Lever, Sólides, InHire
        ├── Páginas de carreira: Nubank, Itaú, BTG, XP, C6, QI Tech, Kinea, iFood, Blip e outras
        └── Notícias de programas de estágio recém abertos (só para descobrir; o link final é sempre o oficial)
        │
        ▼
Filtro e ranking pelo perfil (Python, modelagem quantitativa, IA, finanças)
        │
        ├── Consulta vagas/historico.csv para não repetir vagas já enviadas
        ├── Atualiza vagas/historico.csv com as vagas novas
        ▼
Resumo por email e notificação no celular
```

## Decisões de projeto

- **Sem LinkedIn**: o LinkedIn bloqueia leitura automática (robots.txt). O agente procura a mesma vaga no Gupy ou na página de carreira da empresa.
- **Nunca inventar**: só entram vagas que o agente realmente abriu, com link e data verificados. Sem data visível, a vaga é marcada como "data não informada".
- **Só vagas novas**: prioridade para vagas publicadas nos últimos 3 a 4 dias, e o histórico em `vagas/historico.csv` evita repetir o que já foi enviado.
- **Sem servidor e sem chave de API**: roda como tarefa agendada do Claude, sem infraestrutura própria.

## Estrutura

| Caminho | O que é |
|---|---|
| `instrucoes/prompt.md` | Instruções completas do agente (perfil, fontes, regras e formato do email) |
| `vagas/historico.csv` | Todas as vagas já enviadas, usado para não repetir |
| `vagas/AAAA-MM-DD.md` | Resumo enviado em cada execução |

## Exemplo de saída

Veja os arquivos em [`vagas/`](vagas/).
