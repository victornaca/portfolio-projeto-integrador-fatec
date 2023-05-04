<h1 p align="center"> 
Victor Fernandes dos Santos | Portifólio
<p align="center"> 
 <a href="https://www.python.org/"><img src="https://img.shields.io/badge/Curso%3A-Banco de Dados-purple"/></a>
 <a href="https://discordpy.readthedocs.io/en/stable/"><img src="https://img.shields.io/badge/Linguagem Principal%3A-Python-purple"/></a>
 <a href="https://squarecloud.app/"><img src="https://img.shields.io/badge/Metodologia%3A-API-purple"/></a>
</p> 

## Introdução
>### O que é API?
O termo API vem de Aprendendo por Projeto Integrador é um texto modelo da indústria tipográfica e de impressão. <p>
O Lorem Ipsum tem vindo a ser o texto padrão usado por estas indústrias desde o ano de 1500, quando uma misturou os caracteres de um texto para criar um espécime de livro. Este texto não só sobreviveu 5 séculos, mas também o salto para a tipografia electrónica, mantendo-se essencialmente inalterada.

>### O que é Banco de Dados?
Além de projetar, implementar e gerenciar sistemas de banco de dados, o profissional desenvolve métodos para o uso de informações no apoio à tomada de decisões gerenciais.

Um banco de dados é um sistema de armazenamento de informações que permite que os dados sejam organizados, gerenciados e acessados de forma eficiente. O sistema armazena dados em um esquema lógico, de forma que eles sejam facilmente acessados pelos colaboradores que demandam o acesso às informações.

Faz parte da rotina de trabalho do profissional de Banco de Dados organizar as informações da empresa, garantir a segurança, definir as permissões de acesso ao sistema e desenvolver códigos.

>### Quem sou eu?
Estudante de Tecnologo em Banco de Dados e apaixonado por dados, me chamo Victor Fernandes dos Santos, possuo 24 anos e atualmente sou estágiario de consumo irregular pela EDP São Paulo, estágio esse onde realizo ETL de dados, automações de processos em linguagem Python e visualização de gráficos em BI.

Iniciei a faculdade por incentivo de colegas que viram potencial na minha facilidade de modelagem de dados e com a linguagem de SQL.

>### Principais Conhecimentos

>#### SQL

Uma das minhas principais linguagens onde tive o conhecimento prévio em 2015 com o Tecnico de Tecnologia da Informação realizado na ETEC Machado de Assis. Após minha formação em técnico deixei a linguagem e a tecnologia de lado na tentativa de realizar outros planos quando recebi o convite para trabalho de suporte onde voltei a ter contato com o SQL.
Hoje é uma das minhas principais linguagens no trabalho para trabalho de ETL e analise de dados.

## Meus Projetos
>### Automatização e Visualização dos Dados - IACIT | 2022-2

A IACIT é uma empresa de consultoria meteorológica, e hoje um de seus serviços é fornecer aos clientes relatórios customizados de dados meteorológicos. Como a empresa trabalha processando muitas informações manualmente acabam que perdendo tempo e desperdiçando recursos. 
Pensando nisso nós da Fluffy junto ao cliente pensamos em desenvolver um sistema que permite realizar a importação dos dados meteorológicos, bem como armazená-los em uma base de dados para posteriormente gerar os relatórios desejados pelos clientes.

