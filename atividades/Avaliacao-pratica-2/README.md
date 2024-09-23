**Proposta:
Terminar a avaliação-10 fazer consultas a tabela Matricula.
Criar scripts sql para prova Prática
Listar todos os alunos reprovados. Nome do Aluno, Nome da Disciplina, Nome do Professor, N1,N2,Média,Faltas, Status da reprovação("Reprovado por Média", "Reprovado por Falta")
Listar todos os alunos aprovados. Nome Aluno, Nome da Disciplina, Nome do Professor, N1,N2,Média,Faltas, Status da reprovação("Aprovado por Média");
Listar a quantidade de alunos aprovados;
Listar a quantidade de alunos reprovados;
Uso do PlaywithDocker com as seguintes operações: Instalação dos containers MYsql e PhpMyAdmin usando o Docker Compose conforme definido na Avaliação-10;
Execução de scripts semelhantes aos scripts citados acima.
Captura da imagem da saida do script;
Gerar no repositório do GitHub a pasta avaliacao_pratica-2 e nela gravar as imagens dos resultados da execução dos scripts.
Copiar o link do github especificamente da pasta avaliacao_pratica_2 na Avaliação que será criada no GoogleClassrom.
tocando na sua conta

login docker fachada
digite seu nome de usuário e senha
criar o arquivo .yml

faça vi docker-compose.yml

pressione a tecla i para entrar no modo INSERT do vi

version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: mysql_container
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: myuser
      MYSQL_PASSWORD: mypassword
    volumes:
      - mysql_data:/var/lib/mysql
    ports:
      - "3306:3306"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin_container
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
      PMA_USER: root
      PMA_PASSWORD: rootpassword
    ports:
      - "8080:80"
    depends_on:
      - mysql

volumes:
  mysql_data:
fechando o vi

pressione esc
pressione : (dois pontos)
digite wq e aperte enter
instalar o conector do mysql para o python

faça pip install mysql-connector-python
criando o container

fachada docker-compose up -d
não atendo no banco de dados

fachada use meubanco de dados;
para criar as tabelas

cole o código abaixo para criar as tabelas no banco de dados
Código:
CREATE TABLE IF NOT EXISTS TB_ALUNOS (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome TEXT
);
CREATE TABLE IF NOT EXISTS TB_PROFESSOR (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome TEXT
);
CREATE TABLE IF NOT EXISTS TB_DISCIPLINA (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome TEXT
);
CREATE TABLE IF NOT EXISTS Matricula (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome_aluno INT,
    nome_professor INT,
    disciplina INT,
    nota_N1 FLOAT,
    nota_N2 FLOAT,
    faltas INT,
    Aprovado_SN BOOLEAN,
    FOREIGN KEY (nome_aluno) REFERENCES TB_ALUNOS(id),
    FOREIGN KEY (nome_professor) REFERENCES TB_PROFESSOR(id),
    FOREIGN KEY (disciplina) REFERENCES TB_DISCIPLINA(id)
);
pressione enter para confirmar, caso ele não faça automaticamente
antes de sair do mysql, você deve fazer algumas inserções aleatórias para populares como tabelas

Código para tabelas populares:
-- Inserindo dados na tabela TB_ALUNOS
INSERT INTO TB_ALUNOS (nome) VALUES ('Joao Silva');
INSERT INTO TB_ALUNOS (nome) VALUES ('Maria Oliveira');
INSERT INTO TB_ALUNOS (nome) VALUES ('Carlos Souza');
INSERT INTO TB_ALUNOS (nome) VALUES ('Fernanda Costa');
INSERT INTO TB_ALUNOS (nome) VALUES ('Bruno Almeida');
INSERT INTO TB_ALUNOS (nome) VALUES ('Luana Carvalho');
INSERT INTO TB_ALUNOS (nome) VALUES ('Ricardo Barbosa');
INSERT INTO TB_ALUNOS (nome) VALUES ('Sofia Martins');
INSERT INTO TB_ALUNOS (nome) VALUES ('Paulo Ferreira');

-- Inserindo dados na tabela TB_PROFESSOR
INSERT INTO TB_PROFESSOR (nome) VALUES ('Prof. Ana Lima');
INSERT INTO TB_PROFESSOR (nome) VALUES ('Prof. Pedro Santos');
INSERT INTO TB_PROFESSOR (nome) VALUES ('Prof. Carla Mendes');
INSERT INTO TB_PROFESSOR (nome) VALUES ('Prof. Joao Souza');

-- Inserindo dados na tabela TB_DISCIPLINA
INSERT INTO TB_DISCIPLINA (nome) VALUES ('Matematica');
INSERT INTO TB_DISCIPLINA (nome) VALUES ('Historia');
INSERT INTO TB_DISCIPLINA (nome) VALUES ('Fisica');
INSERT INTO TB_DISCIPLINA (nome) VALUES ('Quimica');
INSERT INTO TB_DISCIPLINA (nome) VALUES ('Biologia');

