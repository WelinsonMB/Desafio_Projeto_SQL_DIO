
# 📦 Banco de Dados - E-commerce

Este projeto representa um banco de dados relacional para uma aplicação de e-commerce. O modelo contempla clientes (pessoa física e jurídica), pedidos, pagamentos, produtos, fornecedores, vendedores terceiros, estoques e entregas.

## 🗃️ Estrutura do Banco de Dados

O banco de dados é chamado `ecommerce` e inclui as seguintes tabelas:

### 1. **cliente**
Armazena informações básicas do cliente.
- `idClient`: Identificador único.
- `Nome`: Nome do cliente.
- `endereco`: Endereço completo.
- `cadastro`: Tipo de cliente (`Pessoa Fisica` ou `Pessoa Juridica`).

### 2. **PessoaFisica** e **PessoaJuridica**
Detalham os dados de acordo com o tipo de cliente.
- Pessoa Física: CPF.
- Pessoa Jurídica: CNPJ, Razão Social e Nome Fantasia.

### 3. **pagamento**
Informações de pagamento do cliente.
- `NumCard`: Número do cartão.
- `Validade`: Data de validade.
- Relacionado com a tabela `cliente`.

### 4. **pedido**
Contém os pedidos realizados.
- `StatusPedido`: `Processando` ou `Finalizado`.
- `descPedido`: Descrição do pedido.
- FK para `cliente` e `pagamento`.

### 5. **entrega**
Status de entrega de um pedido.
- `StatusEntrega`: `Em Rota` ou `Entregue`.
- `CodigoRastreio`: Código de rastreio do pedido.
- FK para `pedido`.

### 6. **produto**
Catálogo de produtos disponíveis.
- `Categoria`: Ex: Eletrônico, Vestimenta, etc.
- `descProduto`, `Valor`, `size`.

### 7. **Produto_Pedido**
Relação entre produtos e pedidos (muitos-para-muitos).
- Quantidade de itens pedidos.

### 8. **fornecedor** e **fornecedor_produto**
Fornecedores dos produtos da loja.
- Informações como CNPJ, Razão Social, Nome Fantasia.
- Quantidade de produtos fornecidos.

### 9. **terceiros** e **terceiro_produto**
Representam vendedores terceiros.
- Armazenam produtos revendidos por esses terceiros.

### 10. **estoque** e **estoque_produto**
Controle de local e quantidade de produtos armazenados.
- Relacionamento muitos-para-muitos entre produtos e estoques.

---

## 📥 Inserções de Exemplo

O script também inclui **dados populados** para testes:
- Clientes (pessoas físicas e jurídicas).
- Pagamentos com cartão.
- Pedidos com produtos variados.
- Entregas com rastreamento.
- Estoques com diferentes produtos.
- Fornecedores e vendedores terceiros.

---

## 📊 Consultas SQL

### 📌 Pedidos por cliente com itens
```sql
SELECT c.Nome, c.cadastro, p.StatusPedido, prod.descProduto AS 'Descrição do Produto', 
       prod.Categoria, pp.Quantidade
FROM cliente c 
JOIN pedido p USING(idClient) 
JOIN produto_pedido pp USING(idPedido) 
JOIN produto prod ON pp.idProduto = prod.idProduto;
```

### 📦 Fornecedor, produto e estoque
```sql
SELECT f.RazaoSocial, f.NomeFantasia AS 'Nome Fantasia', prod.Categoria, 
       prod.Valor, prod.descProduto, es.local AS 'Local do estoque'
FROM fornecedor f 
JOIN fornecedor_produto fp USING (idFornecedor)
JOIN produto prod ON fp.idProduto = prod.idProduto
JOIN estoque_produto ep ON ep.idProduto = prod.idProduto 
JOIN estoque es USING(idEstoque);
```

### 🚚 Cliente, pedido e entrega
```sql
SELECT c.nome, c.cadastro, ped.descPedido, e.CodigoRastreio, e.StatusEntrega 
FROM cliente c 
JOIN pedido ped USING(idClient)
JOIN entrega e USING(idPedido);
```

### 💳 Cliente e forma de pagamento
```sql
SELECT * 
FROM cliente 
JOIN pagamento USING (idClient);
```

---

