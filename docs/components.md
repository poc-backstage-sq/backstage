# Componentes do Backstage

## Introdução ao Backstage: Componentes, Catálogo, Templates, TechDocs e Plugins

O Backstage é uma plataforma de gerenciamento de software de código aberto que oferece uma visão unificada e centralizada de todos os componentes de um ecossistema de software. Com o Backstage, as equipes podem rastrear a propriedade e os metadados de seus softwares, como serviços, APIs, bibliotecas e pipelines de dados, por meio do Catálogo de Software.

O Catálogo de Software é o coração do Backstage e permite que as equipes gerenciem e mantenham seus softwares dentro de uma visão uniforme. Os arquivos YAML de metadados são essenciais para o Catálogo, pois eles permitem que o Backstage colete e torne visíveis esses metadados. Além disso, o Catálogo torna todos os softwares em uma organização detectáveis, facilitando a descoberta e o uso por outras equipes.

Além do Catálogo, o Backstage oferece recursos adicionais para melhorar a experiência de desenvolvimento e colaboração. Os Software Templates permitem criar novos componentes de forma rápida e padronizada, acelerando o processo de desenvolvimento. O TechDocs é um recurso que permite criar e compartilhar documentação técnica para os componentes, facilitando o entendimento e o uso correto dos mesmos.

Os Plugins são extensões do Backstage que adicionam funcionalidades específicas, como integração com sistemas externos, monitoramento de métricas e muito mais. Com os Plugins, é possível personalizar o Backstage de acordo com as necessidades da equipe e do projeto.

Neste texto, exploraremos em detalhes cada um desses componentes do Backstage e como eles podem melhorar a gestão e a colaboração no desenvolvimento de software.


## Catalogo

Agora que temos uma ideia do que o Backstage é e pode fazer e do que está por baixo do capô, podemos começar a explorar alguns de seus recursos. Nesta postagem, exploraremos o Backstage Catalog.

## O que é o Catálogo?
O Backstage Catalog tem como objetivo ser o hub centralizado para rastrear a propriedade e os metadados de todos os softwares em nosso ecossistema (serviços, APIs, bibliotecas, pipelines de dados, etc.). Os arquivos YAML de metadados estão no centro do Catalog, junto com o código, e permitirão que o Backstage os colete e os torne visíveis. O Catalog permite que as equipes gerenciem e mantenham seus softwares dentro de uma visão uniforme. Além disso, ele tornará todos os softwares em nossas organizações detectáveis.

Se você leu a introdução do Backstage, pode encontrar o Catálogo em http://url_do_backstage:3000/catalog .

## Como adicionar componentes ao catálogo

Conforme mencionado acima, os arquivos YAML de metadados estão no coração do Catálogo e devem ser armazenados no VCS de sua escolha. Podemos adicionar componentes ao Catálogo de várias maneiras diferentes:

