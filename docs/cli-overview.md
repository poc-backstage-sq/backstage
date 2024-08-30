---
id: visao-geral-cli
title: Visão Geral
description: Visão geral do Backstage CLI
---

## Introdução

Um dos objetivos do Backstage é fornecer uma experiência de desenvolvimento agradável no projeto e ao redor dele. Criar novos aplicativos e plugins deve ser simples, a velocidade de iteração deve ser rápida e o esforço de manter ferramentas personalizadas deve ser mínimo. Como parte desse objetivo, o Backstage fornece seu próprio sistema de compilação e ferramentas, entregues principalmente por meio do pacote [`@backstage/cli`](https://www.npmjs.com/package/@backstage/cli). Ao criar um aplicativo usando [`@backstage/create-app`](https://www.npmjs.com/package/@backstage/create-app), você recebe um projeto que já está preparado com uma configuração típica e scripts de pacote para executar os comandos mais comuns.

Por baixo dos panos, o CLI usa o [Webpack](https://webpack.js.org/) para empacotar, o [Rollup](https://rollupjs.org/) para construir pacotes, o [Jest](https://jestjs.io/) para testar e o [eslint](https://eslint.org/) para linting. Ele também inclui ferramentas para trabalhar com aplicativos Backstage, por exemplo, para manter o aplicativo atualizado e verificar a configuração estática. Para uma visão mais detalhada das ferramentas, consulte a página [sistema de compilação](./cli-build-system.md) e, para uma lista de comandos, consulte a página [comandos](./cli-commands.md).

Embora as ferramentas do Backstage sejam opinativas em relação ao seu funcionamento, também é possível usar suas próprias ferramentas, parcial ou totalmente. Por exemplo, o CLI fornece um comando para construir um pacote de plugin para publicação, mas a saída é uma combinação bastante padrão de JavaScript transpilado e declarações de tipo TypeScript. O uso do comando do CLI pode ser complementado ou substituído por outras ferramentas, se necessário.

O Backstage CLI intencionalmente não fornece muitos ganchos para substituir ou personalizar o processo de compilação. Isso permite a evolução do CLI sem ter que levar em conta uma ampla superfície de API. Isso nos permite iterar e melhorar as ferramentas, além de manter o sistema atualizado com mais facilidade.

## Glossário

- **Pacote** - Um pacote no ecossistema Node.js, frequentemente publicado em um registro de pacotes como o [NPM](https://www.npmjs.com/).
- **Monorepo** - Uma estrutura de projeto que consiste em vários pacotes dentro de um único projeto, onde os pacotes podem ter dependências locais entre si. Frequentemente habilitado por meio de ferramentas como o [lerna](https://lerna.js.org/) e [yarn workspaces](https://classic.yarnpkg.com/en/docs/workspaces/).
- **Pacote Local** - Um dos pacotes dentro de um monorepo. Esses pacotes podem ou não ser publicados em um registro de pacotes.
- **Bundle** - Uma coleção de artefatos de implantação. A saída do processo de empacotamento, que reúne uma coleção de pacotes em uma única coleção de artefatos de implantação.
- **Função do Pacote** - A função declarada de um pacote, consulte [funções do pacote](./cli-build-system.md#funções-do-pacote).
