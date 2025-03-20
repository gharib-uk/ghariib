---
title: "Guia para certificação Vue Mid-Level"
date: 2025-01-03
categories: 
  - "general"
---

Inspirado neste post, resolvi também escrever um texto do caminho que fiz para conseguir a certificação em Vue.

Trabalho com Vue há 3 anos, e no momento estamos fazendo a transição da aplicação de Vue2 para Vue3, então estudar para a certificação me pareceu um bom caminho para atualizar os conhecimentos no framework.

Se você está começando no mundo do desenvolvimento web, não recomendo que faça a certificação, pois a mesma está custando atualmente quase R$600,00 e e acredito que você consegue extrair mais valor fazendo cursos e outras atividades com este dinheiro. Mas se você já é um profissional experiente e quer colocar seus conhecimentos a prova, exibir o badge de dev Vue e ajudar o desenvolvimento do framework (parte do dinheiro vai para a equipe do Vue), recomendo que o faça.

## Estrutura da prova

A prova tem duração máxima de 135 minutos, sendo 30 questões fechadas e 2 desafios de código.

## Questões fechadas

Você deve acertar 25 questões fechadas (algo em torno de 85% de acerto), cada questão tem 4 opções de resposta e sempre apenas uma opção é verdadeira. São questões relativamente fáceis, em sua maioria perguntando o que um bloco de código irá fazer e sobre momentos em que hooks (lifecycles e watchers) são disparados. Todas as questões levam em consideração a Composition API. O tempo é mais do que suficiente para responder e revisar todas as questões.

## Questões abertas

Após a parte de questões fechadas, se inicia a parte dos desafios de código. São dois desafios, um você deve terminar um código com as instruções que são fornecidas e outro é uma correção de bug. Ambos os desafios são realizados na plataforma da Stackblitz, então é recomendável você fazer alguns códigos lá para se acostumar com a ferramenta, mas ela se parece muito com o vscode, então você não deverá ter problemas.

Nos boilerplates dos desafios é usado JS e Composition API, mas você pode usar TS e Options API caso queira. Também nesta parte você tem acesso a uma documentação resumida do Vue através de um modal, a navegação é um pouco ruim, mas ajuda bastante em momentos de dúvida e você pode abrir o devtools do navegador. Recomendo já deixar o devtools configurado para abrir como um fram no navegador e não como nova janela, para evitar algum problema com o software de monitoramento.

No meu caso o desafio de código foi completar uma tela de login, colocando comunicação entre os componentes (emtis, props, template syntax, event handling, form input bindings) e também fazer a estilização dos mesmos. Neste desafio não é necessário o uso de Transitions, mas já vi na internet que outros desafios utilizam bastante, então é bom estudar.

O desafio de resolver um bug foi bastante simples, bastou alterar uma referência de reactive() para ref().

Você não tem acesso se as suas alterações nos códigos estão aprovadas pelos testes automatizados, então você deve submeter a sua solução confiando que o que você está vendo na tela (você tem acesso a aplicação rodando, nao apenas ao código).

## O que estudar e onde estudar

1 - Ao comprar a prova você tem acesso a uma lista de links para a documentação, que são os tópicos que serão cobrados na prova, conforme imagem abaixo. Assim você economiza de ler tópicos que não serão levados em consideração.  
![Lista Topicos Certificacao Vue](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyv7wyppeu4oj0nds0s9e.png)

2 - Ver a seguinte playlist no Yout Tube - VueJS Developer Certification - e acompanhar os códigos que o apresentador faz, de preferência refazendo em sua máquina. Ele basicamente passa pelos tópicos citados no guia da certificação e vai desenvolvendo pequenos snippets que ajudam a fixar o conteúdo.

3 - Faça você mesmo os seus snippets, para fixar os conceitos. Aqui disponilizo um repositório que demonstra como fui acompanhando os estudos e fazendo experimentos para fixar o conteúdo. Mas recomendo você começar o seu do absoluto zero.

## Considerações finais

- Você pode comprar o curso oficial para a certificação, mas pelo preço e dificuldade da prova, entendo ser desperdício, os materiais citados aqui são mais que suficientes.
    
- Para quem já fez provas de certificação, o processo de tirar foto do documento e mostrar a sala que está sendo feita a prova é bem facilitado, muito mais simples que da AWS e da LPI por exemplo. Caso você nunca tenha feito uma prova de certificação antes, procure vídeos na internet (ou converse com um amigo) que comentam sobre como ambiente (tanto virtual quanto real) deve estar preparado para que a prova possa ser realizada e não cause sua desclassificação.
    
- Recomendo fazer a prova e um computador com no mínimo 16GB de RAM e um bom processador. Caso você tenha, assim como eu, problemas técnicos ao realizar a prova, tem um botão na página para sinalizar o problema, você deve sinalizar o problema e sair da prova, após isto mandar um e-mail para que lhe seja garantido um novo voucher. No meu caso foram solicitos e dispibilizaram um novo voucher com menos de uma semana.
    

Go to Source
