# Declaração de uso de IA

## Ferramenta e modelo

- **Ferramenta:** Claude (Anthropic), em modo agente.
- **Modelos:** `claude-sonnet-4-6` (início da sessão, 11 e 12/11) e `claude-sonnet-5-5` (restante da sessão).
- **Sessão completa:** em `sessao.md`, nesta mesma pasta.

## O que a IA fez

- Ajudou-me a entender o funcionamento da aplicação e as regras de negócio. Nos primeiros dias discutimos as funcionalidades e pedi que não me desse as respostas, apenas que me orientasse sobre onde olhar.
- Leu o código do projeto desde o início e me guiou com perguntas, sem apontar qual linha tinha defeito, conforme eu pedi.
- Avaliou as minhas respostas e me ajudou a rodar os comandos de teste e a preparar o ambiente.
- Revisou o código e as entregas comigo, e criou a estrutura dos arquivos de entrega.
- Deu dicas mais diretas em dois pontos: no ranking (apresentou o parâmetro `reverse` e a dica do sinal de menos) e no limite de leitura (indicou o arquivo onde olhar e que o `if` com o `break` devia ser removido).

## O que eu fiz

- Identifiquei os erros, que se concentravam em lógica de programação, sintaxe e desacordo com as regras de negócio, lendo os logs dos testes, verificando informações e analisando o código, com a orientação descrita acima.
- Analisei e compreendi as regras de negócio do projeto para identificar os erros com mais clareza.
- Escrevi as correções, fiz os commits e redigi o README-ENTREGA.
- Percebi que o limite de leitura não era coberto pelos testes, ao comparar o `Reservas:` da CLI com o número de linhas do CSV.

