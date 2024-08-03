<h1 align="center">
  <img src="assets/reprograma-fundos-claros.png" alt="logo reprograma" width="500">
</h1>

# Tema da Aula

Turma Online ON32 - Imersão JavaScript | Semana 09 | 2024 | Professora Beatriz Duarte

### Instruções
Antes de começar, vamos organizar nosso setup.
* Fork esse repositório 
* Clone o fork na sua máquina (Para isso basta abrir o seu terminal e digitar `git clone url-do-seu-repositorio-forkado`)
* Entre na pasta do seu repositório (Para isso basta abrir o seu terminal e digitar `cd nome-do-seu-repositorio-forkado`)
* [Add outras intrucoes caso necessario]

### Objetivo
- Compreender os diferentes tipos de erros que podem ocorrer em APIs
- Aprender estratégias eficazes para lidar com erros
- Compreender sobre a importância de logs

### Resumo
O que veremos na aula de hoje?
- [Gestão de erros](#tema-da-aula)
    - [Instruções](#instruções)
    - [Objetivo](#objetivo)
    - [Resumo](#resumo)

- [Conteúdo](#conteúdo)
  - [Importância da Gestão de erros](#importancia)
  - [Tipos de Erros em API](#tipos-erros)
    - [Erros do Cliente](#erros-cliente)
    - [Erros do Servidor](#erros-servidor)
    - [Erros de Validação](#erros-validacao)
  - [Middlewares](#middlewares)
    - [O que é um middleware?](#oq-middleware)
    - [Vantagens](#vantagens)
  - [Logs no Sistema](#logs)
  - [Exercícios](#exercícios)
  - [Material da aula](#material-da-aula)
  - [Links Úteis](#links-úteis)

# Conteúdo
<a id="importancia"></a>

## Por que a gestão de erros é importante em um sistema?

Ter uma gestão eficaz de erros é essencial para qualquer sistema, especialmente para APIs, pois impacta diretamente a experiência do usuário, a manutenção do sistema, a segurança, a eficiência operacional, uma boa estratégia de gestão de erros não apenas melhora a qualidade do sistema, mas também aumenta a confiança e a satisfação dos usuários.

### Melhor experiência do usuário

- **Feedback claro:** Mensagens de erro claras e informativas ajudam os usuários a entenderem o que deu errado e como podem corrigir o problema.
- **Confiança no sistema:** Uma gestão de erros eficaz transmite confiabilidade. Quando os usuários recebem mensagens de erro amigáveis e úteis, eles confiam mais no sistema.
- **Menos frustração:** Usuários frustrados tendem a abandonar o sistema. Erros bem geridos minimizam a frustração e melhoram a retenção de usuários.

### Manutenção e debug mais fácil

- **Identificação rápida de problemas:** Erros bem documentados e com mensagens detalhadas ajudam os desenvolvedores a identificar e corrigir problemas mais rapidamente.
- **Logs detalhados:** Registros de erros detalhados são essenciais para a debugar e para entender o comportamento do sistema em diferentes cenários.o falhou.

### Eficiência

- **Menor tempo de inatividade:** Uma gestão eficaz de erros permite a rápida identificação e correção de problemas, reduzindo o tempo de inatividade do sistema.
- **Melhor uso de recursos:** Erros bem geridos evitam o desperdício de recursos computacionais e humanos, focando a equipe em melhorias e novas funcionalidades.

<a id="tipos-erros"></a>

## Tipos de erros em APIs

<a id="erros-cliente"></a>

### 1. Erros do Cliente (Client Errors)

Os erros do cliente ocorrem quando a requisição feita para a API está incorreta ou não pode ser processada devido a problemas do lado do cliente. Eles geralmente estão na faixa de códigos de status HTTP 4xx.

**Exemplos:**

- **400 Bad Request:** Requisição inválida ou malformada.
    - Exemplo: Tentar adicionar uma despesa sem o campo obrigatório 'valor'.
- **401 Unauthorized:** Falta de autenticação ou credenciais inválidas.
    - Exemplo: Tentar acessar dados de despesas sem estar autenticado.
- **403 Forbidden:** O usuário não tem permissão para acessar o recurso solicitado.
    - Exemplo: Tentar acessar despesas de outro usuário.

<a id="erros-servidor"></a>

### 2. Erros do Servidor (Server Errors)

Os erros do servidor ocorrem quando a API encontra um problema ao processar uma requisição válida. Esses erros indicam que o problema está no servidor. Eles geralmente estão na faixa de códigos de status HTTP 5xx.

**Exemplos:**

- **500 Internal Server Error:** Ocorreu um erro inesperado no servidor.
    - Exemplo: Falha ao recuperar dados de despesas devido a um bug no código.
- **502 Bad Gateway:** O servidor recebeu uma resposta inválida ao tentar processar a requisição.
    - Exemplo: Falha de comunicação entre a API e um serviço externo.
- **503 Service Unavailable:** O servidor está temporariamente indisponível.
    - Exemplo: O banco de dados da API está fora do ar.

<a id="erros-validacao"></a>

### 3. Erros de Validação (Validation Errors)

Os erros de validação ocorrem quando os dados fornecidos na requisição não atendem aos requisitos esperados pela API. Esses erros geralmente são tratados como um subconjunto dos erros do cliente, com códigos de status HTTP específicos.

**Exemplos:**

- **422 Unprocessable Entity:** A requisição está bem-formada, mas não pode ser processada devido a erros semânticos.
    - Exemplo: Tentar adicionar uma despesa com um valor negativo.
- **400 Bad Request:** Entrada de dados em um formato incorreto.
    - Exemplo: Enviar a data da despesa em um formato inválido, como 'DD-MM-AAAA' ao invés de 'AAAA-MM-DD'.

## Estratégia para gestão de erros

### Componentes dos Padrões de Resposta:

- **Status HTTP:** Código de status que indica o tipo de erro (ex: 400, 401, 500).
- **Mensagens de Erro:** Mensagens descritivas que ajudam a entender o problema.
- **Estrutura de Dados:** Formato consistente para retornar informações de erro (ex: JSON).

```
{
"status": 400,
"error": "Bad Request",
"message": "O campo 'valor' é obrigatório."
}
```

<a id="middlewares"></a>

## Middleware de Tratamento de Erros

<a id="oq-middlewares"></a>

### O que é Middleware?

Middleware é uma função em aplicativos web que tem acesso ao objeto de solicitação (request), ao objeto de resposta (response) e à próxima função middleware no ciclo de solicitação-resposta de uma aplicação. Essas funções podem executar qualquer código, modificar a solicitação e a resposta, finalizar o ciclo de solicitação-resposta ou chamar o próximo middleware.

### O que é um Middleware de Tratamento de Erros?

Um middleware de tratamento de erros é uma função especializada no ciclo de solicitação-resposta que captura, gerencia e responde a erros que ocorrem durante a execução da aplicação. Ele centraliza a lógica de tratamento de erros, garantindo que todos os erros sejam processados de maneira consistente e apropriada.

<a id="vantagens"></a>

### Vantagens

1. **Centralização:** Um único ponto para capturar e gerenciar erros, facilitando a manutenção e a consistência na resposta a erros.
2. **Segurança:** Permite ocultar detalhes internos do erro, evitando a exposição de informações sensíveis aos usuários finais.
3. **Registro de erros:** Facilita o registro e a análise de erros, ajudando na identificação e correção de problemas.
4. **Melhoria da experiência do usuário:** Fornece mensagens de erro claras e amigáveis, melhorando a usabilidade e a confiança dos usuários no sistema.

<a id="logs"></a>

## Logs no sistema

Qual a importância dos logs?

1. **Diagnóstico de problemas:** Logs de erros fornecem informações detalhadas que ajudam na identificação e resolução de problemas.
2. **Manutenção proativa:** Com logs, é possível detectar padrões e antecipar problemas antes que eles afetem os usuários.
3. **Segurança:** Monitorar erros pode ajudar a identificar e mitigar ataques ou tentativas de exploração de vulnerabilidades.
4. **Conformidade:** Algumas regulamentações exigem que as empresas mantenham registros detalhados de erros e falhas do sistema.

Um log de erros deve incluir informações suficientes para identificar e entender o erro:

- **Data e Hora:** Quando o erro ocorreu.
- **Nível de Severidade:** Gravidade do erro (ex: info, warning, error, critical).
- **Mensagem de Erro:** Descrição clara do erro.
- **Contexto:** Informações adicionais que ajudam a entender o contexto do erro (ex: usuário, endpoint, parâmetros).


***
### Exercícios 
* [Exercicio para sala](/exercicios/para-sala/)
* [Exercicio para casa](/exercicios/para-casa/)

### Material da aula 
* [Material](/material)

### Links Úteis
* 

<p align="center">
Desenvolvido com :purple_heart:  
</p>

