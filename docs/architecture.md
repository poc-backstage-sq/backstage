---
id: Arquitetura-backstage
title: Arquitetura
description: Arquitetura backstage
---
![software-catalog](../assets/header.png)

[Backstage](https://backstage.io/) é uma plataforma aberta para construir portais de desenvolvedores. Alimentado por um catálogo de software centralizado, o Backstage restaura a ordem em seus microsserviços e infraestrutura e permite que suas equipes de produtos entreguem código de alta qualidade rapidamente, sem comprometer a autonomia.

O Backstage unifica todas as suas ferramentas de infraestrutura, serviços e documentação para criar um ambiente de desenvolvimento simplificado de ponta a ponta.

<iframe width="672" height="378" src="https://www.youtube.com/embed/85TQEpNCaU0" title="Player de vídeo do YouTube" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

O Backstage inclui, de forma nativa:

- [Catálogo de Software do Backstage](../features/software-catalog/index.md) para gerenciar todo o seu software (microsserviços, bibliotecas, pipelines de dados, sites, modelos de ML, etc.)

- [Modelos de Software do Backstage](../features/software-templates/index.md) para criar rapidamente novos projetos e padronizar suas ferramentas com as melhores práticas da sua organização

- [TechDocs do Backstage](../features/techdocs/README.md) para facilitar a criação, manutenção, busca e uso de documentação técnica, utilizando uma abordagem de "docs like code"

- Além disso, um ecossistema em crescimento de [plugins de código aberto](https://github.com/backstage/backstage/tree/master/plugins) que expandem ainda mais a customização e funcionalidade do Backstage

## Backstage e a CNCF

O Backstage é um projeto de incubação da CNCF após se formar no Sandbox. Leia o anúncio [aqui](https://backstage.io/blog/2022/03/16/backstage-turns-two#out-of-the-sandbox-and-into-incubation).

<img src="https://backstage.io/img/cncf-white.svg" width="400" />

## Benefícios

- Para _gerentes de engenharia_, permite que você mantenha padrões e melhores práticas em toda a organização, e pode ajudá-lo a gerenciar todo o seu ecossistema tecnológico, desde migrações até certificação de testes.

- Para _usuários finais_ (desenvolvedores), torna rápido e simples construir componentes de software de maneira padronizada, e fornece um local central para gerenciar todos os projetos e documentação.

- Para _engenheiros de plataforma_, permite extensibilidade e escalabilidade, permitindo que você integre facilmente novas ferramentas e serviços (por meio de plugins), além de estender a funcionalidade das existentes.

- Para _todos_, é uma experiência única e consistente que conecta todas as suas ferramentas de infraestrutura, recursos, padrões, proprietários, colaboradores e administradores em um só lugar.

Se você tiver dúvidas ou precisar de suporte, junte-se ao nosso [chat no Discord](https://discord.gg/backstage-687207715902193673).


# Arquitetura backstage

Na introdução do Backstage, falamos sobre os tipos de problemas que o Backstage pretende resolver e tivemos nossa primeira experiência de como iniciá-lo e brincar com ele. Embora agora estejamos prontos para começar a sujar as mãos, vamos dedicar um momento para entender as peças que fazem do Backstage o ótimo software que ele é. Isso nos dará uma ideia melhor do que está por baixo do capô e como tudo se encaixa.


## Como é a aparência

A configuração que fizemos na introdução do Backstage esconde um pouco da complexidade para que pudéssemos começar a funcionar rapidamente. Quando implantado com o objetivo de ser usado por nossos usuários-alvo, o Backstage será composto de:

- A interface do usuário principal
- Os plugins da IU e seus serviços de suporte
- bases de dados

A imagem abaixo mostra um exemplo de uma implantação do Backstage com alguns plugins.

![](assets/img/https___dev-to-uploads.s3.amazonaws.com_uploads_articles_a8s33rzlnzrf2y3hn5vv.avif)

 (fonte da imagem: documentação oficial do Backstage)


A UI retratada acima é um wrapper do lado do cliente em torno de um conjunto de plugins. Ela é responsável por fornecer alguns componentes e bibliotecas de UI principais para atividades compartilhadas.

Os plugins podem ser de três tipos:

- Autônomo - executado inteiramente no navegador
- Com suporte de serviço - faça solicitações de API para um serviço dentro da instalação do Backstage
- Com suporte de terceiros - faça solicitações de API para um serviço de terceiros (por meio de um proxy)

Cada plugin se tornará disponível na UI em uma URL dedicada, já que são aplicativos do lado do cliente. Os plugins são escritos em TypeScript ou JavaScript e vivem em seu próprio diretório em backstage/plugins . Para mais informações sobre como construir e integrar plugins, confira Introdução aos plugins.

O Backstage usa a biblioteca Knex que suporta uma infinidade de bancos de dados. Apesar disso, o Backstage foi testado para funcionar com SQLite (para fins de teste) e PostgreSQL (para cargas de trabalho de produção).

O Backstage também pode ser conteinerizado e implantado no Kubernetes (você pode até usar o Helm ).

Esta foi uma rápida visão geral de como o Backstage se parece por baixo do capô. A seguir, veremos algumas das peças que tornam o Backstage (ainda mais) valioso.

