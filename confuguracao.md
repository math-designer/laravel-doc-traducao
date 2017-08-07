# Configuração
  * [Introdução]()
  * [Configuração de Ambiente]()
  	* [Obtendo Configurações de Ambiente]()
  	* [Definido o Ambiente Atual]()
  * [Acessando os Valores de Configuração]()
  * [Cache de Configuração]()
  * [Modo de Manutenção]()

#Introdução
Todos os arquivo de configuração do Laravel estão armazenados na pasta `config`. Cada opção é documentada, então sinta-se a vontade para olhar os arquivos e se familirizar com as opções disponíveis.

#Configuração de Ambiente
As vezes é útil ter configurações diferentes de acordo com o ambiente onde a aplicação está rodando. Por exemplo, você pode quer utilizar localmente um driver de Cache diferente do que você utiliza no servidor de produção.

Para faciliar, o Laravel utiliza o biblioteca PHP [DotEnv](https://github.com/vlucas/phpdotenv). Numa instalação nova do Laravel, o diretório raiz da sua aplicação contém o arquivo `.env.example`. Se você instalar o Laravel via Composer, este arquivo será automaticamente renomeado para `.env`. Caso contrário, você deve renomear este arquivo manualmente.

Seu arquivo `.env` não deve ser comitado para o controle de versão, desde que cada desenvolvedor/servidor que utilizam sua aplicação podem utilizar uma configuração de ambiente diferente. Além disso, isso seria um risco de segurança caso um intruso obtesse acesso ao seu repositório, já que qualquer credencial estaria exposta.

Se você esta desenvolvendo com um time, você pode continuar incluindo o arquivo `.env.example` com a sua aplicação. Colocado valores iniciais no arquivo exemplo de configuração, outros desenvolvedores do seu time podem claramente ver quais variáveis são necessárias para rodar sua aplicação. Você também pode criar um arquivo `.env.testing`. Este arquivo irá sobreescrever os valores so seu `.env` quando rodando testes com o PHPUnit ou executrando comandos do Artisan com a opção `--env=testing`.


> Qualquer variável do seu arquivo `.env` pode ser subistituído por varáveis de ambiente externas como váríaveis em nivel de sistema ou em nível de servidor.



**Obtendo Configurações de Ambiente**
Todas as variáveis listadas neste arquivo serão carregadas através da variável super global `$_ENV` quando sua aplicação receber uma requisição. Entretando, você pode usar o helper `env` para obter valores das variáveis do seu arquivo de configuração. De fato, se você olhar os arquivos de configuração do Laravel, você verá diversas opções que utilizam este helper.

```php
'debug' => env('APP_DEBUG', false),
```

O segundo valor passado para a função `env` é o "valor padrão". Este valor será usando caso nenhuma variável de ambiente existir para a chave informada.



**Definindo o Ambiente Atual**
O ambiente atual da aplicação é definida pela variável `APP_ENV` no arquivo `.env`. Você pode acessar este valor pelo método `environment` da [facade](https://laravel.com/docs/5.4/facades) `App`:


```php
$environment = App::environment();
```

Você pode verificar qual o ambiente esta definido, passando o nome como argumento para o método `environment`. O método irá retornar `true` se o ambiente for correspondente com o valor informado:


```php
if (App::environment('local')) {
    // The environment is local
}

if (App::environment(['local', 'staging'])) {
    // The environment is either local OR staging...
}
```

#Acessando os Valores de Configuração
Você pode acessar de forma fácil os valores das configurações através do helper `config` de qualquer lugar da aplicação. Os valores das configurações são usando utilizando a sintaxe 'ponto', que inclui o nome do arquivo e a opção que você quer acessar. Um valor padrão também pode ser informado caso a opção não exista:

```php
$value = config('app.timezone')
```

Para definir a configuração em tempo de execução passe um array para o help `config`

```php
config(['app.timezone' => 'America/Chicago'])
```


#Cache de Configuração
Para dar à sua aplicação uma melhor performasse, você deve fazer o cache de todas as suas configurações em um único arquivo usando o comando Artisan `config:cache`. Ele irá combinar todas as opções de configuração da sua aplicação dentro de um único arquivo que será rapidamente carregado pelo framework.

Você deve tomar como rotina executar o comando `php artisan cache:config` no deploy para produção. Este comando não deve ser rodado durante o desenvolvimento, uma vez que as pode frequentemente sofrer modificações, no decorrer do desenvolvimento da sua aplicação.

#Modo de Manutenção
Quando sua aplicação estiver em manutenção, uma página customizada será mostrada para todas as requisições feitas na sua aplicação. Isso torna fácil "desativar" sua aplicação enquanto você está atualizando ou quando você esta realizando alguma manutenção. Uma verificação do modo de manutenção é incluída na pilha de middlewares padrão da sua aplicação. Quando o modo de manutenção estiver ativado, uma `MaintenanceModeException` será lançada com um status HTTP 503.

Para ativar o modo de manutenção, execute o comando `down` do Artisan.

```
php artisan down
```

Você também pode informar as opções `message` e `retry` para o comando `down`. O valor de `message` pode ser utilizada para ser exibida ou criar um log customizado, enquanto que o valor de `retry` será definido como cabeçalho HTTP `Retry-After`:

```
php artisan down --message="Atualizando Banco de Dados" --retry=60
```

Para desativar o modo de manutenção, use o comando `up`:

```
php artisan up
```

> Você pode customizar o template padrão do modo de manutenção, criando seu template em `resources/views/errors/503.blade.php`


**Modo Manutenção e Filas**
Enquanto sua aplicação estiver no modo de manutenção, nenhuma [fila de trabalho](https://laravel.com/docs/5.4/queues) será carregada. Os trabalhos serão carregados normalmente quando o modo de manutenção estiver desativado.

**Modo de Manutenção Alternativo**
Uma vez que o modo de manutenção demora alguns segundos para ser ativado, considere uma alternativa como [Envoyer](https://envoyer.io/) para realiza sem demora seu deploy com o Laravel.