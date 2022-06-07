### Projeto_Interdisciplinar
CREATE DATABASE ocorrenciascpd;

CREATE SCHEMA tb;

CREATE SCHEMA vw;

CREATE TABLE tb.funcionario(
	matricula bigint,
	nome varchar(69) NOT NULL,
	departamento bigint,
	CONSTRAINT funcionario_pk primary KEY(matricula)
);

CREATE TABLE tb.departamento(
	id bigint,
      nome varchar(50),
	descricao text,
	CONSTRAINT departamento_pk primary KEY(id)
);

CREATE TABLE tb.ocorrencia(
	id serial,
	status varchar(10) NOT NULL DEFAULT 'aberta',
	data date,
	descricao text,
	funcionario bigint,
	departamento bigint,
	CONSTRAINT ocorrencias_pk primary KEY(id),
	CONSTRAINT status_check CHECK((status = 'aberta') OR (status = 'encerrada')),
	CONSTRAINT date_check CHECK(data <= current_date)
);

ALTER TABLE tb.ocorrencia 
ADD CONSTRAINT func_ocorrencia_fk FOREIGN KEY(funcionario) REFERENCES tb.funcionario(matricula)
ON UPDATE CASCADE ON DELETE SET NULL;

ALTER TABLE tb.ocorrencia 
ADD CONSTRAINT depto_ocorrencia_fk FOREIGN KEY(departamento) REFERENCES tb.departamento(id)
ON UPDATE CASCADE ON DELETE SET NULL;

ALTER TABLE tb.funcionario
ADD CONSTRAINT func_depto_fk FOREIGN KEY(departamento) REFERENCES tb.departamento(id)
ON UPDATE CASCADE ON DELETE SET NULL;
