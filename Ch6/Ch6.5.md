# Scripting GitHub

Ya que se vio todas las opciones más grandes de GitHub, veremos como podemos usar los sistemas `hook` y los `API` de GitHub a nuestro favor

## Servicios y Hooks
Esta es la manera más facil de que GitHub interactue con los sistemas exteriores

### Servicios
Estos pueden ser encontrados en ajustes, en la sección de `webhooks and services`, ahi encontraras una gran variedad de servicios que puedes elegir, la mayoria de ellos son servicios de integración continua, como `bug and issue trackers`, `chat rooms`, etc. Si eliges `email` encontraras una pestaña que te permitira configurar un correo, esto agregara ese correo para las notificaciones.
Si estas usando algún sistema que querrias integrar a GitHub deberias revisar esta sección para ver si se encuentra.

### Hooks
Si necesitas algo más especifico o si quieres integrar algún servicio que no se encuentra en la lista, puedes usar el sistema de `hooks`, solo tienes que proporcioner un URL y GitHub proporcianara un `HTTP payload`, Generalmente la manera en la que esto funciona es la siguiente, configuras un pequeño servicio web que escuche un `Hook payload` de GitHub y haga algo con la información recibida, para establecer un `hook` en la sección de ajustes mencionada anterior mente, das click en `add webhooks`, esto abrira una pestaña de configuración, generalmente, esto es muy simple, solo añades el URL y lo demas es estandar, por defecto viene que la unica opción de obtener un `payload` es por un `push`.
Para obtener más información sobre como escribir `webhooks` y consejos puedes revisar https://docs.github.com/en/webhooks-and-events/webhooks/about-webhooks.

## API de GitHub
Los servicios y `Hooks` son utiles para ser notificado por `pushes` pero, si quieres obtener otro tipo de información o automatizar eventos y acciones, Para eso sirven las `API`

### Uso Basico
La cosa más basica que puedes hacer con las `API` es un pedido `GET` en un `endpoint`, esto no requieres autentificación, por ejemplo, si queremos obtener la información del usuario llamada `schacon` podemos usar algo como esto
```
    $ curl https://api.github.com/users/schacon
    {
    "login": "schacon",
    "id": 70,
    "avatar_url": "https://avatars.githubusercontent.com/u/70",
    # …
    "name": "Scott Chacon",
    "company": "GitHub",
    "following": 19,
    "created_at": "2008-01-27T17:19:28Z",
    "updated_at": "2014-06-10T02:37:23Z"
    }
```
hay bastantes `endpoints` a los que podemos accesar como, `proyectos` `commits` `issues`, etc. Incluso puedes usar el `API` para hacer `render` arbitrariamente a markdown o buscar un `.gitignore`
```
    $ curl https://api.github.com/gitignore/templates/Java
    {
    "name": "Java",
    "source": "*.class

    # Mobile Tools for Java (J2ME)
    .mtj.tmp/

    # Package Files #
    *.jar
    *.war
    *.ear

    # virtual machine crash logs, see https://www.java.com/en/download/help/error_hotspot.xml
    hs_err_pid*
    "
    }
```

