# Internship search agent

An AI agent that looks for internships at top companies that combine **technology, AI and finance** in São Paulo (or remote/hybrid in Brazil), filters them against my profile, and emails me a detailed summary three times a week.

Built by Thomas Martins (Economics, FGV EESP · Vice President of FGV Quant).

## Focus

The main criterion is the **quality and profile of the company**, not just the job title. Two groups:

1. **AI-heavy fintechs and startups**: companies solving real problems with AI and data. My reference companies are **Brex, QI Tech and Enter**; others in the same spirit include CloudWalk (InfinitePay), Pismo, Dock, Cora, Creditas, Asaas, Clara, Conta Simples, Belvo, Celcoin, Neon, Nuvemshop and iFood.
2. **Large companies that combine tech and finance**: Nubank, Mercado Pago/Mercado Livre, Stone, PicPay, Inter, C6 Bank, XP, BTG Pactual, Itaú, B3 and similar, always in technology, data, AI, product or engineering roles.

Target roles: AI, machine learning, data science, LLMs, AI automation, AI product/ops, software engineering, product, data and analytics for financial products, and credit modeling.

Filtered out: "pure" quant finance (quant or investment managers, trading desks, fund research, securitization), companies with no tech or finance profile, roles that are not internships, roles outside Brazil that require relocation, and administrative, sales or HR roles. Fewer, better matches beat a long list.

## How it works

```
Schedule (Mon, Wed, Fri at 5:43 pm, Brasília time)
        │
        ▼
Claude agent (scheduled task, runs in the cloud)
        │
        ├── Gupy (MCP connector): searches for AI, data, ML, data science, fintech, product and software engineering internships
        ├── Startup and tech job boards: Ashby, Greenhouse, Lever, Sólides, InHire, Recrutei
        ├── Career pages of target companies (Brex, QI Tech and Enter are checked on every run)
        └── News about newly opened internship programs (discovery only; the final link is always the official one)
        │
        ▼
Filtering and ranking by company quality and fit with my profile
        │
        ├── Reads vagas/historico.csv so jobs already sent are not repeated
        ├── Appends the new jobs to vagas/historico.csv
        ▼
Detailed email summary (in Portuguese) and a phone notification
```

Each job in the email gets a full breakdown: about the team, responsibilities, requirements (including whether an Economics degree and my graduation date fit), conditions, selection process, why it matches my résumé, red flags, and a 1 to 5 fit score, plus a "pick of the day".

## Design decisions

- **No LinkedIn**: LinkedIn blocks automated reading (robots.txt), so the agent looks for the same job on Gupy or the company's career page instead.
- **Never make things up**: only jobs the agent actually opened make it in, with verified links and dates. Anything a page doesn't state is reported as "não informado" (not stated).
- **New jobs only**: priority goes to jobs posted in the last 3 to 4 days, and the history in `vagas/historico.csv` prevents resending.
- **No server and no API key**: runs as a scheduled Claude task, with no infrastructure of its own.

## Structure

| Path | What it is |
|---|---|
| `instructions/prompt.md` | Full agent instructions (profile, sources, rules and email format) |
| `vagas/historico.csv` | Every job already sent, used to avoid repeats |
| `vagas/YYYY-MM-DD.md` | The summary sent on each run (in Portuguese) |

## Sample output

See the files in [`vagas/`](vagas/).
