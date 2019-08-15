# Trabalho de Conclusão de Curso
* **Documentação dos códigos e estruturas**
* **_Desenvolvido com modelo MVC para melhor organização dos códigos._**

## Observações:
* Routes definidas automaticamente pelo 'Core.php'
* Model com CRUD pré montado
* Classe para Session já inclusa
* Classe para Cookies já inclusa
* Os arquivos para funcionamento da estrutura estão em 'core/' e não devem ser alterados

## Regras para funcionamento correto
* Não alterar arquivos dentro da pasta 'core/'
* Não alterar 'index.php'
* Não mexer em '.htaccess' a menos que esteja fazendo instalação em ambiente personalizado

## Regras para instalação do código
* Definir status do sistema em 'environment.php'
* Configurar variaveis em 'config.php'
* Para desenvolvimento local instalar no endereço 'localhost/TCC/'
* Criar database com o mesmo nome definido em 'config.php'

## Entendendo o Core() e sistema de Routes
* As rotas são definidas por dados enviados na URL  e capturados por GET

#### Veja o exemplo abaixo:
```
https://meusite.com.br/home/buscar/10
```
* Nesse exemplo está sendo chamado o controller 'home', function 'buscar' e enviando o '10' como parâmetro

## Como utilizar Controllers
* O nome deve ser seguido de 'Controller'
* Ele deve herdar caracteristicas da classe 'Controller'
* Quando um Controller definido por URL não existir, será redirecionado para 'notFoundController'
* Se um Controller existir mas a função não, será redirecionado para o método 'index()'

#### Veja exemplo abaixo
```
<?php
/*
*Controller padrão do sistema
*/
class homeController extends Controller {

    public function index() {
        $data = [

        ];

        $this->loadTemplate('templatename', 'viewname', $data);
    }

}
```

## Funções nativas do Controller
* Algumas funções já vem prontas para serem utilizadas e reduzir códigos presentes nela

#### A function 'loadView()' serve para chamar a view informada e transformar as chaves do array enviado em variaveis que carregam o valor da chave
```
$data = [
            
];
$this->loadView('viewname', $data);
```

#### A function 'loadTemplate()' carrega a view informada dentro de um template informado
```
$data = [

];
$this->loadTemplate('templatename', 'viewname', $data);
```

#### A function 'loadComponent()' deve ser executada dentro da view e carrega o arquivo com extensão '.php' q está dentro da pasta 'views/components/' e com o nome informado e com possibilidade de enviar parâmetros
```
<?php
//sem parâmetros
$this->loadComponent('componentname');

//com parâmetros
$this->loadComponent('componentname', $dados);
?>
```

#### A function 'verificador()' busca um arquivo em 'views/verificadores/' e envia o parâmetro para alguma verificação
* Deve sempre ser declarada no topo da view
```
<?php
$this->verificador('verificadorname', $dados);
?>
```

## CRUD do sistema
* Dentro da estrutura não devem ser digitadas códigos sql
* Criar um Model para cada tabela do seu sistema e nunca chamar a classe Model diretamente
* Dentro do construtor deve ser informado qual o nome da tabela

#### Definir da seguinte forma:
```
<?php
class User extends Model {

    public function __construct() {
        parent::__construct("users");
    }    
}
```

#### Select com Model
* O método 'exec()' deve sempre ser chamado para a query ser executada
```
<?php
$user = new User();
$buscaSemId = $user->find()->exec();

$buscaComId = $user->find("id = {$id}")->exec();
>
``` 
* O retorno caso seja não haja resultados é um 'false' ou um array com os dados

#### Utilizando Order e Limit
```
<?php
$user = new User();
$buscaComLimit = $user->find()->limit('5')->exec();

$buscaComOrder = $user->find()->order('id DESC')->exec();

$buscaComOsDois = $user->find()->order('id DESC')->limit('5')->exec();
>
``` 

#### Insert com Model
* No array enviado, as chaves devem ser os campos da tabela e seus valores o valor a ser preenchido
* O método sempre retorna um 'true'
```
<?php
$user = new User();
$dados = [
    'nome' => 'Nome do Usuário',
    'email' => 'Email de testes',
    'senha' => '123'
]
$user->create($dados);
>
```

#### Update com Model
* No array enviado, as chaves devem ser os campos da tabela e seus valores o valor a ser alterado
* Deve ser enviada uma condição para não ocorrerem erros
* O método sempre retorna um 'true'
```
<?php
$user = new User();
$dados = [
    'nome' => 'Novo nome',
    'senha' => '123456'
]
$user->update($dados, "id = {$id}");
>
```

#### Delete com Model
* Semelhante ao Select
* Deve ser enviado apenas a condição
* Pela semelhança ao Select, os métodos Order e Limit podem ser utilizados com o Delete
* Deve ser finalizado com um 'destroy()'
* Retorna um 'true'
```
<?php
$user = new User();
$user->delete("id = {$id}")->destroy();

$user->delete("id = {$id}")->limit('5')->destroy();
>
``` 

## Trabalhando com SESSION
* Não definir diretamente a variavel '$_SESSION'
* Não rodar o comando 'session_start()' em nenhuma local do sistema
* Usar a variavel global '$session' para aplicar os comando

#### Colocando valor na SESSION:
```
global $session;
$session->set('dados', $login);
```

#### Puxando valor da SESSION:
```
global $session;
$response = $session->get('dados');
```

#### Verificando se valor existe:
```
global $session;
$response = $session->vazio('dados');
```

#### Esvaziando valor da SESSION:
```
global $session;
$session->esvaziar('dados');
```

#### Destruindo a SESSION:
```
global $session;
$session->destroy();
```

## Trabalhando com COOKIES
* Não utilizar diretamente os comandos para cookies no sistema
* Chamar global '$cookies' para trabalhar com eles
* Tempo padrão para 1 ano ou 1 mês de vida
* O tempo pode ser personalizado
* Caso não seja informado o time 'year', 'month' ou personalizado o tempo será automaticamente de 1 ano

#### Puxando um valor em Cookie:
```
global $cookies;
$data = [
    'error' => $error,
    'inputvalue' => $cookies->get('emailLogin')
];
```

#### Criando um Cookie:
```
global $cookies;
$cookie->set('emailLogin', $email);
```

#### Criando um Cookie com tempo personalizado:
* O tempo deve estar em milisegundos
```
global $cookies;
$cookie->set('emailLogin', $email, $time);
```

#### Atualizando um Cookie:
```
global $cookies;
$cookie->update('emailLogin', $email);
```

#### Destruindo um Cookie:
```
global $cookies;
$cookie->del('emailLogin');
```
