# Projeto-de-Banco-de-Dados
Este repositório é para envio de projetos de BD realizados durante o bootcamp ofertado pela DIO.
Primeiro Projeto: Projeto Conceitual para um E-commerce, onde conterá algumas entidade como: Cliente, produto, pedido, estoque e fornecedor.
No anexo coloco o projeot inicial criado com a ajuda da professora. E meu desafio é refinar o projeot conceitual e responder, com a modelagem, algumas perguntas:
 1 - Cliente PJ e PF – Uma conta pode ser PJ ou PF, mas não pode ter as duas informações;
 2 - Pagamento – Pode ter cadastrado mais de uma forma de pagamento;
 3 - Entrega – Possui status e código de rastreio;
![ECOMMERCE](https://github.com/user-attachments/assets/6ba7e5d4-4115-40dd-b30d-d710a32f87af)

Após criar meu modelo conceitual, realizamos a criação do Banco de Dados através do Mysql e a inserção de dados, como também realizamos algumas queries sobre o contexto de ecommerce respondendo a algumas perguntas de negócio.


[Uploading ecommercecreatedb.sql…](-- Criação do Banco de Dados para o cenário de E-commerce
-- drop database ecommerce;
create database ecommerce;
use ecommerce;

-- criar tabela cliente - CLIENTS
create table clientes(
	idCliente INT auto_increment primary key,
    Fname varchar(10),
    Minit char(3),
    Lname varchar(20),
    CPF char(11) not null,
    Address varchar(50),
    constraint unique_cpf_cliente unique (CPF)
);

alter table clientes auto_increment=1;

-- criar tabela produto - PRODUCT
create table produtos(
	idProduto INT auto_increment primary key,
	Pname varchar(30) not null,
    classificação_kids bool default false,
    Categoria enum('Eletrônico', 'Vestimenta','Brinquedos', 'Alimentos', 'Moveis') not null,
	avaliação float default 0,
    size varchar(10)
    );

-- size = dimensão do produto

-- criar tabela de pagamentos
-- para ser continuado no desafio: termine de implementar a tabela e crie a conexão com as tabelas necessárias
-- além disso, reflita essa modificação no diagrama de esquema relacional
create table pagamentos(
	idCliente int,
    idPagamento int, -- foreign key de pedidos
    tipoPagamento enum('Boleto', 'Cartão', 'Dois cartões', 'PIX'),
    limiteDisponível float,
    primary key (idCliente, idPagamento)
);

-- criar tabela pedido - ORDER
create table pedidos(
	idPedido INT auto_increment primary key,
    idPedidoCliente int,
    status_do_pedido enum('Em andamento', 'Processando', 'Enviado', 'Entregue') default 'Processando',
    descrição_pedido varchar(255),
    frete float default 10,
    paymentCash boolean default false, -- pagamento em boleto
    constraint fk_pedido_cliente foreign key (idPedidoCliente) references clientes(idCliente)
		on update cascade
);
desc pedidos;

-- criar tabela estoque - STORAGE
create table estoque(
	idEstoque int auto_increment primary key,
    estoqueLocation varchar(225),
    quantidade int default 0
);

-- criar tabela fornecedor - SUPPLIER
create table fornecedor(
	idFornecedor int auto_increment primary key,
    RazaoSocial varchar(225) not null,
    CNPJ char(15) not null,
    contato char(11) not null,
    constraint unique_fornecedor unique (CNPJ)
);

desc fornecedor;

-- criar tabela vendedor - SELLER
create table vendedor(
	idVendedor int auto_increment primary key,
    RazaoSocial varchar(255) not null,
    NomeFantasia varchar(255),
    CNPJ char(15),
    CPF char(11),
    Vlocation varchar(255),
    contato char(11) not null,
    constraint unique_pj_vendedor unique (CNPJ),
    constraint unique_pf_vendedor unique (CPF)
);

-- criar tabela Produto_vendedor (terceiro) - PRODUCTSELLER
create table produtoVendedor(
	idPvendedor int,
    idPproduto int,
    QtProduto int default 1,
    primary key (idPvendedor, idPproduto),
    constraint fk_produto_vendedor foreign key (idPvendedor) references vendedor(idVendedor),
    constraint fk_produto_produto foreign key (idPproduto) references produtos(idProduto)
);

desc produtoVendedor;

-- criar tabela Produto_pedido - PRODUCTORDER
create table produtoPedido(
	idPPproduto int,
    idPPpedido int,
    ppQuantidade int default 1,
    ppStatus enum('Disponivel', 'Sem estoque') default 'Disponivel',
    primary key (idPPproduto, idPPpedido),
    constraint fk_produto_produtos foreign key (idPPproduto) references produtos(idProduto),
    constraint fk_produto_pedidos foreign key (idPPpedido) references pedidos(idPedido)
);

-- criar tabela produto em estoque - STORAGELOCATION
create table produtoEstoque(
	idPEproduto int,
    idPEestoque int,
    location varchar(255) not null, 
    primary key (idPEproduto, idPEestoque),
    constraint fk_produto_estoque foreign key (idPEestoque) references estoque(idEstoque),
    constraint fk_produtoestoque_produtos foreign key (idPEproduto) references produtos(idProduto)
);

-- criar a tabela ProdutoFornecedor - PRODUCTSUPPLIER
create table produtoFornecedor(
	idPFproduto int,
    idPFfornecedor int,
    quantidade int not null,
    primary key (idPFproduto, idPFfornecedor),
    constraint fk_produto_fornecedor_produto foreign key (idPFproduto) references produtos(idProduto),
    constraint fk_produto_fornecedor_fornecedor foreign key (idPFfornecedor) references fornecedor(idFornecedor)
);
desc produtoFornecedor;

show tables; -- mostra todas as tabelas

-- select * from referential_constraints where constraint_schema = 'ecommerce'; -- recuperar todas as contrainst no BD
