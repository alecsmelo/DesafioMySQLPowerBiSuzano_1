# Desafio Banco de Dados E-commerce DIO

### Projeto de construção de banco de dados visual usando o MySQL Workbench

## 🥇 Desafio

<p>
Construir um modelo de banco de dados inspirado no exibido no Bootcamp DIO PowerBi Suzano, fazendo 
melhorias nos seguintes itens:
- Cliente PJ e PF – Uma conta pode ser PJ ou PF, mas não pode ter as duas informações;
- Pagamento – Pode ter cadastrado mais de uma forma de pagamento;
- Entrega – Possui status e código de rastreio;
</p>

## ⚙️ Script MySQL

~~~ MySQL
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `mydb` DEFAULT CHARACTER SET utf8 ;
USE `mydb` ;

-- -----------------------------------------------------
-- Table `mydb`.`CadastroGeral`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`CadastroGeral` (
  `idCadastroGeral` INT NOT NULL,
  `Nome` VARCHAR(45) NULL,
  `Endereço` VARCHAR(45) NULL,
  `Cidade` VARCHAR(45) NULL,
  `Estado` VARCHAR(45) NULL,
  `Email` VARCHAR(45) NULL,
  `Telefone` VARCHAR(45) NULL,
  PRIMARY KEY (`idCadastroGeral`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`ClientePF`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`ClientePF` (
  `idCliente` INT NOT NULL,
  `CPF` VARCHAR(45) NULL,
  `Clientecol` VARCHAR(45) NULL,
  `CadastroGeral_idCadastroGeral` INT NOT NULL,
  PRIMARY KEY (`idCliente`),
  INDEX `fk_ClientePF_CadastroGeral_idx` (`CadastroGeral_idCadastroGeral` ASC) VISIBLE,
  CONSTRAINT `fk_ClientePF_CadastroGeral`
    FOREIGN KEY (`CadastroGeral_idCadastroGeral`)
    REFERENCES `mydb`.`CadastroGeral` (`idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Cliente_PJ`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Cliente_PJ` (
  `idCliente_PJ` INT NOT NULL,
  `RazaoSocial` VARCHAR(250) NULL,
  `CNPJ` VARCHAR(45) NOT NULL,
  `InscEstadual` VARCHAR(45) NULL,
  `CadastroGeral_idCadastroGeral` INT NOT NULL,
  PRIMARY KEY (`idCliente_PJ`, `CadastroGeral_idCadastroGeral`),
  UNIQUE INDEX `CNPJ_UNIQUE` (`CNPJ` ASC) VISIBLE,
  INDEX `fk_Cliente_PJ_CadastroGeral1_idx` (`CadastroGeral_idCadastroGeral` ASC) VISIBLE,
  CONSTRAINT `fk_Cliente_PJ_CadastroGeral1`
    FOREIGN KEY (`CadastroGeral_idCadastroGeral`)
    REFERENCES `mydb`.`CadastroGeral` (`idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Forncedores`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Forncedores` (
  `idForncedores` INT NOT NULL,
  `RazaoSocial` VARCHAR(250) NULL,
  `CNPJ` VARCHAR(45) NOT NULL,
  `InscEstadual` VARCHAR(45) NULL,
  `CadastroGeral_idCadastroGeral` INT NOT NULL,
  PRIMARY KEY (`idForncedores`, `CadastroGeral_idCadastroGeral`),
  UNIQUE INDEX `CNPJ_UNIQUE` (`CNPJ` ASC) VISIBLE,
  UNIQUE INDEX `InscEstadual_UNIQUE` (`InscEstadual` ASC) VISIBLE,
  INDEX `fk_Forncedores_CadastroGeral1_idx` (`CadastroGeral_idCadastroGeral` ASC) VISIBLE,
  CONSTRAINT `fk_Forncedores_CadastroGeral1`
    FOREIGN KEY (`CadastroGeral_idCadastroGeral`)
    REFERENCES `mydb`.`CadastroGeral` (`idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`VendedorMktPlace`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`VendedorMktPlace` (
  `idVendedorMktPlace` INT NOT NULL,
  `Nome` VARCHAR(250) NULL,
  `CNPJ` VARCHAR(45) NOT NULL,
  `InscEstadual` VARCHAR(45) NULL,
  `CadastroGeral_idCadastroGeral` INT NOT NULL,
  PRIMARY KEY (`idVendedorMktPlace`, `CadastroGeral_idCadastroGeral`),
  UNIQUE INDEX `CNPJ_UNIQUE` (`CNPJ` ASC) VISIBLE,
  INDEX `fk_VendedorMktPlace_CadastroGeral1_idx` (`CadastroGeral_idCadastroGeral` ASC) VISIBLE,
  CONSTRAINT `fk_VendedorMktPlace_CadastroGeral1`
    FOREIGN KEY (`CadastroGeral_idCadastroGeral`)
    REFERENCES `mydb`.`CadastroGeral` (`idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`MetodosPagamentos`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`MetodosPagamentos` (
  `idMetodosPagamentos` INT NOT NULL,
  `CartaoCred` VARCHAR(45) NULL,
  `Validade` VARCHAR(45) NULL,
  `NomeImpresso` VARCHAR(45) NULL,
  `Bandeira` VARCHAR(45) NULL,
  `ClientePF_idCliente` INT NOT NULL,
  `Cliente_PJ_idCliente_PJ` INT NOT NULL,
  `Cliente_PJ_CadastroGeral_idCadastroGeral` INT NOT NULL,
  PRIMARY KEY (`idMetodosPagamentos`, `ClientePF_idCliente`, `Cliente_PJ_idCliente_PJ`, `Cliente_PJ_CadastroGeral_idCadastroGeral`),
  UNIQUE INDEX `CartaoCred_UNIQUE` (`CartaoCred` ASC) VISIBLE,
  INDEX `fk_MetodosPagamentos_ClientePF1_idx` (`ClientePF_idCliente` ASC) VISIBLE,
  INDEX `fk_MetodosPagamentos_Cliente_PJ1_idx` (`Cliente_PJ_idCliente_PJ` ASC, `Cliente_PJ_CadastroGeral_idCadastroGeral` ASC) VISIBLE,
  CONSTRAINT `fk_MetodosPagamentos_ClientePF1`
    FOREIGN KEY (`ClientePF_idCliente`)
    REFERENCES `mydb`.`ClientePF` (`idCliente`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_MetodosPagamentos_Cliente_PJ1`
    FOREIGN KEY (`Cliente_PJ_idCliente_PJ` , `Cliente_PJ_CadastroGeral_idCadastroGeral`)
    REFERENCES `mydb`.`Cliente_PJ` (`idCliente_PJ` , `CadastroGeral_idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Estoque`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Estoque` (
  `idEstoque` INT NOT NULL,
  `Quantidade` VARCHAR(45) NULL,
  `UnidadeArmazenagem` VARCHAR(45) NULL,
  `Forncedores_idForncedores` INT NOT NULL,
  `Forncedores_CadastroGeral_idCadastroGeral` INT NOT NULL,
  PRIMARY KEY (`idEstoque`, `Forncedores_idForncedores`, `Forncedores_CadastroGeral_idCadastroGeral`),
  INDEX `fk_Estoque_Forncedores1_idx` (`Forncedores_idForncedores` ASC, `Forncedores_CadastroGeral_idCadastroGeral` ASC) VISIBLE,
  CONSTRAINT `fk_Estoque_Forncedores1`
    FOREIGN KEY (`Forncedores_idForncedores` , `Forncedores_CadastroGeral_idCadastroGeral`)
    REFERENCES `mydb`.`Forncedores` (`idForncedores` , `CadastroGeral_idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Produto`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Produto` (
  `idtable1` INT NOT NULL,
  `NomeProduto` VARCHAR(45) NULL,
  `Categoria` VARCHAR(45) NULL,
  `Descrição` VARCHAR(45) NULL,
  `Valor` VARCHAR(45) NULL,
  `VendedorMktPlace_idVendedorMktPlace` INT NOT NULL,
  `VendedorMktPlace_CadastroGeral_idCadastroGeral` INT NOT NULL,
  `Estoque_idEstoque` INT NOT NULL,
  `Estoque_Forncedores_idForncedores` INT NOT NULL,
  `Estoque_Forncedores_CadastroGeral_idCadastroGeral` INT NOT NULL,
  PRIMARY KEY (`idtable1`, `VendedorMktPlace_idVendedorMktPlace`, `VendedorMktPlace_CadastroGeral_idCadastroGeral`, `Estoque_idEstoque`, `Estoque_Forncedores_idForncedores`, `Estoque_Forncedores_CadastroGeral_idCadastroGeral`),
  INDEX `fk_Produto_VendedorMktPlace1_idx` (`VendedorMktPlace_idVendedorMktPlace` ASC, `VendedorMktPlace_CadastroGeral_idCadastroGeral` ASC) VISIBLE,
  INDEX `fk_Produto_Estoque1_idx` (`Estoque_idEstoque` ASC, `Estoque_Forncedores_idForncedores` ASC, `Estoque_Forncedores_CadastroGeral_idCadastroGeral` ASC) VISIBLE,
  CONSTRAINT `fk_Produto_VendedorMktPlace1`
    FOREIGN KEY (`VendedorMktPlace_idVendedorMktPlace` , `VendedorMktPlace_CadastroGeral_idCadastroGeral`)
    REFERENCES `mydb`.`VendedorMktPlace` (`idVendedorMktPlace` , `CadastroGeral_idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Produto_Estoque1`
    FOREIGN KEY (`Estoque_idEstoque` , `Estoque_Forncedores_idForncedores` , `Estoque_Forncedores_CadastroGeral_idCadastroGeral`)
    REFERENCES `mydb`.`Estoque` (`idEstoque` , `Forncedores_idForncedores` , `Forncedores_CadastroGeral_idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Entrega`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Entrega` (
  `idEntrega` INT NOT NULL,
  `CodRastreio` VARCHAR(45) NULL,
  `StatusEntrega` VARCHAR(45) NULL,
  PRIMARY KEY (`idEntrega`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Pedido_ClientePF`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Pedido_ClientePF` (
  `ClientePF_idCliente` INT NOT NULL,
  `Produto_idtable1` INT NOT NULL,
  `Produto_Estoque_idEstoque` INT NOT NULL,
  `MetodosPagamentos_idMetodosPagamentos` INT NOT NULL,
  `MetodosPagamentos_ClientePF_idCliente` INT NOT NULL,
  `Entrega_idEntrega` INT NOT NULL,
  PRIMARY KEY (`ClientePF_idCliente`, `Produto_idtable1`, `Produto_Estoque_idEstoque`, `Entrega_idEntrega`),
  INDEX `fk_ClientePF_has_Produto_Produto1_idx` (`Produto_idtable1` ASC, `Produto_Estoque_idEstoque` ASC) VISIBLE,
  INDEX `fk_ClientePF_has_Produto_ClientePF1_idx` (`ClientePF_idCliente` ASC) VISIBLE,
  INDEX `fk_Pedido_ClientePF_MetodosPagamentos1_idx` (`MetodosPagamentos_idMetodosPagamentos` ASC, `MetodosPagamentos_ClientePF_idCliente` ASC) VISIBLE,
  INDEX `fk_Pedido_ClientePF_Entrega1_idx` (`Entrega_idEntrega` ASC) VISIBLE,
  CONSTRAINT `fk_ClientePF_has_Produto_ClientePF1`
    FOREIGN KEY (`ClientePF_idCliente`)
    REFERENCES `mydb`.`ClientePF` (`idCliente`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_ClientePF_has_Produto_Produto1`
    FOREIGN KEY (`Produto_idtable1` , `Produto_Estoque_idEstoque`)
    REFERENCES `mydb`.`Produto` (`idtable1` , `Estoque_idEstoque`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Pedido_ClientePF_MetodosPagamentos1`
    FOREIGN KEY (`MetodosPagamentos_idMetodosPagamentos` , `MetodosPagamentos_ClientePF_idCliente`)
    REFERENCES `mydb`.`MetodosPagamentos` (`idMetodosPagamentos` , `ClientePF_idCliente`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Pedido_ClientePF_Entrega1`
    FOREIGN KEY (`Entrega_idEntrega`)
    REFERENCES `mydb`.`Entrega` (`idEntrega`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Pedido_ClientePJ`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Pedido_ClientePJ` (
  `Produto_idtable1` INT NOT NULL,
  `Produto_Estoque_idEstoque` INT NOT NULL,
  `Cliente_PJ_idCliente_PJ` INT NOT NULL,
  `MetodosPagamentos_idMetodosPagamentos` INT NOT NULL,
  `MetodosPagamentos_Cliente_PJ_idCliente_PJ` INT NOT NULL,
  `Entrega_idEntrega` INT NOT NULL,
  PRIMARY KEY (`Produto_idtable1`, `Produto_Estoque_idEstoque`, `Cliente_PJ_idCliente_PJ`, `Entrega_idEntrega`),
  INDEX `fk_Produto_has_Cliente_PJ_Cliente_PJ1_idx` (`Cliente_PJ_idCliente_PJ` ASC) VISIBLE,
  INDEX `fk_Produto_has_Cliente_PJ_Produto1_idx` (`Produto_idtable1` ASC, `Produto_Estoque_idEstoque` ASC) VISIBLE,
  INDEX `fk_Pedido_ClientePJ_MetodosPagamentos1_idx` (`MetodosPagamentos_idMetodosPagamentos` ASC, `MetodosPagamentos_Cliente_PJ_idCliente_PJ` ASC) VISIBLE,
  INDEX `fk_Pedido_ClientePJ_Entrega1_idx` (`Entrega_idEntrega` ASC) VISIBLE,
  CONSTRAINT `fk_Produto_has_Cliente_PJ_Produto1`
    FOREIGN KEY (`Produto_idtable1` , `Produto_Estoque_idEstoque`)
    REFERENCES `mydb`.`Produto` (`idtable1` , `Estoque_idEstoque`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Produto_has_Cliente_PJ_Cliente_PJ1`
    FOREIGN KEY (`Cliente_PJ_idCliente_PJ`)
    REFERENCES `mydb`.`Cliente_PJ` (`idCliente_PJ`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Pedido_ClientePJ_MetodosPagamentos1`
    FOREIGN KEY (`MetodosPagamentos_idMetodosPagamentos` , `MetodosPagamentos_Cliente_PJ_idCliente_PJ`)
    REFERENCES `mydb`.`MetodosPagamentos` (`idMetodosPagamentos` , `Cliente_PJ_idCliente_PJ`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Pedido_ClientePJ_Entrega1`
    FOREIGN KEY (`Entrega_idEntrega`)
    REFERENCES `mydb`.`Entrega` (`idEntrega`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

~~~
<br>
<br> 
## 🖼️ Diagrama Banco de Dados 

![Diagrama DB](https://github.com/alecsmelo/DesafioMySQLPowerBiSuzano_1/blob/main/Projeto_Banco_Dados_ECommerce_DIO.jpg)

<br>
<br>

> Arquivos anexo em PDF e MySQL para conferência do resultado

### 🔗 [Download PDF](https://github.com/alecsmelo/DesafioMySQLPowerBiSuzano_1/releases/tag/PDF)
