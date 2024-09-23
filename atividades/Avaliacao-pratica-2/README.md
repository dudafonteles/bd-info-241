### Proposta: 
- Terminar a avaliação-10 fazer consultas a tabela Matricula. 
- Criar scripts sql para para prova Prática
  1) Listar todos os aluno reprovados. Nome do Aluno, Nome da Disciplina, Nome do Professor, N1,N2,Média,Faltas, Status da reprovação("Reprovado por Média", "Reprovado por Falta") 
  2) Listar todos os alunos aprovados. Nome Aluno, Nome da Disciplina, Nome do Professor, N1,N2,Média,Faltas, Status da reprovação("Aprovado por Média"); 
  3) Listar a Quantidade de alunos aprovados;
  4) Listar a Quantidade de alunos reprovados;

1) Uso do PlaywithDocker com as seguintes operações: Instalação dos containers MYsql e PhpMyAdmin usando  o Docker Compose como definido na Avaliação-10;
2) Execução de Scripts similares aos scripts citados acima. 
3) Captura da imagem da saida do script;
4) Gerar no repositorio do GitHub a pasta avaliacao_pratica-2 e nela gravar as imagens dos resultados da execução dos scripts.
5) Copiar o link do github especificamente da pasta avaliacao_pratica_2 na Avaliação que será criada no GoogleClassrom.


    
- entrando em sua conta
    - faça **docker login**
    - digite seu nome de usuario e senha
- criar o arquivo .yml
  - faça **vi docker-compose.yml**
  - pressione a tecla **i** para entrar no modo INSERT do vi
  
    ~~~yml

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


  - fechando o vi
    - pressione **esc**
    - pressione **:** (dois pontos)
    - digite **wq** e aperte **enter**
      

- criando o container
  - faça **docker-compose up -d**

- instale o connector do mysql para o python
  - faça **pip install mysql-connector-python**

- criando o arquivo .py para realizar a tarefa
  - faça **vi main.py**
  - pressione a tecla **i** para entrar no modo INSERT do vim
  - cole o script usando **Control + Shift + V**

    ~~~python
    
    import mysql.connector
    connect = mysql.connector.connect(
    user= "myuser",
    password= "mypassword",
    host= "localhost",
    database= "mydatabase"
    )

    cursor = connect.cursor()

    tabelas = ["TB_MATRICULA", "TB_ALUNO", "TB_PROFESSOR", "TB_DISCIPLINA"]

    cursor.execute(
    """
        CREATE TABLE IF NOT EXISTS TB_ALUNO(
            id_aluno INT PRIMARY KEY,
            nomeAluno VARCHAR(50) NOT NULL
        );
    """
    )

    cursor.execute(
    """
        CREATE TABLE IF NOT EXISTS TB_PROFESSOR(
            id_prof INT PRIMARY KEY,
            nomeProf VARCHAR(50) NOT NULL,
            disciplina VARCHAR(50) NOT NULL
        );
    """
    )

    cursor.execute(
    """
        CREATE TABLE IF NOT EXISTS TB_DISCIPLINA(
            id_disc INT PRIMARY KEY,
            nomeDisc VARCHAR(50) NOT NULL,
            horas INT NOT NULL
        );
    """
    )

    cursor.execute(
    """
        CREATE TABLE IF NOT EXISTS TB_MATRICULA(
            id_matricula INT PRIMARY KEY AUTO_INCREMENT,
            id_aluno INT,
            id_prof INT,
            id_disc INT,
            notaN1 DECIMAL(4,2),
            notaN2 DECIMAL(4,2),
            faltas INT,
            FOREIGN KEY (id_aluno) REFERENCES TB_ALUNO(id_aluno),
            FOREIGN KEY (id_prof) REFERENCES TB_PROFESSOR(id_prof),
            FOREIGN KEY (id_disc) REFERENCES TB_DISCIPLINA(id_disc)
        );
    """
    )

    connect.commit()

    for item in range(len(tabelas)):
    cursor.execute(f"DELETE FROM {tabelas[item]};")
    cursor.execute(f"ALTER TABLE {tabelas[item]} AUTO_INCREMENT = 1;")
    connect.commit()

    cursor.execute(
    """
        INSERT INTO TB_ALUNO(id_aluno, nomeAluno) 
        VALUES (1, "Lucas"), (2, "Sabrinna"), (3, "Vinicius")
        
    """
    )

    connect.commit()

    cursor.execute(
    """
        INSERT INTO TB_PROFESSOR(id_prof, nomeProf, disciplina) 
        VALUES (1, "Prof Alexandre", "Física"), 
               (2, "Prof Wendell", "Redes de Computadores"), 
               (3, "Prof Alisson", " MFDS")
        
    """
    )

    connect.commit()

    cursor.execute(
    """
    INSERT INTO TB_DISCIPLINA(id_disc, nomeDisc, horas) 
    VALUES (1, "Física", 80),
           (2, "Redes de Computadores", 60),
           (3, "MFDS", 90);
    """
    )

    connect.commit()

    cursor.execute(
    """
        INSERT INTO TB_MATRICULA(id_aluno, id_prof, id_disc, notaN1, notaN2, faltas) 
        VALUES (1, 1, 1, 6.0, 8.5, 8),
               (2, 2, 2, 6.0, 5.0, 9),
               (3, 3, 3, 8.0, 7.0, 15);
    """
    )

    connect.commit()

    cursor.execute(
    """
        SELECT A.nomeAluno, M.notaN1, M.notaN2, M.faltas
        FROM TB_MATRICULA M
        JOIN TB_ALUNO A ON M.id_aluno = A.id_aluno;
    """
    )

    matriculas = cursor.fetchall()

    for matricula in matriculas:
        nomeAluno, notaN1, notaN2, faltas = matricula
    if ((notaN1 + notaN2)/2) < 6:
        print(f"nome: {nomeAluno}, status: Reprovado por nota")
    elif faltas > 10:
        print(f"nome: {nomeAluno}, status: Reprovado por falta")
    else:
        print(f"nome: {nomeAluno}, status: Aprovado")


    cursor.close()
    connect.close()

  - coloque a senha **rootpassword**
  - fechando o vi file.py
    - pressione **esc**
    - pressione **:** (dois pontos)
    - digite **wq** e aperte **enter**
      - o que o **wq** faz?
        - ele salva o arquivo

- executando o script
  - faça **python main.py**
  - o resultado deverá retornar o que se pede na questao.
- saindo de sua conta
    - faça **docker logout**
