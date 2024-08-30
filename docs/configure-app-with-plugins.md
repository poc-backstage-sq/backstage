---
id: configurar-aplicativo-com-plugins
title: Configurando o aplicativo com plugins
description: Documentação sobre como configurar o aplicativo com plugins
---

Os plugins do Backstage personalizam o aplicativo de acordo com suas necessidades. Existe um
[diretório de plugins](https://backstage.io/plugins) com plugins para muitas necessidades comuns de infraestrutura - CI/CD, monitoramento, auditoria e muito mais.

## Adicionando plugins existentes ao seu aplicativo

Os seguintes passos pressupõem que você tenha
[criado um aplicativo Backstage](./create-an-app.md) e deseje adicionar um plugin existente a ele.

Estamos usando o
[CircleCI](https://github.com/CircleCI-Public/backstage-plugin/tree/main/plugins/circleci)
plugin neste exemplo, que é projetado para mostrar informações de pipeline de CI/CD anexadas
a uma entidade no catálogo de software.

1. Adicione o pacote npm do plugin ao repositório:

  ```bash
  # Do diretório raiz do Backstage
  yarn add --cwd packages/app @circleci/backstage-plugin
  ```

  Observe que o plugin é adicionado ao pacote `app`, em vez do
  `package.json` raiz. Os aplicativos Backstage são configurados como monorepos com
  [Yarn workspaces](https://classic.yarnpkg.com/en/docs/workspaces/). Como
  o CircleCI é um plugin de interface do usuário frontend, ele vai para `app` em vez de `backend`.

2. Adicione a extensão `EntityCircleCIContent` às páginas de entidade no aplicativo:

  ```tsx title="packages/app/src/components/catalog/EntityPage.tsx"
  /* highlight-add-start */
  import {
    EntityCircleCIContent,
    isCircleCIAvailable,
  } from '@circleci/backstage-plugin';
  /* highlight-add-end */

  const conteudoCICD = (
    <EntitySwitch>
     {/* ... */}
     {/* highlight-add-next-line */}
     <EntitySwitch.Case if={isCircleCIAvailable}>
      <EntityCircleCIContent />
     </EntitySwitch.Case>
     ;{/* highlight-add-end */}
    </EntitySwitch>
  );
  ```

  Este é apenas um exemplo, mas cada instância do Backstage pode integrar conteúdo ou
  cartões para atender às suas necessidades em diferentes páginas, abas, etc. Além disso, enquanto alguns
  plugins, como este exemplo, são projetados para anotar ou suportar entidades específicas do catálogo de software, outros podem ser destinados a serem usados de forma independente e
  seriam adicionados fora da `EntityPage`, como serem adicionados à navegação principal.

3. _[Opcional]_ Adicione uma configuração de proxy:

  Plugins que coletam dados de serviços externos podem exigir o uso de um serviço de proxy.
  Este plugin acessa a API REST do CircleCI e, portanto, requer uma definição de proxy.

  ```yaml title="app-config.yaml"
  proxy:
    '/circleci/api':
     target: https://circleci.com/api/v1.1
     headers:
      Circle-Token: ${CIRCLECI_AUTH_TOKEN}
  ```

### Adicionando uma página de plugin à barra lateral

Em um aplicativo Backstage padrão criado com
[@backstage/create-app](./create-an-app.md), a barra lateral é gerenciada dentro de
`packages/app/src/components/Root/Root.tsx`. O arquivo exporta todo o
elemento `Sidebar` do seu aplicativo, que você pode estender com entradas adicionais, adicionando novos elementos `SidebarItem`.

Por exemplo, se você instalar o plugin `api-docs`, um `SidebarItem` correspondente
pode ser algo assim:

```tsx title="packages/app/src/components/Root/Root.tsx"
// Importe o ícone do Material UI
import ExtensionIcon from '@material-ui/icons/Extension';

// ... dentro do componente AppSidebar
<SidebarItem icon={ExtensionIcon} to="api-docs" text="APIs" />;
```

Você também pode usar seus próprios SVGs diretamente como componentes de ícone. Apenas certifique-se de que eles
estejam dimensionados de acordo com o padrão do
[SvgIcon](https://material-ui.com/api/svg-icon/) do Material UI de 24x24px e defina a
extensão como `.icon.svg`. Por exemplo:

```tsx
import InternalToolIcon from './internal-tool.icon.svg';
```

Em dispositivos móveis, a `Sidebar` é exibida na parte inferior da tela. Para
personalizar a experiência, você pode agrupar `SidebarItems` em um `SidebarGroup`
(Exemplo 1) ou criar um `SidebarGroup` com um link (Exemplo 2). Todos os
`SidebarGroup`s são exibidos na navegação inferior com um ícone.

```tsx
// Exemplo 1
<SidebarGroup icon={<MenuIcon />} label="Menu">
  ...
  <SidebarItem icon={ExtensionIcon} to="api-docs" text="APIs" />
  ...
<SidebarGroup />
```

```tsx
// Exemplo 2
<SidebarGroup label="Search" icon={<SearchIcon />} to="/search">
  ...
  <SidebarItem icon={ExtensionIcon} to="api-docs" text="APIs" />
  ...
<SidebarGroup />
```

Se nenhum `SidebarGroup` for fornecido, um menu padrão exibirá o conteúdo da `Sidebar`.
