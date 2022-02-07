# HelloDeploy App to compile bootstrap SCSS styles

## Inicio

  Para esta app prueba se compilaran estilos scss utilizando la última version de bootstrap asi como compilar estilos que queramos añadir.

  Los pasos son los sigueintes.

## Instalar bootstrap

  Dentro de nuestra aplicación se tiene una carpeta llamada assets, nos colocaremos dentro de ella y posteriormente haremos la instalaión de bootstrap mediante NPM creando un archivo packege.json.

  ```
  $ cd assets
  $ npm init -y
  $ npm i bootstrap @popperjs/core
  ```

## Incluir bootstrap de los node_modules dentro de assets de la app

  Dentro de assets crearemos un archivo .scss llamado:  
  ``` 
  app.scss  
  ``` 
  En este nuevo archivo importaremos bootstrap de los node_modules con la siguiente linea: 

  ``` @import "../node_modules/bootstrap/scss/bootstrap"; ```

  Nota: Para modificar el archivo de estilos de bootstrap se hace desde este archivo que se indica en la ruta anterior, en el se hacen las importaciones de los archivos scss que utiliza.

## Incluir bootstrap en javascript para Esbuild

  Dentro de assets debemos de ir a al archivo app.js `/js/app.js`.
   ```
  $ cd js
   ```

  Una vez dentro de ` app.js `, hacemos la importación de bootstrap con la siguiente linea:
  ``` import 'bootstrap'; ```

## Agregar compilador de SASS en las dependencias Phoenix

  Añadimos la importacion en el archivo `mix.exs` de nuestro proyecto.

  ``` 
  def deps do
    [
      {:dart_sass, "~> 0.2", runtime: Mix.env() == :dev}
    ]
  end
  ```
### Añadir dependencia en la carpeta deps

  Una vez añadida la dependencia en el archivo `mix.exs` meteremos el siguiente comando para agregarla al proyecto:

  ``` 
  mix deps.get
  ``` 

## Configurar Dart Sass

  Este paso es para indicar que versión queremos usar en nuestra configuración del archivo `config/config.exs`

  Solo debemos agregar la siguiente linea de codigo con la ultima versión actualmente:
  ```
  config :dart_sass,
  version: "1.43.1",
  default: [
    args: ~w(css/app.scss ../priv/static/assets/app.css),
    cd: Path.expand("../assets", __DIR__)
  ]
  ```

## Agregar wtcher de Sass

  Debemos de agregar dentro del watcher de nuestra app en el archivo  `config/dev.exs` debajo del watcher de esbulid.

  ```
  watchers: [
    # Start the esbuild watcher by calling Esbuild.install_and_run(:default, args)
    esbuild: {Esbuild, :install_and_run, [:default, ~w(--sourcemap=inline --watch)]},
    sass: {DartSass, :install_and_run,  [:default, ~w(--embed-source-map --source-map-urls=absolute --watch)]}
  ]
  ```

## Creación de css en prodcción

  Finalmente debemos de modificar la mix task de "assets.deploy" dentro del archivo `mix.exs` en los aliases.
  ```
  "sass default --no-source-map --style=compressed"
  ```

  Agregando el comando que utilizara sass, la misk task quedaria de la siguiente manera:
  ```
  "assets.deploy": [
    "esbuild default --minify",
    "sass default --no-source-map --style=compressed",
    "phx.digest"
  ]
  ```

## Learn more

  * Official website: https://www.phoenixframework.org/
  * Guides: https://hexdocs.pm/phoenix/overview.html
  * Docs: https://hexdocs.pm/phoenix
  * Forum: https://elixirforum.com/c/phoenix-forum
  * Source: https://github.com/phoenixframework/phoenix
