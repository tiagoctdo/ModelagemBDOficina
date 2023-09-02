# ModelagemBDOficina
módulo de modelagem de BD com modelo ER, para criar o esquema lógico para o contexto de uma oficina.

-- Drop the database if it exists
DROP DATABASE IF EXISTS oficina;

-- Create the database 
CREATE DATABASE oficina;
USE oficina;

-- Table: cliente
CREATE TABLE Cliente (
    idCliente INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    dataNascimento DATE NOT NULL,
    NomeCompleto VARCHAR(50) NOT NULL,
    email VARCHAR(45),
    endereco VARCHAR(50),
    telefone VARCHAR(20)
);

-- table veiculo
CREATE TABLE Veiculo (
  IDveiculo INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  Placa VARCHAR(10) NOT NULL,
  Modelo VARCHAR(10),
  Ano VARCHAR(4),
  usuario_idcliente INT NOT NULL
);
-- TabelaOrdemdeServiço

   CREATE TABLE OrdemServico (
  OrdemID INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  DataAbertura DATE NOT NULL,
  DataFechamento DATE NOT NULL,
  VeiculoID INT NOT NULL,
  DescricaoProblema TEXT NOT NULL,
  DescricaoServicoRealizado TEXT NOT NULL,
  ValorTotal DECIMAL(10, 2) NOT NULL,
  IDveiculo INT NOT NULL,
  MecanicoResponsavel VARCHAR(45) NOT NULL,
  CONSTRAINT fk_OrdemServico_Veiculo1 FOREIGN KEY (VeiculoID)
    REFERENCES Veiculo (IDveiculo)
);

-- tabela peças
CREATE TABLE PeçasLoja (
  IDPeça INT NOT NULL AUTO_INCREMENT,
  Nome VARCHAR(50) NOT NULL,
  Descrição TEXT NOT NULL,
  PreçoUnitário DECIMAL(10, 2) NOT NULL,
  PRIMARY KEY (`IDPeça`)
);

-- Recuperações Simples com SELECT Statement:
SELECT * FROM cliente;

-- Filtros com WHERE Statement:
SELECT * FROM Veiculo WHERE usuario_idcliente = 123;

-- Expressões para Atributos Derivados:
SELECT OrdemID, ValorTotal, ValorTotal * 1.1 AS ValorTotalComTaxa FROM OrdemServico;

-- Ordenações com ORDER BY:
SELECT * FROM cliente ORDER BY NomeCompleto ASC;

-- Condições de Filtros aos Grupos – HAVING Statement:
SELECT MecanicoResponsavel, AVG(ValorTotal) AS MediaValorTotal
FROM OrdemServico
GROUP BY MecanicoResponsavel
HAVING MediaValorTotal > 500;

-- Junções entre Tabelas para Fornecer uma Perspectiva mais Complexa dos Dados:
SELECT OS.OrdemID, OS.DataAbertura, V.Placa, C.NomeCompleto
FROM OrdemServico OS
INNER JOIN Veiculo V ON OS.VeiculoID = V.IDveiculo
INNER JOIN Cliente C ON V.usuario_idcliente = C.idCliente
WHERE OS.OrdemID = 1;