- Registrar componentes manualmente - [via plugin Catalog Import](https://github.com/backstage/backstage/tree/master/plugins/catalog-import):

  ![](assets/img/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_ou51wvsyw4z6wssxwors.avif)

- Configuração estática

- Criando novos componentes através do Backstage

- Integração com uma fonte externa - algumas organizações já possuem um sistema existente para monitorar o software e podem ser integradas ao Backstage

## Configuração estática

Vamos "colocar a mão na massa" e adicionar um serviço ao Catálogo por meio de configuração. Para isso, usaremos um repositório criado para a demo do Flux CD e o integraremos no Backstage. Adicionaremos um catalog-info.yamlarquivo ao repositório com uma configuração simples de Componente :

```
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: gitops-flux-helm
  description: Gitops Flux Helm demo
  tags:
    - gitops
spec:
  type: service
  lifecycle: production
  owner: user:guest
```

Movendo nossa atenção para os Backstage, abaixo app-config.yamlatualizaremos duas seções:

```
integrations:
  gitlab:
    - host: gitlab.com
      token: <YOUR-API-TOKEN>

catalog:
  rules:
    - allow: [Component, System, API, Group, User, Template, Location]
  locations:
    # gitops-flux-helm project
    - type: url
      target: https://gitlab.com/mccricardo-blog-demos/gitops-flux-helm/-/blob/master/catalog-info.yaml
```

E pronto, o Backstage agora pode rastrear nosso projeto.


![](assets/img/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_4opk9p4qbsapl5qbdpjc.avif)

![](assets/img/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_7fp5mbqggawoo6oikg8w.avif)

Essa configuração estática atinge um resultado similar ao registro manual de componentes. Mas quando você junta esse tipo de configuração com integrações de descoberta como o Github Discovery a mágica começa a acontecer.

Componentes criados por meio do Backstage são registrados automaticamente no catálogo. Em um post futuro, exploraremos os Backstage Software Templates e veremos mágica de verdade acontecendo.


# Software Templates

 

Se nosso objetivo é tornar nossas vidas e as vidas daqueles que trabalham conosco mais fáceis e felizes, o Backstage Software Templates pode nos dar uma mão. Esta ferramenta pode nos ajudar a criar Componentes dentro do Backstage que carregarão esqueletos de código, preencherão algumas variáveis ​​e publicarão esse template em algum lugar.

##  $$$$ CONTINUAR DAQUI


## Como funciona o TechDocs

O Backstage gera documentação em um processo de três etapas:

- Preparar - buscar os arquivos de origem do código-fonte e passá-los para o gerador
- Gerar - executar a fase de construção (por meio de um contêiner ou mkdocscli)
- Publicar - carregue os arquivos gerados para o armazenamento


![](assets/img/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_5pm4fty1hf5f2j2e3sip.avif)

> fonte da imagem: documentação oficial do Backstage

Conforme mostrado acima, para uma implantação de produção, esse processo deve ser desacoplado da implantação do Backstage. O Backstage seria onlyresponsável por buscar e apresentar a documentação gerada.


### Modelo de documentação

A documentação pode ser criada como um projeto independente e nossa configuração local já tem um modelo com o qual podemos brincar.

![](assets/img/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_oskvew6taukxxstfa4bv.avif)

## Adicione TechDocs à nossa configuração


Para nossa demonstração, usaremos uma configuração mais simples (como mostrado abaixo) e deixaremos o Backstage fazer todo o trabalho pesado. Para tornar as coisas ainda mais fáceis, precisaremos do Docker.

![](assets/img/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_212guvxag1j1fz39sesr.avif)

Nós bem reutilizaremos nosso repositório do post do Flux CD . Adicionaremos alguns arquivos e editaremos outro. Começaremos criando um mkdocs.yamlarquivo.

```
site_name: 'gitops-flux-helm-docs'

nav:
  - Home: index.md

plugins:
  - techdocs-core
```

Em seguida, criamos um index.mddentro de uma docspasta.


```
# Example

This is a basic example of documentation.
```

Agora editaremos **catalog-info.yaml** e adicionaremos a seguinte anotação em **metadata**.

```
metadata:
  annotations:
    backstage.io/techdocs-ref: url:https://gitlab.com/mccricardo-blog-demos/gitops-flux-helm
```

Do lado do repositório, terminamos. Adicionamos a configuração necessária que permitirá ao Backstage encontrar **docs/index.md** e gerar a documentação. Agora voltamos nossa atenção para o Backstage. Em app-config.yamladicionaremos o seguinte código.

```
techdocs:
  builder: 'local'
  generators:
    techdocs: 'docker'
  publisher:
    type: 'local'
```

E é isso. Vamos deixar o Backstage fazer todo o trabalho pesado (com um pouco de ajuda do Docker).

![](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F2se4qayfoibqtyb4l46q.png)

![](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F9d7m434aq0xwk4kq4hg0.png)


