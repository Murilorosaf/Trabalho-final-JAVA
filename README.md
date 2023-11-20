# Trabalho-final-JAVA

# JAVA SQLite Basics

**Java para Banco de Dados SQLite: Guia Simples**

---

**Documentação:**

A documentação é como um manual que explica como as coisas funcionam. No caso do Java e SQLite, você pode encontrar informações importantes sobre como usar o banco de dados em documentos online. Esses documentos são como guias que mostram o caminho certo.

Documentação do SQLite:

[SQLite Documentation](https://www.sqlite.org/docs.html)

Site utilizado para aprender como implementar o java com o banco de dados:

[SQlite Java - How To Use JDBC To Interact with SQLite](https://www.sqlitetutorial.net/sqlite-java/)

---

**Conexão do Banco de Dados SQLite usando o Driver do SQLite JDBC - JAVA:**

Conectar o Java ao SQLite é como ligar dois amigos por telefone. Para fazer isso, usamos algo chamado "Driver JDBC", que é como um tradutor que ajuda o Java a falar com o SQLite. Dessa forma, as informações podem ser compartilhadas entre eles.

Abaixo segue o passo a passo de como fazer a conexão:

1. Baixar o Driver JDBC do Sqlite
    
    Link da pagina: [https://mvnrepository.com/artifact/org.xerial/sqlite-jdbc](https://mvnrepository.com/artifact/org.xerial/sqlite-jdbc)
    
2. Agora depois de baixado coloque esse arquivo no mesmo diretório da pasta que for ficar o código em java ou que fique de fácil acesso à pasta.
3. Após crie um arquivo em java chamado `Connect.java` e cole esse código
    
    ```java
    package net.sqlitetutorial;
    
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.SQLException;
    
    /**
     *
     * @author sqlitetutorial.net
     */
    public class Connect {
         /**
         * Connect to a sample database
         */
        public static void connect() {
            Connection conn = null;
            try {
                // db parameters
                String url = "jdbc:sqlite:C:/sqlite/db/NOME_DO_BANCO_SQLITE.db";
                // create a connection to the database
                conn = DriverManager.getConnection(url);
                
                System.out.println("Connection to SQLite has been established.");
                
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            } finally {
                try {
                    if (conn != null) {
                        conn.close();
                    }
                } catch (SQLException ex) {
                    System.out.println(ex.getMessage());
                }
            }
        }
        /**
         * @param args the command line arguments
         */
        public static void main(String[] args) {
            connect();
        }
    }
    ```
    
4. Para roda esse código, primeiro compile o arquivo com o comando:
    
     
    
    ```bash
    javac *.java
    ```
    
    E após esse comando aqui:
    
    ```bash
    java -classpath ".;sqlite-jdbc-3.43.0.0.jar" net.sqlitetutorial.Connect
    
    Obs: se você colocou o driver em um diretorio diferente tera que seguir até o local onde fica o arquivo.
    ```
    
5. No final é para dar esse resultado (se não ocorrer erros):
    
    ```bash
    Connection to SQLite has been established.
    ```
    

---

### Solução de problemas

Se você receber a seguinte mensagem, você deve ter perdido a etapa 8:

```
Error: Could not find or load main class net.sqlitetutorial.ConnectLinguagem de código:Sessão Shell(shell)
```

### Como funciona o programa

No `connect()`método:

Primeiro, declare uma variável que contém uma string de conexão ao banco de dados sqlite`c:\sqlite\db\chinook.db`

```java
String url = "jdbc:sqlite:C:/sqlite/db/chinook.db";Linguagem de código:Java(java)
```

A seguir, use a `DriverManager`classe para obter uma conexão de banco de dados com base na string de conexão.

```java
conn = DriverManager.getConnection(url);Linguagem de código:Java(java)
```

Em seguida, intercepte qualquer um `SQLException`  no `try catch`bloco e exiba a mensagem de erro.

Depois disso, feche a conexão com o banco de dados no `finally`bloco.

Finalmente, chame o `connect()`método no `main()`método da `Connect`classe.

---

**Criação de Tabelas:**

Imagine uma tabela como uma planilha onde você organiza suas informações. No Java, criar uma tabela no SQLite é como fazer uma tabela na planilha. Você diz ao programa quais são as colunas (informações) que você deseja e o tipo de informação que elas vão armazenar.

1. Primeiro, prepare uma `[CREATE TABLE](https://www.sqlitetutorial.net/sqlite-create-table/)`
    
    declaração para criar a tabela desejada.
    
2. Segundo, conecte-se ao banco de dados
    
    .
    
3. Terceiro, crie uma nova instância da `StatementConnection`
    
    classe a partir de um
    
    objeto.
    
4. Quarto, execute a `CREATE TABLEexecuteUpdate()Statement`
    
    instrução chamando o
    
    método do
    
    objeto.
    

```java
O programa a seguir ilustra as etapas de criação de uma tabela.

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

/**
*

- @author [sqlitetutorial.net](http://sqlitetutorial.net/)
*/
public class Main {
    
    
    
    - Create a new table in the test database
    - 
    - /
    public static void createNewTable() {
    // SQLite connection string
    String url = "jdbc:sqlite:C://sqlite/db/tests.db";

 // SQL statement for creating a new table
 String sql = "CREATE TABLE IF NOT EXISTS warehouses (\\n"
         + "	id integer PRIMARY KEY,\\n"
         + "	name text NOT NULL,\\n"
         + "	capacity real\\n"
         + ");";

 try (Connection conn = DriverManager.getConnection(url);
         Statement stmt = conn.createStatement()) {
     // create a new table
     stmt.execute(sql);
 } catch (SQLException e) {
     System.out.println(e.getMessage());
 }

/**

- @param args the command line arguments
*/
public static void main(String[] args) {
createNewTable();
}

}
```

---

**Inserção de Dados na Tabela:**

Depois de criar a tabela, é hora de adicionar informações a ela. Isso é como preencher os espaços da sua planilha. Você diz ao programa quais valores deseja adicionar em cada coluna e ele os coloca no lugar certo.

1. Primeiro, conectar-se ao banco de dados através do `Connection`
    
    .
    
2. Em seguida, prepare o `INSERT`. Se você usar parâmetros para a instrução, use um ponto de interrogação (?) para cada parâmetro.
3. Em seguida, crie uma instância do `PreparedStatementConnection`
    
    objeto
    
    .
    
4. Depois disso, defina os valores correspondentes para cada espaço reservado usando o método set do `PreparedStatement` como: `setInt()`, `setString()`, etc.
5. Finalmente, chame o `executeUpdate()` do método `PreparedStatement`

Segue o código abaixo:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

/**
 *
 * @author sqlitetutorial.net
 */
public class InsertApp {

    /**
     * Connect to the test.db database
     *
     * @return the Connection object
     */
    private Connection connect() {
        // SQLite connection string
        String url = "jdbc:sqlite:C://sqlite/db/NOME_DO_BANCO.db";
        Connection conn = null;
        try {
            conn = DriverManager.getConnection(url);
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
        return conn;
    }

    /**
     * Insert a new row into the warehouses table
     *
     * @param name
     * @param capacity
     */
    public void insert(String name, double capacity) {
        String sql = "INSERT INTO warehouses(name,capacity) VALUES(?,?)";

        try (Connection conn = this.connect();
                PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, name);
            pstmt.setDouble(2, capacity);
            pstmt.executeUpdate();
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {

        InsertApp app = new InsertApp();
        // insert three new rows
        app.insert("Raw Materials", 3000);
        app.insert("Semifinished Goods", 4000);
        app.insert("Finished Goods", 5000);
    }

}
```

---

**Consultas na Tabela:**

Consultar a tabela é como procurar algo específico na sua planilha. Você pode pedir ao programa para mostrar apenas as informações que você precisa. Por exemplo, se você tem uma lista de nomes, pode pedir para ver apenas os nomes que começam com a letra "A".

1. Primeiro vamos criar um método `Connection` que fará a função de se conectar com o banco de dados
2. Após criaremos um novo método chamado `SelectApp` que ira fazer a consulta no Banco de Dados.
3. Dentro desse método `SelectApp` iremos criar uma instância do `Statement` e do `ResultSet`.
4. A instância do `ResultSet` ira executar uma variável que ali conterá o código em SQL que irá fazer a consulta.
5. Depois disso, percorra o conjunto de resultados usando o `next()`método do `ResultSet`objeto.
6. Por fim, use o método get* do `ResultSet`objeto, como `getInt()`, `getString()`, `getDouble()`, etc., para obter os dados em cada iteração.

---

Siga o código abaixo:

```java
import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/**
 *
 * @author sqlitetutorial.net
 */
public class SelectApp {

    /**
     * Connect to the test.db database
     * @return the Connection object
     */
    private Connection connect() {
        // SQLite connection string
        String url = "jdbc:sqlite:C://sqlite/db/NOME_DO_BANCO.db";
        Connection conn = null;
        try {
            conn = DriverManager.getConnection(url);
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
        return conn;
    }

    
    /**
     * select all rows in the warehouses table
     */
    public void selectAll(){
        String sql = "SELECT coluna, coluna, coluna FROM nome_da_tabela";
        
        try (Connection conn = this.connect();
             Statement stmt  = conn.createStatement();
             ResultSet rs    = stmt.executeQuery(sql)){
            
            // loop through the result set
            while (rs.next()) {
                System.out.println(rs.getInt("coluna") +  "\t" + 
                                   rs.getString("coluna") + "\t" +
                                   rs.getDouble("coluna"));
            }
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }
    
   
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        SelectApp app = new SelectApp();
        app.selectAll();
    }

}
```

---

**Exclusão de Dados na Tabela:**

Às vezes, você precisa remover algo da sua planilha. Da mesma forma, no Java e SQLite, você pode excluir informações da tabela. Se algo não é mais necessário, você diz ao programa para removê-lo, como tirar um nome da lista.

1. Primeiro, conecte com o banco com o `Connection`.
2. Em seguida, prepare a instrução `DELETE`. Caso queira que a instrução receba parâmetros, use o espaço reservado de ponto de interrogação (?) dentro da instrução.
3. Em seguida, crie uma nova instância da   `PreparedStatement` classe chamando o `prepareStatement()`método do `Connection`objeto.
4. Depois disso, forneça valores no lugar do espaço reservado para ponto de interrogação usando o método set* do  `PreparedStatement`objeto, por exemplo,  `setInt()`, `setString()`, etc.
5. Finalmente, execute a `DELETE` instrução chamando o `executeUpdate()`método do `PreparedStatement`objeto.

Segue o código abaixo:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

/**
 *
 * @author sqlitetutorial.net
 */
public class DeleteApp {

    /**
     * Connect to the test.db database
     *
     * @return the Connection object
     */
    private Connection connect() {
        // SQLite connection string
        String url = "jdbc:sqlite:C://sqlite/db/test.db";
        Connection conn = null;
        try {
            conn = DriverManager.getConnection(url);
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
        return conn;
    }

    /**
     * Delete a warehouse specified by the id
     *
     * @param id
     */
    public void delete(int id) {
        String sql = "DELETE FROM warehouses WHERE id = ?";

        try (Connection conn = this.connect();
                PreparedStatement pstmt = conn.prepareStatement(sql)) {

            // set the corresponding param
            pstmt.setInt(1, id);
            // execute the delete statement
            pstmt.executeUpdate();

        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        DeleteApp app = new DeleteApp();
        // delete the row with id 3
        app.delete(3);
    }

}
```

---

**Banco de Dados SQLite:**

O banco de dados SQLite é como um arquivo especial onde todas as informações são armazenadas. É como um arquivo grande onde o programa guarda todas as tabelas e dados que você criou. Assim, quando você precisa, o programa pode acessar essas informações rapidamente.

- **Explicação**: SQLite é uma biblioteca de software que fornece um sistema de gerenciamento de banco de dados relacional. O lite no SQLite significa leve em termos de configuração, administração de banco de dados e recursos necessários.

---

1. Como baixar o Sqlite: 
    
    [Instalação do Sqlite](https://www.notion.so/Instala-o-do-Sqlite-c2a2552edd494642a89166c59a9c6f01?pvs=21)
    
2. link para todos os conhecimentos relacionados a banco de dados em Sqlite: 

---
