# aerolineaVirtual

Esta aplicación fue generada con JHipster 9.2.0. Puede consultar la documentación y la ayuda en [https://www.jhipster.tech/documentation-archive/v9.2.0](https://www.jhipster.tech/documentation-archive/v9.2.0).

## Estructura del proyecto

Node es necesario para la generación del proyecto y recomendable para el desarrollo. El archivo `package.json` siempre se genera, con el fin de ofrecer una mejor experiencia de desarrollo mediante prettier, hooks de commit, scripts y demás herramientas.

En la raíz del proyecto, JHipster genera archivos de configuración para herramientas como git, prettier, eslint o husky, todas ampliamente conocidas y documentadas en la web.

La estructura de `/src/*` sigue la organización estándar de un proyecto Java.

- `.yo-rc.json` — archivo de configuración de Yeoman.
  La configuración de JHipster se almacena en este archivo bajo la clave `generator-jhipster`. También puede encontrar claves `generator-jhipster-*` correspondientes a la configuración de blueprints específicos.
- `.yo-resolve` (opcional) — resolutor de conflictos de Yeoman.
  Permite aplicar una acción determinada cuando se detectan conflictos, omitiendo las preguntas para los archivos que coincidan con un patrón. Cada línea debe tener el formato `[patrón] [acción]`, donde el patrón sigue la sintaxis de [Minimatch](https://github.com/isaacs/minimatch#minimatch) y la acción es `skip` (valor por omisión) o `force`. Las líneas que comienzan con `#` se consideran comentarios y se ignoran.
- `.jhipster/*.json` — archivos de configuración de las entidades de JHipster.

- `npmw` — envoltorio para utilizar la instalación local de npm.
  Por defecto, JHipster instala Node y npm de forma local mediante la herramienta de construcción. Este envoltorio garantiza que se use esa instalación local, evitando las diferencias que pueden producir distintas versiones. Al emplear `./npmw` en lugar del `npm` tradicional, es posible configurar un entorno sin Node instalado globalmente para desarrollar o probar la aplicación.
- `/src/main/docker` — configuraciones de Docker para la aplicación y para los servicios de los que depende.

## Desarrollo

El sistema de construcción instalará automáticamente la versión recomendada de Node y npm.

Se proporciona un envoltorio para ejecutar npm. Solo necesitará ejecutar este comando cuando cambien las dependencias declaradas en [package.json](package.json).

```bash
./npmw install
```

El sistema de construcción se apoya en scripts de npm y en Webpack.

Ejecute los siguientes comandos en dos terminales separadas para obtener un entorno de desarrollo cómodo, en el que el navegador se actualiza automáticamente cada vez que se modifican archivos en el disco.

```bash
./npmw run backend:start
./npmw run start
```

npm se utiliza también para gestionar las dependencias de CSS y JavaScript de la aplicación. Puede actualizarlas indicando una versión más reciente en [package.json](package.json), o bien ejecutar `./npmw update` y `./npmw install`. Añada la opción `help` a cualquier comando para consultar su uso; por ejemplo, `./npmw help update`.

El comando `./npmw run` muestra la lista de todos los scripts disponibles en el proyecto.

### Soporte para PWA

JHipster incorpora soporte para aplicaciones web progresivas (PWA), desactivado por omisión. Uno de los componentes principales de una PWA es el _service worker_.

El código de inicialización del _service worker_ viene comentado por defecto. Para activarlo, descomente el siguiente fragmento en `src/main/webapp/index.html`:

```html
<script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('./service-worker.js').then(function () {
      console.log('Service Worker Registered');
    });
  }
</script>
```

Nota: el _service worker_ de JHipster funciona con [Workbox](https://developer.chrome.com/docs/workbox), que genera dinámicamente el archivo `service-worker.js`.

### Gestión de dependencias

Por ejemplo, para añadir la biblioteca [Leaflet](https://leafletjs.com/) como dependencia de ejecución de la aplicación, ejecute:

```bash
./npmw install --save --save-exact leaflet
```

Para disponer durante el desarrollo de las definiciones de tipos de TypeScript del repositorio [DefinitelyTyped](https://definitelytyped.org/), ejecute:

```bash
./npmw install --save-dev --save-exact @types/leaflet
```

A continuación deberá importar los archivos JS y CSS indicados en las instrucciones de instalación de la biblioteca, para que [Webpack](https://webpack.js.org/) los tenga en cuenta.

Nota: en el caso concreto de Leaflet quedan algunos pasos adicionales que no se detallan aquí.

Para más instrucciones sobre cómo desarrollar con JHipster, consulte [Uso de JHipster en desarrollo](https://www.jhipster.tech/development/).

## Construcción para producción

### Empaquetado como jar

Para generar el jar definitivo y optimizar la aplicación aerolineaVirtual para producción, ejecute:

```bash
./mvnw -Pprod clean verify
```

Este proceso concatena y minifica los archivos CSS y JavaScript del cliente, y modifica `index.html` para que apunte a los nuevos archivos generados. Para comprobar que todo funcionó correctamente, ejecute:

```bash
java -jar target/*.jar
```

Después abra [http://localhost:8080](http://localhost:8080) en su navegador.

Consulte [Uso de JHipster en producción](https://www.jhipster.tech/documentation-archive/v9.2.0/production/) para obtener más detalles.

### Empaquetado como war

Para empaquetar la aplicación como un war y desplegarla en un servidor de aplicaciones, ejecute:

```bash
./mvnw -Pprod,war clean verify
```

### JHipster Control Center

JHipster Control Center facilita la gestión y el control de sus aplicaciones. Puede iniciar un servidor local, accesible en http://localhost:7419, con:

```bash
docker compose -f src/main/docker/jhipster-control-center.yml up
```

## Pruebas

### Pruebas de Spring Boot

Para ejecutar las pruebas de la aplicación:

```bash
./mvnw verify
```

### Gatling

Las pruebas de rendimiento se ejecutan con [Gatling](https://gatling.io/) y están escritas en Scala. Se encuentran en [src/test/java/gatling/simulations](src/test/java/gatling/simulations).

Puede ejecutar todas las pruebas de Gatling con:

```bash
./mvnw gatling:test
```

### Pruebas del cliente

Las pruebas unitarias del cliente se ejecutan con Vitest. Están ubicadas junto a los componentes y se lanzan con:

```bash
./npmw test
```

## Otros

### Calidad del código con Sonar

Sonar se utiliza para analizar la calidad del código. Puede iniciar un servidor local, accesible en http://localhost:9001, con:

```bash
docker compose -f src/main/docker/sonar.yml up -d
```

Nota: en [src/main/docker/sonar.yml](src/main/docker/sonar.yml) se desactivó la redirección forzada de autenticación de la interfaz, con el fin de facilitar las primeras pruebas con SonarQube. En un entorno real conviene volver a activarla.

Puede lanzar un análisis de Sonar mediante [sonar-scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner) o con el plugin de Maven:

```bash
./mvnw -Pprod clean verify sonar:sonar -Dsonar.login=admin -Dsonar.password=admin
```

Si necesita repetir la fase de Sonar, asegúrese de indicar al menos la fase `initialize`, ya que las propiedades de Sonar se cargan desde el archivo sonar-project.properties.

```bash
./mvnw initialize sonar:sonar -Dsonar.login=admin -Dsonar.password=admin
```

Además, en lugar de pasar `sonar.password` y `sonar.login` como argumentos de línea de comandos, estos parámetros pueden configurarse en [sonar-project.properties](sonar-project.properties) de la siguiente forma:

```bash
sonar.login=admin
sonar.password=admin
```

Para más información, consulte la [página de calidad del código](https://www.jhipster.tech/documentation-archive/v9.2.0/code-quality/).

### Soporte de Docker Compose

JHipster genera varios archivos de configuración de Docker Compose en la carpeta [src/main/docker/](src/main/docker/) para levantar los servicios de terceros necesarios.

Por ejemplo, para iniciar en contenedores los servicios requeridos, ejecute:

```bash
docker compose -f src/main/docker/services.yml up -d
```

Para detener y eliminar los contenedores:

```bash
docker compose -f src/main/docker/services.yml down
```

La [integración de Spring con Docker Compose](https://docs.spring.io/spring-boot/reference/features/dev-services.html) está activada por omisión. Es posible desactivarla en `application.yml`:

```yaml
spring:
  ...
  docker:
    compose:
      enabled: false
```

También puede llevar a contenedores la aplicación completa junto con todos los servicios de los que depende. Para ello, construya primero la imagen de Docker de la aplicación:

```bash
npm run java:docker
```

O bien, si utiliza un sistema operativo sobre un procesador arm64, como los chips Apple Silicon (M*), construya la imagen correspondiente con:

```bash
npm run java:docker:arm64
```

A continuación ejecute:

```bash
docker compose -f src/main/docker/app.yml up -d
```

Para más información, consulte [Docker y Docker Compose](https://www.jhipster.tech/documentation-archive/v9.2.0/docker-compose/). Esa página incluye también información sobre el subgenerador de Docker Compose (`jhipster docker-compose`), capaz de generar configuraciones de Docker para una o varias aplicaciones de JHipster.

## Integración continua (opcional)

Para configurar la integración continua del proyecto, ejecute el subgenerador ci-cd (`jhipster ci-cd`), que permite generar archivos de configuración para diversos sistemas de integración continua. Consulte la página [Configuración de la integración continua](https://www.jhipster.tech/documentation-archive/v9.2.0/setting-up-ci/) para obtener más información.

## Referencias

- [Página principal y documentación más reciente de JHipster](https://www.jhipster.tech/)
- [Archivo de documentación de JHipster 9.2.0](https://www.jhipster.tech/documentation-archive/v9.2.0)
- [Uso de JHipster en desarrollo](https://www.jhipster.tech/documentation-archive/v9.2.0/development/)
- [Uso de Docker y Docker Compose](https://www.jhipster.tech/documentation-archive/v9.2.0/docker-compose)
- [Uso de JHipster en producción](https://www.jhipster.tech/documentation-archive/v9.2.0/production/)
- [Ejecución de pruebas](https://www.jhipster.tech/documentation-archive/v9.2.0/running-tests/)
- [Calidad del código](https://www.jhipster.tech/documentation-archive/v9.2.0/code-quality/)
- [Configuración de la integración continua](https://www.jhipster.tech/documentation-archive/v9.2.0/setting-up-ci/)
- [Node.js](https://nodejs.org/)
- [npm](https://www.npmjs.com/)
- [Gatling](https://gatling.io/)
- [Webpack](https://webpack.js.org/)
- [BrowserSync](https://www.browsersync.io/)
- [Vitest](https://vitest.dev/)
- [Leaflet](https://leafletjs.com/)
- [DefinitelyTyped](https://definitelytyped.org/)