-- Inserindo dados na tabela Matricula
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (1, 1, 1, 7.5, 8.0, 2, TRUE);
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (2, 2, 2, 6.0, 5.5, 5, FALSE);
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (3, 1, 1, 9.0, 8.5, 1, TRUE);
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (4, 3, 3, 6.5, 7.0, 3, TRUE);
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (5, 4, 4, 5.0, 6.0, 6, FALSE);
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (6, 1, 1, 8.5, 9.0, 0, TRUE);
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (7, 2, 5, 7.0, 7.5, 4, TRUE);
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (8, 3, 3, 9.0, 8.5, 1, TRUE);
INSERT INTO Matricula (nome_aluno, nome_professor, disciplina, nota_N1, nota_N2, faltas, Aprovado_SN) 
VALUES (9, 4, 4, 5.5, 6.0, 8, FALSE);
pressione enter para confirmar, caso ele não faça automaticamente
saindo do mysql

saída de fachada
criando o arquivo .py para realizar a tarefa

fachada vim main.py

pressione a tecla i para entrar no modo INSERT do vim

cole o script usando Control + Shift + V (no meu caso é assim que cola)

import mysql.connector

# Conexão com o banco de dados
conn = mysql.connector.connect(
    host='localhost',
    user='myuser',
    password='mypassword',
    database='mydatabase'
)

cursor = conn.cursor()

# Função para executar uma consulta e exibir os resultados
# Função para executar uma consulta e exibir os resultados
def executar_consulta(query):
    cursor.execute(query)
    result = cursor.fetchall()
    for row in result:
        # Usar 'join' para formatar a saída sem parênteses ou vírgulas
        print(", ".join(str(item) for item in row))


# 1. Listar todos os alunos reprovados
query_reprovados = """
SELECT 
    TB_ALUNOS.nome AS Nome_Aluno, 
    TB_DISCIPLINA.nome AS Nome_Disciplina, 
    TB_PROFESSOR.nome AS Nome_Professor, 
    Matricula.nota_N1, 
    Matricula.nota_N2, 
    (Matricula.nota_N1 + Matricula.nota_N2) / 2 AS Media, 
    Matricula.faltas, 
    CASE 
        WHEN (Matricula.nota_N1 + Matricula.nota_N2) / 2 < 6 THEN 'Reprovado por Média' 
        WHEN Matricula.faltas > 5 THEN 'Reprovado por Falta' 
    END AS Status_Reprovacao
FROM Matricula
JOIN TB_ALUNOS ON Matricula.nome_aluno = TB_ALUNOS.id
JOIN TB_DISCIPLINA ON Matricula.disciplina = TB_DISCIPLINA.id
JOIN TB_PROFESSOR ON Matricula.nome_professor = TB_PROFESSOR.id
WHERE Matricula.Aprovado_SN = FALSE;
"""
print("Alunos Reprovados:")
executar_consulta(query_reprovados)

# 2. Listar todos os alunos aprovados
query_aprovados = """
SELECT 
    TB_ALUNOS.nome AS Nome_Aluno, 
    TB_DISCIPLINA.nome AS Nome_Disciplina, 
    TB_PROFESSOR.nome AS Nome_Professor, 
    Matricula.nota_N1, 
    Matricula.nota_N2, 
    (Matricula.nota_N1 + Matricula.nota_N2) / 2 AS Media, 
    Matricula.faltas, 
    'Aprovado por Média' AS Status_Aprovacao
FROM Matricula
JOIN TB_ALUNOS ON Matricula.nome_aluno = TB_ALUNOS.id
JOIN TB_DISCIPLINA ON Matricula.disciplina = TB_DISCIPLINA.id
JOIN TB_PROFESSOR ON Matricula.nome_professor = TB_PROFESSOR.id
WHERE Matricula.Aprovado_SN = TRUE;
"""
print("\nAlunos Aprovados:")
executar_consulta(query_aprovados)

# 3. Listar a quantidade de alunos aprovados
query_quantidade_aprovados = """
SELECT COUNT(*) AS Quantidade_Aprovados
FROM Matricula
WHERE Matricula.Aprovado_SN = TRUE;
"""
print("\nQuantidade de Alunos Aprovados:")
executar_consulta(query_quantidade_aprovados)

# 4. Listar a quantidade de alunos reprovados
query_quantidade_reprovados = """
SELECT COUNT(*) AS Quantidade_Reprovados
FROM Matricula
WHERE Matricula.Aprovado_SN = FALSE;
"""
print("\nQuantidade de Alunos Reprovados:")
executar_consulta(query_quantidade_reprovados)

# Fechando a conexão
cursor.close()
conn.close()
coloque a senha rootpassword
fechando o vi file.py

pressione esc
pressione : (dois pontos)
digite wq e aperte enter
o que o que faz?
ele salva o arquivo
mudando o script

fachada python3 main.py
o resultado deverá retornar o que se pede na questão.
saindo da sua conta

fachada docker logout**
