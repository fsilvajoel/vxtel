# Aplicação VxTel

## Descrição resumida

Foram desenvolvidas duas aplicações (backend & frontend) que simulam um sistema
de consulta de valores de chamadas de longa distância para uma empresa
de telefonia chamada VxTel.

## Tecnologias utilizadas

**Backend**
- Node.js
- Typescript
- Express
- Tsyringe - para injeção de dependências 
- Celebrate - para validações de dados enviados no body
- Prettier, eslint e editorconfig - para manter uma certa qualidade no código e
  seguir um padrão já utilizado no mercado, neste caso o padrão escolhido foi do airbnb.
- Jest - para realização de testes

### Como testar o backend

1 - Instalar dependência do projeto
```shell
yarn 
```

2 - Rodar servidor node/express
```shell
yarn dev:server
```

3 - Rodar testes 
```shell
yarn test
```

4 - Teste de endpoints
4.1 - Listagem de planos
* GET http://localhost:3333/plans

*exemplo de retorno*

```json
[
  {
    "id": 1,
    "name": "FaleMais 30",
    "duration": 30
  },
  {
    "id": 2,
    "name": "FaleMais 60",
    "duration": 60
  },
  {
    "id": 3,
    "name": "FaleMais 120",
    "duration": 120
  }
]
```

4.2 - Consulta do valor da ligação
* POST http://localhost:3333/calls

*payload (body json)*

```json
{
	"plan_id": 3,
	"origin": "018",
	"destiny": "011",
	"minutes": 200
}
```

*exemplo retorno*
```json
{
  "more_economically": 167.2, 
  "no_more_economically": 380
}
```



## Princípios aplicados no backend

**Camada de domínio**
Área de conhecimento de determinado "módulo". 

**Camada de infra**
São ferramentas que escolhemos para se relacionar com a camada de domínio
* Faz diferença para o usuário se estamos usando postgresql ou mysql?
* Se usamos Node ou Laravel?
*São decisões técnicas:*
* Banco de dados
* Express/Laravel/etc
* Envio de e-mails

**Service**
Regra de negócio - Quando é algo muito especifico da aplicação:

* Recebimento
* Tratativa de erros/exceções
* Acesso ao repositório

**Repositories**
Repositório é um serviço de domínio, que abstrai a camada de persistência da
aplicação e atua como uma API para os serviços de aplicação (controllers, CLI,
etc). Ou seja, deve ser a única fonte de acesso confiável aos objetos de domínio e
não deve ser responsável por conexões ao banco, envio de e-mail, etc...

**Principios do SOLID utilizados**

Repositories - LINSKOV-SUBSTITUTION-PRINCIPLE - 
Camadas que são integrações com outras bibliotecas devem ser substituíveis
quando necessário, definindo uma camada de regras.

Services - DEPENDENCY INVERSION - 
Os repositories não são instanciados diretamente no service, são injetados e
apenas a interface é definidada. 

**Frontend**
- React.js
- Typescript
- Yup - para validações de dados
- Axios - para realização de requisições para API
- Prettier, eslint e editorconfig - para manter uma certa qualidade no código e
  seguir um padrão já utilizado no mercado, neste caso o padrão escolhido foi do airbnb.
- Styled Components - para estilização CSS
- Polished - para trabalhar com efeito fade
- Formik - para trabalhar com formulários e campos

### Como testar o frontend

1 - Instalar dependência do projeto
```shell
yarn 
```

2 - Rodar aplicação
```shell
yarn start
```

3 - Acessar:
```
http://localhost:3000
```

Prints:


  <img
  src="https://raw.githubusercontent.com/adrianoluisalmeida/vxtel/master/assets/screen-mobile.png"
  width="300">


  <img
  src="https://raw.githubusercontent.com/adrianoluisalmeida/vxtel/master/assets/screen-desktop.png"
  width="800">


## Objetivo 
A empresa de telefonia VxTel, especializada em chamadas de longa
distância nacional, vai colocar um novo produto no mercado chamado FaleMais.

Normalmente um cliente VxTelpode fazer uma chamada de uma cidade para outra
pagando uma tarifa fixa por minuto, com o preçosendo pré-definidoem uma lista
com os códigosDDDs de origem e destino:

Listagem de tarifas:
[calls.json](./backend/src/modules/calls/infra/database/data/calls.json)

Listagem de planos:
[calls.json](./backend/src/modules/plans/infra/database/data/plans.json)

Com o novo produto FaleMaisda VxTelo cliente adquire um plano e pode falar de
graçaatéum determinado tempo (em minutos) e sópaga os minutos excedentes. Os
minutos excedentes tem um acrescimo de 10% sobre a tarifa normal do minuto. Os
planos são FaleMais 30 (30 minutos), FaleMais 60 (60 minutos) e FaleMais 120
(120 minutos).

A VxTel, preocupada com a transparênciajunto aos seus clientes, quer
disponibilizar uma páginana web onde o cliente pode calcular o valor da ligação.
Ali, o cliente pode escolher os códigosdas cidades de origem e destino, o tempo
da ligaçãoem minutos e escolher qual o plano FaleMais. O sistema deve mostrar
dois valores: (1) o valor da ligaçãocom o plano e (2) sem o plano. O custo
inicial de aquisiçãodo plano deve ser desconsiderado para este problema. 

