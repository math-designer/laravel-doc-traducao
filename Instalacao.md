## Instalação
  * [Requisitos do Servidor]()
  * [Instalando Laravel]()
  * [Configuração]()

## Configuração Servidor Web
  * [URLs amigáveis]()
  

# Instalação
### Requisitos do Servidor
O framework Laravel possui alguns requisitos de sistema. Claro, que todos estes requisitos são satisfeitos pela máquina virtual [Laravel Homestead](https://laravel.com/docs/5.4/homestead). Então é altamente recomendado que você use o Homestead como seu ambiente de desenvolvimento.

Entretanto, se você não estiver usando o Homestead, você precisa ter certeza que seu servidor atenda os seguintes requisitos:
  * PHP >= 5.6.4
  * Extensão PHP OpenSSL
  * Extensão PHP PDO
  * Extensão PHP Mbstring
  * Extensão PHP Tokenizer
  * Extensão PHP XML
 
### Instalando Laravel
Laravel utiliza o [Composer](https://getcomposer.org/) para gerenciar suas dependências. Então, antes de utilizar o Laravel, certifique-se de ter o Composer intalando na sua máquina.

**Via instalador Laravel**
Primeiro, baixe o intalador Laravel usando o Composer:

```php
composer global require "laravel/installer"
```

Certifique-se de colocar o diretório `$HOME/.composer/vendor/bin` (ou o diretório equivalente ao seu SO) em sua variável de ambiente, então o executavel `laravel`poderá ser localizado pelo seu sistema.

Uma vez instalado, o comando `laravel new` irá criar uma instalação nova do Laravel no diretório especificado. Por exemplo, `laravel new blog` irá criar o diretório `blog` contendo a nova instalação do Laravel com todas as suas dependências do já instaladas.

```php
laravel new blog
```

**Via Composer Create-Project**
Alternativamente, você também pode instalar o Laravel através do comando `create-project` do Composer no seu terminal:

`composer create-project --prefer-dist laravel/laravel blog`

**Servidor Local de Desenvovimento**
Se você tiver o PHP instalado localmente e gostaria de utilizar o servidor de desenvolvimento embutido do PHP para rodar sua aplicação, você pode utilizar o comando `serve` do Artisan. Este comando ira iniciar um servidor de desenvolvimento em `http://localhost:8000`:

`php artisan serve`

Claro, opções mais robustas estão disponíveis via [`Homestead`](https://laravel.com/docs/5.4/homestead) e [`Valet`](https://laravel.com/docs/5.4/valet).

### Configuração
**Diretório Público**
Depois de instalado o Laravel, você deve configurar a pasta raiz do seu servidor web para ser o diretório `public`. O arquivo `index.php` dentro deste diretório serve como um controlador inicial para todas as requisições HTTP que entram na sua aplicação.

**Arquivos de Configuração**
Todos os arquivos de configuração do Laravel estão dentro do diretório `config`. Cada opção é documentada, então sinta-se a vontade para olhar os arquivos e se familiarizar com as opções disponíveis.

**Permissões dos Diretórios**
Depois de instalar o Laravel, você pode configurar algumas permissões. Diretórios dentro de `storage` e `bootstrap/cache` devem ser graváveis pelo seu servidor web ou o Laravel não irá rodar. Se você estiver usando a máquina virtual `Homestead`, estas permissões já deverão estar definidas.

**Chave da Aplicação**
A próxima coisa que você deve fazer após a instalação do Laravel é definir a chave da sua aplicação para uma string aleatória. Se você instalou o Laravel pelo Composer ou pelo instalador Laravel, esta chave já estará definida pra você pelo comando `php artisan key:generate`

Tipicamente, esta string deve ter o tamanho de 32 caracteres. A chave pode ser definida no arquivo de ambiente `.env`. Se você não tiver renomeado o arquivo `.env.example` para `.env`, você deve fazer isso agora. **Se a chave da aplicação não  estiver definida, sua sessão de usuário e outros dados criptografados não estarão seguros!**

**Configurações Adicionais**
Laravel quase não precisa de outra configuração fora da caixa. Você é livre para começar a desenvolver! Entretanto, você pode querer rever o arquivo `config/app.php` e sua documentação. Ela contém diversas opções como `timezone` e `locale` que você pode querer mudar de acordo com a sua aplicação.

Você também pode querer configurar alguns componentes adicionais do Laravel, tais como:

   * [Cache]()
   * [Banco de Dados]()
   * [Sessão]()


# Configuração Servidor Web
### URLs amigáveis

**Apache**
Laravel inclui o arquivo `public/.htaccess` que é utilizado para prover URLs sem o controlador inicial `index.php` no caminho. Antes de servir o Laravel com Apache, certifique-se de habilitar o módulo `mod_rewrite`, então o arquivo `.htaccess` será analisado pelo servidor.

Se o arquivo `.htaccess` que vem com o Laravel não funcionar com sua instalação do Apache, tente esta alternativa:

```
Options +FollowSymLinks
RewriteEngine On

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
```

**Nginx**

Se você esta usando o Nginx, a seguinte diretiva na configuração do seu site irá direcionar todas as requisições para o controlador inicial `index.php`:

```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```

Claro, quando utilizando o `Homestead` ou `Valet`, URL amigáveis serão automaticamente configuradas.
