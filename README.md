# Projeto-de-Banco-de-Dados
Este repositório é para envio de projetos de BD realizados durante o bootcamp ofertado pela DIO.
Primeiro Projeto: Projeto Conceitual para um E-commerce, onde conterá algumas entidade como: Cliente, produto, pedido, estoque e fornecedor.
No anexo coloco o projeot inicial criado com a ajuda da professora. E meu desafio é refinar o projeot conceitual e responder, com a modelagem, algumas perguntas:
 1 - Cliente PJ e PF – Uma conta pode ser PJ ou PF, mas não pode ter as duas informações;
 2 - Pagamento – Pode ter cadastrado mais de uma forma de pagamento;
 3 - Entrega – Possui status e código de rastreio;
![ECOMMERCE](https://github.com/user-attachments/assets/6ba7e5d4-4115-40dd-b30d-d710a32f87af)

Remodulei meu projeto de Banco de Dados - E-commerce.
Para a primeira pergunta levantada assim, acabei optando por fazer uma especialização da minha entidade CLIENTE e gerei duas outras tabelas: Cliente_PF e Cliente_PJ.
Criei uma nova entidade para PAGAMENTO e acabei relacionando esta diretamente a entidade CLIENTE. Em relação ao terceiro ponto, criei uma entidade ENTREGA e relacionei com PEDIDO em um relacionamento (n:m), onde gerou outra entidade (PEDIDO É ENTREGUE) que vai armazenar os dados: Idpedido e idcliente da tabela PEDIDO, identrega da tabela ENTREGA.
[e-commerce_refinado.pdf](https://github.com/user-attachments/files/18380782/e-commerce_refinado.pdf)
