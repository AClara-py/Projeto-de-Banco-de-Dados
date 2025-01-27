# Projeto-de-Banco-de-Dados
Após o script de criação do Banco de Dados Ecoomerce, chegou o momento de inserirmos os dados e recuperá-los respondendo perguntas através de queries. Segue o script realizado:
OBS: Durante o código é possivel verificar que alguns ontoso estão comentados para melhor entendimento.

[Uploading populando_ecommerce.sql…]()-- inserção de dados e queries
use ecommerce;

show tables;
desc clientes;
 -- idCliente, Fname, Minit, Lname, CPF, Address
 insert into clientes (Fname, Minit, Lname, CPF, Address)
			values ('Maria', 'M', 'Silva', 12346789, 'Rua Prata 29, Carangola - Cidade das flores'),
				   ('Matheus', 'O' , 'Pimentel', 987654321, 'Rua Alemeda 289, Centro - Cidade das flores'),
                   ('Ricardo', 'F', 'Silva', 45678913, 'Avenida Vinha 1009, Centro - Cidade das flores'),
                   ('Julia', 'S' , 'França', 789123456, 'Rua Laranjeiras 861, Centro - Cidade das flores'),
                   ('Roberta', 'G', 'Assis', 98745631, 'Avenida de Koller 19, Centro - Cidade das flores'),
                   ('Isabela', 'M', 'Cruz', 654789123, 'Rua Flores 28, Centro - Cidade das flores');
				
select * from clientes;
-- idProduto, Pname, classificação_kids boolean, Categoria, avaliação, size
insert into produtos (Pname, classificação_kids, Categoria, avaliação, size)
			values ('Fone de ouvido', false, 'Eletrônico', '4', null),
				   ('Barbie Elsa', true, 'Brinquedos', '3', null),
                   ('Body Carters', true, 'Vestimenta', '5', null),
                   ('Microfone Vedo - Youtuber', false, 'Eletrônico', '4', null),
                   ('Sofá retrátil', false, 'Moveis', '3', '3x57x80'),
                   ('Farinha de arroz', false, 'Alimentos', '2', null),
                   ('Fire Stick Amazon', false, 'Eletrônico', '3', null);
                   
select * from produtos;

-- idPedido, idPedidoCliente , status_do_pedido enum('Em andamento', 'Processando', 'Enviado', 'Entregue',descrição_pedido ,frete,paymentCash
insert into pedidos (idPedidoCliente , status_do_pedido, descrição_pedido, frete, paymentCash ) 
				values (1, default, 'Compra via aplicativo', null, 1),
					   (2, default, 'Compra via aplicativo', 50, 1),
                       (3, 'Em andamento', null, null, 1),
                       (4, default, 'Compra via web site', 150, 1);

select * from estoque;

-- idPPproduto, idPPpedido, ppQuantidade, ppStatus
insert into produtoPedido(idPPproduto, idPPpedido, ppQuantidade, ppStatus)
				values (1,13,2, default),
					   (2,16,1, default),
                       (3,19,1, default);
                       
-- estoqueLocation, quantidade
insert into estoque (estoqueLocation, quantidade)
				values  ('Rio de Janeiro', 1000),
						('Rio de Janeiro', 500),
						('São Paulo', 10),
						('São Paulo', 100),
						('São Paulo', 10),
						('Brasília', 60);

-- idPEproduto, idPEestoque, location
insert into produtoEstoque(idPEproduto, idPEestoque, location)
				values (1,2,'RJ'),
					   (2,6,'GO');
                       
-- RazaoSocial varchar(225,CNPJ char(15) , contato
insert into fornecedor (RazaoSocial, CNPJ, contato)
				values ('Almeida e filhos', 12345678000156, '21985474'),
					   ('Eletrônicos Silva', 85451964000157, '21985484'),
                       ('Eletrônicos Valma', 93456789000195, '21975474');

select * from produtoFornecedor;
--  idPFproduto, idPFfornecedor, quantidade
insert into produtoFornecedor (idPFproduto, idPFfornecedor, quantidade)
				values (1,1,500),
					   (1,2,400),
                       (2,1,633),
                       (3,3,5),
                       (2,2,10);

--  RazaoSocial, NomeFantasia, CNPJ, CPF, Vlocation, contato
insert into vendedor (RazaoSocial, NomeFantasia, CNPJ, CPF, Vlocation, contato)
				values ('Tech eletronics', null, 87654321000123, null, 'Rio de Janeiro', 219946287),
					   ('Botique Durgas', null, null , 12345678926, 'Rio de Janeiro', 219567895),
                       ('Kids World', null, 65857234000131, null, 'São Paulo', 1198657484);
                       
-- idPvendedor, idPproduto, QtProduto
insert into produtoVendedor (idPvendedor, idPproduto, QtProduto)
				values (1,6,80),
					   (2,7,10);

select * from produtoVendedor;

-- recuperar o nº de clientes
select count(*) from clientes;

select * from clientes c, pedidos p where c.idcliente = p.idPedidoCliente;

select Fname, Lname, idpedido, status_do_pedido from clientes c, pedidos p where c.idcliente = p.idPedidoCliente;
select concat(Fname,' ', Lname) as CompleteName, idpedido as pedido, status_do_pedido from clientes c, pedidos p where c.idcliente = p.idPedidoCliente;

-- traz inclusive os clientes que não fizeram pedidos
select * from clientes left outer join pedidos on idcliente = idPedidoCliente;

select * from clientes c inner join pedidos p ON c.idCliente = p.idPedidoCliente
					inner join produtoPedido pp on pp.idPPpedido = p.idPedido;