### Comentar en un issue
En cambio, si quieres hacer una acción en el sitio web, tal como comentar en un `issue` o un `pull request` o si quieres interactuar con contenido privado, necesitaras realizar una autentificación.
Hay varias formas de autentificarte, la más basica es con tu usuario y contraseña, pero generalmente es mejor conseguir un token personal, este lo puedes generar en ajustes en la sección de aplicaciones.
Te va a pedir los alcances del token y una descripción, asegurate de usar una buena descripción para que te sientas comoda brrandolo cuando no uses más el `script`.
GitHub solo mostrar el token una vez, asi que asegurate de copiarlo, esto ayuda a que limitas el acceso a lo que necesita el `script` y puede eliminarse, esto tambien aumenta la cantidad de acciones que puedes hacer, pasas de 60 por hora a 5000.
En un ejemplo, vamos a comentar en el `issue#6` para hacer esto, tenemos que hacer un `HTTP POST request` en `repos/<user>/<repo>/issues/<num>/comments` con el token que acabamos de crear.
```
    $ curl -H "Content-Type: application/json" \
        -H "Authorization: token TOKEN" \
        --data '{"body":"A new comment, :+1:"}' \
        https://api.github.com/repos/schacon/blink/issues/6/comments
    {
    "id": 58322100,
    "html_url": "https://github.com/schacon/blink/issues/6#issuecomment-58322100",
    ...
    "user": {
        "login": "tonychacon",
        "id": 7874698,
        "avatar_url": "https://avatars.githubusercontent.com/u/7874698?v=2",
        "type": "User",
    },
    "created_at": "2014-10-08T07:48:19Z",
    "updated_at": "2014-10-08T07:48:19Z",
    "body": "A new comment, :+1:"
    }
```
Puedes usar las `API` para muchas cosas, como crear metas, asignar personas a un `pull request`, crear o modificar etiquetas, accesar a información de un `commit`, etc.

### Cambiar el estatus de un pull request
En un `pull request` cada `commit` puede tener más de un `status` asocioado, y puedes usar un `API` para cambiarlos.
Muchos de los servicios de integración continua y los servicios de testeo, hacen uso de este `API` para reaccionar a los `pushes` probando el codigo de este y mandando el reporte si paso todas las pruebas, tambien lo puedes usar para revisar el formato del mensaje del `commit`.
Digamos que configuras un `webhooks` en tu repositorio que usa un pequeño servicio web que revise un hilo `Signed-off-by` en el mensaje del `commit`
```
    require 'httparty'
    require 'sinatra'
    require 'json'

    post '/payload' do
    push = JSON.parse(request.body.read) # parse the JSON
    repo_name = push['repository']['full_name']

    # look through each commit message
    push["commits"].each do |commit|

        # look for a Signed-off-by string
        if /Signed-off-by/.match commit['message']
        state = 'success'
        description = 'Successfully signed off!'
        else
        state = 'failure'
        description = 'No signoff found.'
        end

        # post status to GitHub
        sha = commit["id"]
        status_url = "https://api.github.com/repos/#{repo_name}/statuses/#{sha}"

        status = {
        "state"       => state,
        "description" => description,
        "target_url"  => "http://example.com/how-to-signoff",
        "context"     => "validate/signoff"
        }
        HTTParty.post(status_url,
        :body => status.to_json,
        :headers => {
            'Content-Type'  => 'application/json',
            'User-Agent'    => 'tonychacon/signoff',
            'Authorization' => "token #{ENV['TOKEN']}" }
        )
    end
    end
```
Con suerte, esto es facil de seguir, en este `webhook` revisa cada `commit` que es `pushed` busca  por el hilo `Signed-off-by` en el mensaje del `commit` y finalmente lo publica con el `HTTP` en `/repos/<user>/<repo>/statuses/<commit_sha>` con el `API` con `endpoint` en `status`.
En este caso puedes mandar un `status` ("Succes","Failure","Error"), una descripción de lo que pasa y un URL que da más información del porque y contexto, si alguien crea un nuevo `pull request` si este `webhook` esta configurado, podria ver algo como `Commit status via the API`, ademas puedes ver un pequeño `checkbox` que muestra si cumplio con la validación, esto es util para que no hagas `push` a algo a lo que no deberias.

### Octokit
Hasta ahora hemos visto como hacer esto en `curl` y en `HTTP`, bastantes librerias `open source` existen para hacer este `API` más idiomatico, al dia de escribir esto, los lenguajes soportados son: `Go`, `Objective-C`, `Ruby`, y `.NET`. Puedes revisar en https://github.com/octokit mas información sobre estos, espero que estas herramientas puedan ayudarte a personalizar tu GitHub para llevar un flujo de trabajo más limpio.


[Anterior](Ch6.4.md)
[Indice](Ch6/Indice.md)