<h1 p align="center"> 
Victor Fernandes dos Santos | Portifólio das APIs 

## Introdução
>### O que é API?
A API (Aprendizagem por Projetos Integrados), desenvolvida no escopo do CADI, é a metodologia de ensino em implantação na Fatec São José dos Campos, desde o Segundo Semestre de 2019 (nos cursos da área de Computação, porém, a metodologia estará completamente implantada em todos os períodos até Primeiro Semestre de 2022).

>#### Pilares da API
- Real Problem Based Learning (rPBL)
- Validação Externa
- Mindset Ágil (Agile)

>### O que é Banco de Dados?
Além de projetar, implementar e gerenciar sistemas de banco de dados, o profissional desenvolve métodos para o uso de informações no apoio à tomada de decisões gerenciais.

Um banco de dados é um sistema de armazenamento de informações que permite que os dados sejam organizados, gerenciados e acessados de forma eficiente. O sistema armazena dados em um esquema lógico, de forma que eles sejam facilmente acessados pelos colaboradores que demandam o acesso às informações.

Faz parte da rotina de trabalho do profissional de Banco de Dados organizar as informações da empresa, garantir a segurança, definir as permissões de acesso ao sistema e desenvolver códigos.

>### Sobre mim
<p align="center"><img src="https://github.com/victornaca/portfolio-projeto-integrador-fatec/blob/main/Resumo/VictorFernandes.png" width="20%"></p>
Sou formado como Técnico em Tecnologia da Informação pela Escola Tecnológica de Caçapava (ETEC) e atualmente estou matriculado no 5º semestre do curso tecnólogo em Banco de Dados na Faculdade de Tecnologia de São José dos Campos (FATEC).

Minha trajetória profissional inclui experiência na área de tecnologia e dados. Antes de ingressar na faculdade, tive a oportunidade de trabalhar como auxiliar de TI em uma rede de supermercados, a Rede Simpatia, onde retomei meu contato com a tecnologia e dados.

Durante o meu período na faculdade, realizei um estágio como Analista de Dados na EDP Brasil, onde participei ativamente de projetos de redução de custos no cluster e da reestruturação de jobs no nosso Lakehouse, resultando em uma economia significativa de 40% para o cluster.

Atualmente, estou empregado na Safran como Analista de Dados, enfrentando o desafio de estabelecer uma equipe de dados que atende não apenas o Brasil, mas também países da América, incluindo México e EUA.

Ao longo da minha jornada, pretendo adquirir mais experiência nas áreas de programação, dados e inteligência artificial, além de continuar meus estudos e obter certificações de mestrado e doutorado. Essa ampla gama de conhecimentos me ajudará a concretizar meu objetivo de, no futuro, fundar minha própria empresa na área de tecnologia.

>### Principais Conhecimentos

>#### SQL

Uma das minhas principais linguagens onde tive o conhecimento prévio em 2015 com o Tecnico de Tecnologia da Informação realizado na ETEC Machado de Assis. Após minha formação em técnico deixei a linguagem e a tecnologia de lado na tentativa de realizar outros planos quando recebi o convite para trabalho de suporte onde voltei a ter contato com o SQL.
Hoje é uma das minhas principais linguagens no trabalho para trabalho de ETL e analise de dados.

## Meus Projetos
>### Dashboard COVID-19 - Interno | 2021-2

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

<details>
<summary>Diagrama Entidade Relacionamento</summary>
<img align="center" alt="Victor" height="100%" width="100%" src="https://github.com/fluffyfatec/Iacit/blob/Sprint-4/GIT/DERCSV.jpg">
</details>
<details>
<summary>Trigger Log de Auditoria</summary>
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
</details>

>#### Aprendizados Efetivos

-Experiencia efetiva como DBA realizando desde a construção da estrutura até manter a escabilidade do mesmo;

-Construção de log de auditoria utilizando Trigger e Function;