[- Conheça o projeto clicando aqui](https://github.com/fluffyfatec/Iacit)<br><br>

>#### Tecnologias Utilizadas

<details>
<summary>Front-End</summary>

* JavaScript
* HTML
* CSS

</details>
<details>
<summary>Back-End</summary>

* Java
* Python
* Spring boot

</details>

<details>
<summary>Banco de Dados</summary>

* PostgreSQL
* SQL
</details>
<br>

>#### Contribuições Pessoais

Além da participação integral no banco de dados como função de DBA, realizando todo processo de levantamento da infraestrutura do banco pensando no escalonamento dos dados que passaram de 13Mi de linhas.</p>

Ainda na função de DBA realizei criação e validação de DER(Diagrama Entidade Relacionamento), Diagrama Lógico, Biblioteca de dados e criação do banco e tabelas. Um dos meus principais desafios no projeto foi a criação do log de auditoria via Trigger.

Abaixo irei demonstrar o DER apresentado como solução para a empresa parceira IACIT, escalavel e normalizado. 
A ideia era quebrar o CSV que vinha com todos os dados por categoria, ja que a plotagem dos graficos e as gerações de insights seriam separadosm. Ao aplicar essa ideia tivemos um ganho na velocidade dos scripts e na geração dos graficos, mesmo tendo milhões de linhas.

![image]('https://raw.githubusercontent.com/fluffyfatec/Iacit/Sprint-4/GIT/DERCSV.jpg)

Abaixo irei demonstrar a Trigger para Log de Auditoria criado para o projeto:

```sql
-- Aqui está sendo criado a function que retorna a Trigger.
CREATE OR REPLACE FUNCTION process_tr_tb_usuario() RETURNS TRIGGER AS $tr_tb_usuario$
DECLARE
 VS_IN_TIPO_OPERACAO  CHAR(1)  := NULL;
-- Aqui iniciado as lógicas para dizer se a operação foi de insert, update ou delete.
BEGIN
  	IF (TG_OP = 'INSERT') THEN
    	VS_IN_TIPO_OPERACAO  := 'I';
  	ELSIF (TG_OP = 'UPDATE') THEN
    	VS_IN_TIPO_OPERACAO  := 'U';
  	ELSIF (TG_OP = 'DELETE') THEN
    	VS_IN_TIPO_OPERACAO  := 'D';
  	END IF;
    -- Caso a operação for Insert ou Update ele irá cadastrar na tabela log os 
    -- novos dados, junto de qual foi a operação realizada
    IF (TG_OP = 'INSERT') OR (TG_OP = 'UPDATE') THEN
	BEGIN
	INSERT INTO log_usuario(
		log_cod_usuario, log_usuario_nome, log_usuario_senha, log_usuario_username, 
        log_cod_permissao, log_usuario_alterou, log_usuario_operacao
	)
	VALUES(
		NEW.cod_usuario, NEW.usuario_nome, NEW.usuario_senha, NEW.usuario_username,
		NEW.cod_permissao, NEW.usuario_alterou, VS_IN_TIPO_OPERACAO);
	RETURN NULL; 
	END;
    -- Caso a operação for Delete ele irá cadastrar na tabela log os 
    -- dados antigos, junto de qual foi a operação realizada
	ELSIF (TG_OP = 'DELETE') THEN
    BEGIN
      INSERT INTO log_usuario(
		log_cod_usuario, log_usuario_nome, log_usuario_senha, log_usuario_username,
		log_cod_permissao, log_usuario_alterou, log_usuario_operacao
	)
	VALUES(
		OLD.cod_usuario, OLD.usuario_nome, OLD.usuario_senha, OLD.usuario_username,
		OLD.cod_permissao, OLD.usuario_alterou, VS_IN_TIPO_OPERACAO);
	RETURN NULL; 
	END;
END IF;
RETURN NULL; 
END;
$tr_tb_usuario$ LANGUAGE plpgsql;
```

Basicamente a Trigger foi utilizada pegando os novos ou antigos dados para gerar um histórico onde poderiamos ver o que foi realizado por usuario X ou Y. Esse log foi feito tanto para usuario como para alterações nas estações.

>#### Aprendizados Efetivos

Nesse projeto tive uma curta experiencia (nas adequadas proporções) de como é realizada a função do DBA junto ao time de desenvolvimento, realizando a construção de toda arquitetura da aplicação pensando em como irá reagir os dados ao longo do projeto.
Um projeto que além de conter muitas linhas de dados também possuiu muita requisição para realizar as plotagens dos graficos e geração dos realatórios.

Também no mesmo projeto tive o primeiro contato com as triggers e functions, construindo um log de auditoria mesmos sem saber quais os elementos necessarios para ser considerado um log de auditoria.

Vale destacar também que foi o primeiro contato com o SGBD PostgreSQL em modo local. Um bom SGBD por ser gratuito de ter boas funções administrativas mas que ainda possui alguns bugs, algumas dificuldades e lentidão anormal para executar alguns processos.