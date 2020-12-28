## Help-Angular-Publish-IIS

Tutorial para publicar aplicação Angular em ambiente de produção utilizando o servidor do Windows IIS.



## Habilitar IIS 

Em **'Ativar ou desativar recursos do Windows'** habilitar **Serviços de Informação da Internet**



## Instalar URL Rewrite
Baixar e instalar o aplicativo no link abaixo:

[https://www.iis.net/downloads/microsoft/url-rewrite](https://www.iis.net/downloads/microsoft/url-rewrite)



## Criar Pasta e Aplicativo para o App no IIS

Criar uma pasta com um nome qualquer (vamos utilizar o nome '**ePortal**') no seguinte diretório:  
C:\inetpub\wwwroot\

Criar um aplicativo no IIS (vamos utilizar o mesmo nome da pasta, ou seja '**ePortal**'), referenciando a pasta criada anteriormente, com o pool de conexao default.



## Publicar aplicativo angular para produção

Publicar Angular com o seguinte comando:
```
ng build --prod --base-href /ePortal/
```

Obs: No comando referenciamos o nome do aplicativo (ePortal) criado no IIS. É importante que os nomes sejam iguais.

Copiar os arquivos gerados na pasta **dist** do App Angular para a pasta **ePortal** que criamos no diretorio C:\inetpub\wwwroot\ePortal\

Veja o arquivo **index.html**:
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>AngularApp</title>
  <base href="/ePortal/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  <link href="https://fonts.googleapis.com/css?family=Roboto:300,400,500&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
  <link rel="stylesheet" href="styles.6a7876cfbf785ec2ad45.css"></head>
  <body class="mat-typography">
  <app-root></app-root>
  <script src="runtime.acf0dec4155e77772545.js" defer></script><script src="polyfills.35a5ca1855eb057f016a.js" defer></script><script                         src="main.132213fa546f56e884ae.js" defer></script></body>
</html>
```

Perceba a existência da tag **base** referênciando o diretório **/ePortal/**. 



## Testar aplicação

Nessa altura a aplicação já pode ser acessada. Porém, ao pressionar a tecla **F5** a aplicação não identifica as rotas filhas. 

Para corrigir, adicione o seguinte arquivo na raíz da aplicação, junto ao **index.html**:
```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
<system.webServer>
  <rewrite>
    <rules>
      <rule name="Angular Routes" stopProcessing="true">
        <match url=".*" />
        <conditions logicalGrouping="MatchAll">
          <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
          <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
        </conditions>
        <action type="Rewrite" url="/ePortal/" />
      </rule>
    </rules>
  </rewrite>
</system.webServer>

</configuration>
```

O arquivo acima deve conter o seguinte nome:  
```
web.config
```

