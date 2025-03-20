---
title: "Item 87: Pense na possibilidade de usar uma forma serializada customizada"
date: 2025-02-06
---

**1\. Problema da forma serializada padrão**

- A forma serializada padrão pode tornar impossível substituir uma implementação descartável.
- Muitas classes da biblioteca Java, como BigInteger, sofreram com essa limitação.
- Deve-se avaliar se a forma serializada padrão é apropriada em termos de flexibilidade, desempenho e correção.

**2\. Quando a forma serializada padrão é aceitável?**

- Quando a representação física do objeto é idêntica ao seu conteúdo lógico.
- Exemplo: uma classe Name com três String (sobrenome, primeiro nome e nome do meio).

**3\. Problemas ao usar a forma serializada padrão quando a representação física difere do conteúdo lógico**

- Acopla a API pública à implementação interna, impedindo mudanças futuras.
- Aumenta o consumo de espaço, armazenando detalhes desnecessários.
- Pode ser ineficiente em tempo de execução, devido a percursos desnecessários no grafo do objeto.
- Pode causar overflow de pilha, especialmente em listas encadeadas grandes.

**4\. Exemplo: Serialização de uma lista encadeada (StringList)**

- A forma serializada padrão armazenaria todos os nós e links internos, o que é ineficiente.
- Solução: armazenar apenas o número de elementos e suas String associadas.
- Isso reduz o espaço ocupado e melhora a performance da serialização.

**5\. Implementação de uma forma serializada customizada**

- Criar os métodos **writeObject** e **readObject** para controlar a serialização.
- Utilizar o modificador transient para evitar serializar detalhes desnecessários.
- Sempre chamar defaultWriteObject e defaultReadObject para garantir compatibilidade futura.
- Documentar a API serializada com @serial (campos) e @serialData (métodos).

**6\. Problema ainda maior com tabelas hash**

- A posição dos elementos depende do código hash da chave, que pode mudar entre execuções.
- Serializar a estrutura física de uma tabela hash poderia corromper suas invariantes.
- Solução: armazenar apenas os pares chave-valor e recriar a estrutura na desserialização.

**7\. Regras gerais para serialização eficiente**

- Declarar como transient qualquer campo que não faça parte do estado lógico do objeto.
- Evitar serializar campos derivados, cujo valor pode ser recalculado.
- Evitar serializar ponteiros para estruturas nativas da JVM.

**Conclusão**

- A forma serializada padrão deve ser aceita apenas se for adequada ao estado lógico do objeto.
- Para maior controle, use uma forma serializada customizada, garantindo eficiência e flexibilidade.
- Isso evita problemas futuros com manutenção, desempenho e compatibilidade da API serializada.

Exemplos do livro:

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fqaybw7eafrhnwp7fmz9r.jpg)  
.

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg39zv3h2vughiay5j6fe.jpg)

.

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzlqg0zaxq2q4yvcv5slk.jpg)

Go to Source
