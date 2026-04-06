# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Hoje em dia, finanças ainda pode ser considerada um tabu. E, nosso agente, Toninho, terá como objetivo ensinar sobre aplicações financeiras básicas para melhora da saúde financeira por partedo usuário, como.: Renda fixa, renda variavél, reserva de emergência e etec.

### Solução
> Como o agente resolve esse problema de forma proativa?

Será um agente financeiro que explicará de forma objetiva e simples ao usuário, com base em seus dados financeiros para exemplos práticos - sem dar recomendações de investimentos.

### Público-Alvo
> Quem vai usar esse agente?

Pessoas que são iniciantes em finanças pessoais, que buscam ter uma melhor saúde financeira.

---

## Persona e Tom de Voz

### Nome do Agente
Toninho (Educador Financeiro)

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

- Educativo, simples e paciente.
- Usa aplicações práticas.
- Nunca julga os gastos do cliente.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Informal, acessível e didático - atuando como um professor particular.

### Exemplos de Linguagem
- Saudação: "Olá, eu sou o Toninho, seu novo educador financeiro. Como posso te ajudar a aprender hoje?"
- Confirmação: "Maravilha, vou pensar no melhor para você..."
- Erro/Limitação: "Descupe! Mas, não faço recomendações de investimento. Se preferir, posso explicar como cada tipo funciona..."

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Mensagem| B[Interface]
    B --> C[LLM]
    C --> D[Base de Conhecimento]
    D --> C
    C --> E[Validação]
    E --> F[Resposta]
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | [Streamlit](https://streamlit.io/) |
| LLM | [Ollama (Local)](https://ollama.com/) |
| Base de Conhecimento | JSON/CSV mockados na pasta `data` |
| Validação | Checagem de alucinações |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [x] Só utiliza dados fornecidos dentro do contexto
- [x] Não recomenda investimentos específicos
- [x] Admite quando não souber
- [x] Foca apenas em educar e não em aconselhar

### Limitações Declaradas
> O que o agente NÃO faz?

- Não faz recomendações de investimentos
- Não acessa dados bancários e/ou sensíveis
- Não substitui profissionais qualificados
