# Dietary · Bia

App de alimentação em uma única página (`index.html`), com a **Bia**, assistente de IA que ajuda a organizar refeições, fazer trocas e entender os alimentos. Os dados de cada pessoa ficam só no aparelho dela.

## A Bia responde para qualquer pessoa

A Bia tenta, nesta ordem:

1. **IA completa pelo servidor** (`api/bia-chat.js`): quem abre o app não precisa configurar nada, porque a chave da IA fica guardada no servidor.
2. **IA do Claude**: quando o app é aberto pelo link publicado no Claude.
3. **Bia essencial**: se nenhuma das anteriores estiver disponível (sem servidor, sem internet, limite atingido), a Bia responde **no próprio aparelho**. Ela cobre calorias e nutrientes (base USDA embutida, com a fonte citada), trocas no cardápio respeitando restrições, próxima refeição, metas, resumo do dia, água, pós-treino, lanches, opções baratas e situações delicadas (CVV 188, SAMU 192). O topo do chat mostra "Modo essencial" e cada resposta traz essa etiqueta.

As respostas se adaptam ao perfil: idade (menores de 18 sem metas de calorias, cuidados para 60+), padrão alimentar (vegetariano, vegano), alergias e intolerâncias, orçamento e tempo para cozinhar. Cada resposta tem o botão **🔊 Ouvir**, que lê o texto em voz alta.

## Publicar com a IA completa (Vercel)

1. Importe este repositório na [Vercel](https://vercel.com/new). Não há etapa de build: a página é estática e a pasta `api/` vira a função do servidor.
2. Em **Settings → Environment Variables**, crie `ANTHROPIC_API_KEY` com a sua chave da [Anthropic](https://console.anthropic.com/).
3. Publique. O app chama `/api/bia-chat` no mesmo endereço, sem configurar mais nada.

Se a página e o servidor ficarem em endereços diferentes, preencha `window.BIA_API_URL` no início do `index.html` e liste o endereço da página em `BIA_ALLOWED_ORIGINS`.

| Variável | Para quê | Padrão |
|---|---|---|
| `ANTHROPIC_API_KEY` | Chave da IA (obrigatória) | não definida |
| `BIA_MODEL` | Modelo usado | `claude-opus-5-5` |
| `BIA_RATE_PER_DAY` | Mensagens por pessoa (IP) por dia | `80` |
| `BIA_DAILY_CAP` | Mensagens por dia no total | `3000` |
| `BIA_ALLOWED_ORIGINS` | Endereços de outras páginas que podem usar o servidor (separados por vírgula) | nenhum |

Os limites ficam na memória do servidor e são aproximados. Para um teto rígido de gasto, defina também um limite mensal no Console da Anthropic. Quando um limite é atingido, a Bia essencial continua respondendo.

## Para o dono do app

Abra o app com `#dono` no fim do endereço (ou toque 5 vezes no cabeçalho da Bia) para ver os detalhes técnicos de erros e o botão ⚙, que conecta uma chave própria só naquele aparelho.
