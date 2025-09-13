
📚 Livraria Dev Saber

Projeto desenvolvido no Programa Desenvolve - Grupo Boticário 2025.
Consiste em uma loja fictícia online (Livraria Dev Saber), com foco na modelagem e análise de dados no Google BigQuery.

🎯 Objetivo do Projeto

Criar um dataset no BigQuery.

Construir tabelas para Clientes, Produtos e Vendas.

Inserir dados fictícios nas tabelas.

Executar consultas de negócio.

Criar VIEWs para simplificar análises.

🗂 Estrutura do Projeto

Dataset: Livraria_DevSaber

Tabelas:

Clientes

Produtos

Vendas

🛠️ Passo a Passo
1. Criação das Tabelas
-- Exemplo de criação de tabela Clientes
CREATE TABLE `Livraria_DevSaber.Clientes` (
  id_cliente INT64,
  nome STRING,
  email STRING,
  data_cadastro DATE
);

-- Exemplo de criação de tabela Produtos
CREATE TABLE `Livraria_DevSaber.Produtos` (
  id_produto INT64,
  titulo_produto STRING,
  autor_produto STRING,
  preco NUMERIC
);

-- Exemplo de criação de tabela Vendas
CREATE TABLE `Livraria_DevSaber.Vendas` (
  id_venda INT64,
  id_cliente INT64,
  id_produto INT64,
  data_venda DATE,
  quantidade INT64
);

2. Inserção de Dados
INSERT INTO `Livraria_DevSaber.Clientes` (id_cliente, nome, email, data_cadastro)
VALUES (1, "Maria Silva", "maria@email.com", "2024-01-15");

INSERT INTO `Livraria_DevSaber.Produtos` (id_produto, titulo_produto, autor_produto, preco)
VALUES (1, "SQL Básico", "João Souza", 59.90);

INSERT INTO `Livraria_DevSaber.Vendas` (id_venda, id_cliente, id_produto, data_venda, quantidade)
VALUES (1, 1, 1, "2024-02-01", 2);

📊 Consultas de Negócio
Livros mais vendidos da semana
SELECT titulo_produto, SUM(quantidade) AS total_vendido
FROM `Livraria_DevSaber.Vendas` v
JOIN `Livraria_DevSaber.Produtos` p
  ON v.id_produto = p.id_produto
WHERE data_venda BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 7 DAY) AND CURRENT_DATE()
GROUP BY titulo_produto
ORDER BY total_vendido DESC;

Desempenho de vendas por mês
SELECT EXTRACT(MONTH FROM data_venda) AS mes, SUM(quantidade) AS total_vendido
FROM `Livraria_DevSaber.Vendas`
GROUP BY mes
ORDER BY mes;

Ticket médio
SELECT AVG(preco * quantidade) AS ticket_medio
FROM `Livraria_DevSaber.Vendas` v
JOIN `Livraria_DevSaber.Produtos` p
  ON v.id_produto = p.id_produto;

Taxa de recompra
SELECT id_cliente,
       COUNT(DISTINCT data_venda) AS qtd_compras,
       CASE WHEN COUNT(DISTINCT data_venda) > 1 THEN "Recomprador"
            ELSE "Não Recomprador" END AS perfil
FROM `Livraria_DevSaber.Vendas`
GROUP BY id_cliente;

Receita total
SELECT SUM(p.preco * v.quantidade) AS receita_total
FROM `Livraria_DevSaber.Vendas` v
JOIN `Livraria_DevSaber.Produtos` p
  ON v.id_produto = p.id_produto;

Receita mensal
SELECT EXTRACT(MONTH FROM data_venda) AS mes,
       SUM(p.preco * v.quantidade) AS receita_mensal
FROM `Livraria_DevSaber.Vendas` v
JOIN `Livraria_DevSaber.Produtos` p
  ON v.id_produto = p.id_produto
GROUP BY mes
ORDER BY mes;

👓 VIEWs
O que é uma VIEW?

Uma VIEW é uma tabela virtual que sempre reflete os dados mais recentes das tabelas subjacentes, sem necessidade de reexecutar manualmente o código.

Exemplo: Consulta de Taxa de Recompra
CREATE VIEW `Livraria_DevSaber.View_TaxaRecompra` AS
SELECT id_cliente,
       COUNT(DISTINCT data_venda) AS qtd_compras,
       CASE WHEN COUNT(DISTINCT data_venda) > 1 THEN "Recomprador"
            ELSE "Não Recomprador" END AS perfil
FROM `Livraria_DevSaber.Vendas`
GROUP BY id_cliente;

🚀 Como Executar

Acesse o Google BigQuery.

Crie um dataset chamado Livraria_DevSaber.

Crie as tabelas Clientes, Produtos e Vendas.

Insira os dados fictícios fornecidos.

Rode as consultas de negócio.

Crie e utilize as VIEWs para análises contínuas.
