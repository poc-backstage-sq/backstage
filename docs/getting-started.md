---
id: getting-started
title: Começando
description: Documentação sobre como começar com o Backstage
---

Para a maioria das instalações do Backstage, instalar o aplicativo autônomo trará a melhor e mais simplificada experiência. Neste guia, você aprenderá a:

- Implantar o Backstage Autônomo com pacotes npm
- Executar o Backstage Autônomo com um banco de dados SQLite em memória e conteúdo de demonstração

Este guia pressupõe um entendimento básico de trabalho em um sistema operacional baseado em Linux usando ferramentas como apt-get, npm, yarn e curl. O conhecimento em Docker também é útil para aproveitar ao máximo a instalação do Backstage.

Se você planeja contribuir com plugins ou com o projeto em geral, recomendamos que você use o guia de [Contribuidores](https://github.com/backstage/backstage/blob/master/CONTRIBUTING.md) para fazer uma instalação baseada em repositório.

### Pré-requisitos

- Acesso a um sistema operacional baseado em Unix, como Linux, MacOS ou
  [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/)
- Um ambiente de compilação semelhante ao GNU disponível na linha de comando.
  Por exemplo, no Debian/Ubuntu, você vai querer ter os pacotes `make` e `build-essential` instalados.
  No MacOS, você vai querer ter executado `xcode-select --install` para obter as ferramentas de compilação da linha de comando do XCode.
- Uma conta com direitos elevados para instalar as dependências
- `curl` ou `wget` instalado
- Node.js [Versão Ativa LTS](https://nodejs.org/en/blog/release/) instalado usando um destes
  métodos:
  - Usando `nvm` (recomendado)
    - [Instalando o nvm](https://github.com/nvm-sh/nvm#install--update-script)
    - [Instalar e alterar a versão do Node com o nvm](https://nodejs.org/en/download/package-manager/#nvm)
  - [Download Binário](https://nodejs.org/en/download/)
  - [Gerenciador de Pacotes](https://nodejs.org/en/download/package-manager/)
  - [Usando pacotes NodeSource](https://github.com/nodesource/distributions/blob/master/README.md)
- `yarn` [Instalação](https://classic.yarnpkg.com/en/docs/install)
  - Você precisará usar o Yarn classic para criar um novo projeto, mas ele pode ser [migrado para o Yarn 3](../tutorials/yarn-migration.md)
- `docker` [instalação](https://docs.docker.com/engine/install/)
- `git` [instalação](https://github.com/git-guides/install-git)
- Se o sistema não estiver diretamente acessível pela rede, as seguintes portas
  precisam estar abertas: 3000, 7007. Isso é bastante incomum, a menos que você esteja
  instalando em um contêiner, VM ou sistema remoto.

### Crie seu Aplicativo Backstage

Para instalar o aplicativo autônomo do Backstage, usamos o `npx`, uma ferramenta para executar
executáveis Node diretamente do registro. Essa ferramenta faz parte da sua instalação do Node.js.
Executar o comando abaixo irá instalar o Backstage. O assistente irá
criar um subdiretório dentro do seu diretório de trabalho atual.

```bash
npx @backstage/create-app@latest
```

> Observação: Se isso falhar na etapa `yarn install`, é provável que você precise instalar algumas dependências adicionais que são usadas para configurar o `isolated-vm`. Você pode obter mais informações na seção de [requisitos](https://github.com/laverdet/isolated-vm#requirements) deles e, em seguida, executar `yarn install` manualmente novamente depois de concluir essas etapas.

O assistente irá solicitar o nome do aplicativo, que também será o nome do diretório.

![Captura de tela do assistente solicitando um nome para o aplicativo.](../assets/getting-started/wizard.png)

### Execute o aplicativo Backstage

Quando a instalação estiver concluída, você pode ir para o diretório do aplicativo e
iniciar o aplicativo. O comando `yarn dev` executará o frontend e o backend como
processos separados (nomeados `[0]` e `[1]`) na mesma janela.

```bash
cd my-backstage-app
yarn dev
```

![Captura de tela da saída do comando, com a mensagem de que o webpack foi compilado com sucesso](../assets/getting-started/startup.png)

Pode levar um pouco de tempo, mas assim que a mensagem
`[0] webpack compiled successfully` aparecer, você pode abrir um navegador e acessar diretamente
o seu portal Backstage recém-instalado em `http://localhost:3000`. Você pode começar a explorar a demonstração imediatamente. Observe que o banco de dados em memória será limpo quando você reiniciar o aplicativo, então você provavelmente vai querer
continuar com as etapas do banco de dados.

![Captura de tela do portal Backstage.](../assets/getting-started/portal.png)

Na próxima parte deste tutorial, você aprenderá como mudar para um banco de dados persistente,
configurar autenticação e adicionar sua primeira integração. Continue
com [começando: Configurando o Backstage](configuration.md).

Compartilhe suas experiências, comentários ou sugestões conosco:
[no discord](https://discord.gg/backstage-687207715902193673), registre problemas para qualquer
[recurso](https://github.com/backstage/backstage/issues/new?labels=help+wanted&template=feature_template.md)
ou
[sugestões de plugins](https://github.com/backstage/backstage/issues/new?labels=plugin&template=plugin_template.md&title=%5BPlugin%5D+THE+PLUGIN+NAME),
ou
[bugs](https://github.com/backstage/backstage/issues/new?labels=bug&template=bug_template.md)
que você tenha, e sinta-se à vontade para
[contribuir](https://github.com/backstage/backstage/blob/master/CONTRIBUTING.md)!
