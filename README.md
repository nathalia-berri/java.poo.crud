Criando tabela no Banco de Dados XAMPP MySQL:
mysql -u root
CREATE DATABASE hardwaredb;
USE hardwaredb;
CREATE TABLE componentes_hardware (
codigo INT PRIMARY KEY,
nome VARCHAR(255) NOT NULL,
descricao TEXT);

Principais Funções das Classes:
HardwareComponente: Classe abstrata que serve como modelo para componentes de hardware.
MotherComponente e OtherComponente: Subclasses que representam os tipos de hardware com prefixos específicos.
ConexaoMySQL: Classe de conexão ao banco, usada por HardwareCRUD.
HardwareCRUD: Classe responsável pelo CRUD diretamente no banco de dados.
Executora: Classe que executa e testa operações CRUD, conectando-se ao banco de dados através de HardwareCRUD.

Classes e Seus Papéis
HardwareComponente (Classe Abstrata)
Representa a base para todos os componentes de hardware.
Atributos: codigo, nome, descricao.
Contém um método abstrato aplicarPrefixo(), que é implementado nas subclasses.
MotherComponente (Classe Concreta)
Extende HardwareComponente.
No construtor, chama aplicarPrefixo(), que adiciona o prefixo "mot_" ao nome.
OtherComponente (Classe Concreta)
Também extende HardwareComponente.
Aplica o prefixo "oth_" ao nome no seu construtor.
ConexaoMySQL
Gerencia a conexão com o banco de dados MySQL.
O método getConexaoMySQL() estabelece a conexão e retorna um objeto Connection.
HardwareCRUD
Implementa os métodos para operações CRUD:
create(HardwareComponente componente): Insere um novo componente na tabela componentes_hardware.
read(): Busca e exibe todos os componentes armazenados.
update(int codigo, String novoNome, String novaDescricao): Atualiza o nome e a descrição de um componente existente.
delete(int codigo): Remove um componente com base em seu código.
Usa PreparedStatement para prevenir SQL Injection e melhorar a eficiência das consultas.
Executora
Contém o método main, onde a execução da aplicação começa.
Instancia HardwareCRUD e um Scanner para interagir com o usuário.
Apresenta um menu para o usuário realizar operações CRUD nos componentes de hardware.

Extensão de comunicação entre Java e Banco de Dados
Precisou ser instalado o MySQL Connector/J 9.1.0 porque ele é o driver JDBC que permite que a aplicação Java se conecte a um banco de dados MySQL. 
JDBC (Java Database Connectivity) é a API usada em Java para interagir com bancos de dados. O MySQL Connector/J age como uma "ponte" entre a sua aplicação Java e o servidor MySQL, permitindo que sejam enviados os comandos SQL e receba resultados.

Fluxo da Aplicação
Inicialização
O método main da classe Executora inicia e instancia HardwareCRUD, que estabelece a conexão com o banco de dados usando ConexaoMySQL.
Interação com o Usuário
Um menu é apresentado ao usuário, permitindo que ele escolha entre cadastrar, listar, atualizar ou excluir componentes.
Criação de Componentes
Quando o usuário opta por cadastrar um componente:
Um objeto de MotherComponente ou OtherComponente é criado, com o prefixo apropriado no nome.
O método create da classe HardwareCRUD é chamado, inserindo o componente no banco de dados. O PreparedStatement é usado para executar a consulta SQL.
Leitura de Componentes
Ao escolher listar componentes, o método read é chamado. Este método executa uma consulta para buscar todos os componentes e imprime os resultados.
Atualização de Componentes
O usuário informa o código do componente que deseja atualizar.
O novo nome e descrição são coletados e passados ao método update do HardwareCRUD, que executa a atualização no banco de dados.
Exclusão de Componentes
O usuário informa o código do componente a ser excluído, e o método delete é chamado. O PreparedStatement é usado novamente para remover o componente do banco de dados.
Saída
O usuário pode sair do programa selecionando a opção correspondente no menu.
