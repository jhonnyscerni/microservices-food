curso: Microservices: Clean Architecture, DDD, SAGA, Outbox & Kafka

Os Microserviços foram baseados em uma arquitetura hexagonal e conceitos de arquitetura Limpa.
![Estrutura](imgs/clean-arch-project.png)

A interface de camadas de dados (Data Layer Interface) é a definição de porta de saida que vai precisar ser implementada por um adaptador.(Data Layer Adapter)
Esta interface incluirá todos os métodos relacionados à fonte de dados que a camada de domínio requer para completar o logíca de negócios.
Verificando a imagem a camada de dados depende da camada de negócios.

Vantagens:
- A camada de negócios pode ser desenvolvida e testada sem exigir uma implementação real da camada de dados. (Develop and Test Independently "Desenvolva e teste independentemente")
- quando implementamos a camada de dados, podemos alterar essa implementação sem afetar a
camada de negócios, desde que obedeçamos aos contratos na interface da camada de dados, que está na camada de negócios. (Easily replacable "facil substituir")

Para usar a interface durante a codificação na camada de coominio é preciso usar <b>injeção de dependencia (DI)</b>

Outra questão é para usarmos a camada de negócio e a camada de dados juntas é preciso usar um outro componente.
Exemplo na imagem "CONTAINER" ele vai possuir a classe principal do Spring boot. Nesse componente vai ter uma dependencia de negocio, camada de dados e também API.
Os registros do Bean serão no modulo container
As portas de saída e o implementador dessas portas como adaptadores secundários.
Em uma arquitetura limpa

----
API - Injeta a interface do dominio</br>
A porta de entrada pode ser chamada como porta da camada de domínio.(Verde-domain "Domain Layer Interface")
Esta porta também é uma interface, quando pensamos em Java, e exigirá um implementador, e
ser implementado na própria camada de domínio. ("Domain Layer Adapter")
A camada da API injetará e usará a porta de entrada usando injeção de dependência.</b>Interface do domain da porta de entrada</b>
As chamadas de método serão delegadas à implementação, que também vão está na camada de domínio. Em termos de arquitetura hexagonal, a camada de API que usa a porta de entrada é um adaptador primário.
É um adaptador porque pode ser facilmente alterado sem tocar na camada de domínio. Graças ao uso da porta de entrada.
O adaptador da camada de dados na camada de dados era um adaptador secundário na arquitetura limpa


<b>Serviço Externo</br></b>
Digamos que você precise ter uma chamada de serviço externa na camada de negócios.
Então eu tenho um serviço externo aqui. De acordo com o princípio de arquitetura limpa que acabei de aplicar
Como podemos integrar esse serviço externo no aplicativo?
Novamente.
Vou adicionar uma porta na camada de negócios.
Desta vez vou chamá-lo como porta de serviço externa.(External Service Port)
E então implemente no serviço externo com um adaptador, que é um adaptador secundário em hexagonal
termos de arquitetura. (External Service Adapter " Secondary")


<b>Broker Messagens</br></b>
Em seguida, podemos adicionar um componente de mensagens.
Novamente aplicando o princípio de inversão de dependência Vou adicionar uma interface de porta de mensagens na camada de negócios. (Messaging Port)
E implemente-o com um adaptador de mensagens no componente de mensagens. (Messaging Adapter)
Digamos que este adaptador usará Kafka.
No entanto, como mencionado, quando você deseja alterar o adaptador com qualquer outra tecnologia de mensagens
Será uma operação indolor graças à arquitetura limpa que você usa.
Assim, no futuro, você pode facilmente alterar o barramento de mensagens do Kafka para qualquer outra solução.

------
No final, a lógica central será o componente mais independente e estável desta aplicação.
E permitirá desenvolver e testar a lógica de negócios de forma independente.
Domain - (Independent & Stable)


Visualize dependencies:
https://github.com/ferstl/depgraph-maven-plugin
mvn com.github.ferstl:depgraph-maven-plugin:aggregate -DcreateImage=true -DreduceEdges=false -Dscope=compile "-Dincludes=com.food.ordering.system*:*"
![Estrutura](imgs/estrutura.png)




