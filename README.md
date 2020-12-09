# [Máster en Ingeniería Web por la Universidad Politécnica de Madrid (miw-upm)](http://miw.etsisi.upm.es)
# Back-end con Tecnologías de Código Abierto: **SPRING**
> Proyecto TPV. Este proyecto es un apoyo docente de la asignatura. Es una aplicación completa realizada con un Front-end: Angular
 y dos Back-ends: Spring. El Back-end-user se desarrolla con programación síncrona y Postgresql. El Back-end-core se
 realiza con programación reactiva y MongoDB.

## Tecnologías necesarias
`Java` `Maven` `Spring-Boot` `Reactor` `Angular` `MondoDB` `JPA` `SQL` `GitHub` `Travis-CI` `Sonarcloud` `Better Code Hub` `Heroku`

## Estado del código
* Front-end-angular: [![Angular - Build](https://github.com/miw-upm/betca-tpv-angular/workflows/Angular%20-%20Build/badge.svg)](https://github.com/miw-upm/betca-tpv-angular/actions)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=es.upm.miw%3Abetca-tpv-angular&metric=alert_status)](https://sonarcloud.io/dashboard?id=es.upm.miw%3Abetca-tpv-angular)
* Back-end-user:[![TPV User](https://github.com/miw-upm/betca-tpv-user/workflows/TPV%20User/badge.svg)](https://github.com/miw-upm/betca-tpv-user/actions)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=es.upm.miw%3Abetca-tpv-user&metric=alert_status)](https://sonarcloud.io/dashboard?id=es.upm.miw%3Abetca-tpv-user)
* Back-end-core:[![CI - Test](https://github.com/miw-upm/betca-tpv-core/workflows/CI%20-%20Test/badge.svg)](https://github.com/miw-upm/betca-tpv-core/actions)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=es.upm.miw%3Abetca-tpv-core&metric=alert_status)](https://sonarcloud.io/dashboard?id=es.upm.miw%3Abetca-tpv-core) 

## :octocat: Repositorios
* [Front-end-angular](https://github.com/miw-upm/betca-tpv-angular)
* [Back-end-user](https://github.com/miw-upm/betca-tpv-user)
* [Back-end-core](https://github.com/miw-upm/betca-tpv-core)

## :gear: Instalación del proyecto
1. Clonar repositorios, **mediante consola**:
   * betca-tpv-angular
```sh
> cd <folder path>
> git clone https://github.com/miw-upm/betca-tpv-angular
> cd betca-tpv-angular
betca-tpv-angular> npm install
```
   * betca-tpv-user
```sh
> cd <folder path>
> git clone https://github.com/miw-upm/betca-tpv-user
```
   * betca-tpv-core
```sh
> cd <folder path>
> git clone https://github.com/miw-upm/betca-tpv-core
```   

2. Importar el proyecto `betca-tpv-angular` mediante **WebStorm** 
   * **Open**, y seleccionar la carpeta del proyecto.
3. Importar los proyectos `betca-tpv-user` & `betca-tpv-core` mediante **IntelliJ** 
   1. **Import Project**, y seleccionar la carpeta del proyecto.
   1. Marcar **Create Project from external model**, elegir **Maven**.
   1. **Next** … **Finish**.

4. Ejecución
   * Ejecución de test: se utiliza MongoDB embebido y H2 embebido
   * Ejecución en local:
      1. BBDD. Se debe tener arrancado el motor de MongoDB: `mongodb://localhost:27017/tpv`, y el motor de 
      Postgresql: `spring.datasource.url=jdbc:postgresql://localhost:5432/tpv`
      2. Spring. Ejecutar mediante linea de comando en ambos proyectos: `> mvn clean spring-boot:run`  
      3. Angular. Ejecutar mediante linea de comand: `> ng serve`  

# :book: Documentación del proyecto
> Este proyecto es la práctica TPV desarrollada de forma colaborativa por todos los alumnos. Se parte de la versión `core`,
ya implementada, y se pretende ampliar con un conjunto de mejoras. Un **T**erminal **P**unto de **V**enta
es un sistema informático que gestiona el proceso de venta mediante una interfaz accesible para los vendedores o compradores.
Permite la creación e impresión del recibo ticket o factura de venta —con los detalles de las referencias y precios— de los artículos vendidos,
actualiza los cambios en el nivel de existencias de mercancías (STOCK) en la base de datos... Además tiene la parte de venta on-line.

## Interfaz de Usuario
* [App TPV desplegada en Heroku](https://betca-tpv-angular.herokuapp.com)

![](./docs/betca-tpv.png)

## Arquitectura del Front-end: Angular
### Carpetas
![](docs/front-end-folders.png)
### Módulos
![](docs/front-end-modules.png)
### Seguridad

Se plantea en dos procesos:

En primer lugar, se debe conseguir un API Key o token de tipo Json Web Token (JWT), accediendo al end-point `POST /users/token` mediante Basic Auth. Para ello se envía en la cabecera las credenciales (Authorization).
Un token JWT está formado por tres partes:
* Header. Se define el tipo.
* Payload. Se establecen los datos que queramos que queden almacenados en el token, como el proveedor, el usuario, fecha
 de expiración… y los privilegios, o en nuestro caso, el rol del usuario. Esta información va codificada y firmada, pero no encriptada, y por lo tanto es una información pública y visible.
* Signature. Es la firma, que depende de una clave secreta, para saber si el contenido de Payload es válido.
> Authorization = Basic \<user>:\<pass><sub>Base64</sub>  
> Authorization = Bearer \<header><sub>Base64</sub> .\<payload><sub>Base64</sub> .\<signature><sub>Base64</sub>

Justo abajo os presentamos un token válido:
```json
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYmYiOjE2MDc0NDk2NzYsInJvbGUiOiJBRE1JTiIsImlzcyI6Im1pdy5qd3QuaXNzdWVyIiwibmFtZSI6ImFkbWluIiwiZXhwIjoxNjA3NDUzMjc2LCJpYXQiOjE2MDc0NDk2NzYsInVzZXIiOiI2In0.iFeIfrslgdwA53dv7-vg27GT7bcEINnqSKJsQJNB4rc
```

Tenéis más documentación de JWT (https://jwt.io).   
Para su tratamiento utilizaremos la librerías de _auth0_ (https://auth0.com) para Angular.

Se han desarrollado dos clases, _AuthService_, que nos realiza el login, y nos facilita métodos para conocer el alcance de 
privilegios del usuario, además de guardar el token. Y también tenemos _TokenInterceptor_, 
que añade automáticamente en cabecera el token, siempre que exista. Ambas clases se encuentran en el módulo _core_.

![](./docs/front-end-security.png)

#### Enrutamiento con seguridad
Hemos decidido realizar carga perezosa de los módulos, y para ello se delega al módulo de rutas la carga de los sub-módulos,
 en el momento que el usuario elige una sub-ruta.   
 
Para la seguridad, se ha implementado uno genérico (_RoleGuardService_), se configura en el propio enrutamiento los roles permitidos,
 y este a través de servicio de autenticación (_AuthService_) comprueba el rol actual del usuario, remitiendo a la página principal
  si este no tuviera el rol requerido.

```typescript
const routes: Routes = [
  {path: '', pathMatch: 'full', redirectTo: 'home'},
  {path: 'home', loadChildren: () => import('./home/home.module').then(module => module.HomeModule)},
  {path: 'shop', loadChildren: () => import('./shop/shop.module').then(module => module.ShopModule)}
  // {path: 'home', component: HomeComponent},  // eager load
  // {path: 'shop', component: ShopComponent}   // eager load
];
@NgModule({imports: [RouterModule.forRoot(routes)],exports: [RouterModule]})
export class AppRoutingModule {}
```

```typescript
const routes: Routes = [
  {path: '', component: HomeComponent, children: []}
];
@NgModule({imports: [RouterModule.forChild(routes)],exports: [RouterModule]})
export class HomeRoutingModule {}
```

```typescript
const routes: Routes = [
  {
    path: '', // 'shop' to forRoot
    component: ShopComponent,
    canActivate: [RoleGuardService],
    data: {roles: [Role.ADMIN, Role.MANAGER, Role.OPERATOR]},
    children: [ // or path: shop/articles
      {path: 'articles', component: ArticlesComponent},
      {path: 'cashier-closed', component: CashierClosedComponent},
      {path: 'cashier-opened', component: CashierOpenedComponent},
      {path: 'providers', component: ProvidersComponent},
      {path: 'tickets', component: TicketsComponent},
    ]
  }
];
@NgModule({imports: [RouterModule.forChild(routes)], exports: [RouterModule]})
export class ShopRoutingModule {}
```

### Código

#### Uso del Pipe `async`

```html
<p><button mat-raised-button (click)=requestSync() >Sync </button> {{sync}}</p>
<p><button mat-raised-button (click)=requestAsync()>Async</button> {{asyn | async}}</p>
```
```typescript
export class InputOverviewExample {
  sync: string;
  asyn: Observable<string>;
  requestSync(): void {
    this.serviceMock()
    .subscribe(item => this.sync = item);
  }
  requestAsync(): void {
     this.asyn = this.serviceMock();
  }
  serviceMock(): Observable<string> {
    return of('Result');
  }
}
```

### Crud Component
```html
<app-crud (create)="create()" (read)="read($event)" (update)="update($event)"
          [data]="articles" [deleteAction]="false" [title]="title"></app-crud>
```
```typescript
export class ArticlesComponent {
  title = 'Articles management';
  articles = of([]);
  create(): void {
    this.dialog.open(ArticleCreationUpdatingDialogComponent);
  }
  read(article: Article): void {
    this.dialog.open(ReadDetailDialogComponent, {
      data: {
        title: 'Article Details',
        object: this.articleService.read(article.barcode)
      }
    });
  }
  update(article: Article): void {
    this.articleService.read(article.barcode)
      .subscribe(fullArticle => this.dialog.open(ArticleCreationUpdatingDialogComponent, {data: fullArticle}));
  }
}
```

### Anexo. Preparación del proyecto y ecosistema

#### Instalaciones de herramientas
1. Desinstalar node & borrado de carpetas, si procede:
   * C:\Users\*\AppData\Roaming\npm
   * C:\Users\*\AppData\Roaming\npm-cache

2. Instalar Node, todo estándar
   * node  --version  v14.15.0
   * npm –version  6.14.8
   * npm list // muestra todas las dependencias instaladas
   * npm list <dependence> // muestra la dependencia especificada
   * npm update –g
   
3. Instalar Angular CLI
   * npm install -g @angular/cli   11.0.1
   * npm install -g @angular/cli@latest
   * ng –-version

4. Crear una nueva aplicación
   * ng new <app>

5. Ejecutar aplicación:
   * cd &lt;app>
   * ng serve
   * Navegador: http://localhost:4200/
   
6. Instalar express:
   * npm i express  // i:install
   * npm list express    4.17.1
   * Crear el fichero `server.js`
   * ng build  --prod
   * node server.js
   
7. Instalar Material + Flex
   * ng add @angular/material
   * npm i @angular/flex-layout
   * npm list @angular/material   11.0.0
   * npm list @angular/flex-layout   11.0.0-beta.33
   
8. Instalar Jwt
   * npm i @auth0/angular-jwt
   * npm list @auth0/angular-jwt   5.0.1

#### Versionado: **package.json**
```json
{
  "name": "betca-tpv-angular",
  "version": "4.1.0-SNAPSHOT",
}
```
`~: versión mas cercana posible, ^: versión compatible mas alta`
#### Perfil: **carpeta _environments_**

En el fichero `tsconfig`, habilitar las opciones de compilación:
```json
{
  "compilerOptions": {
    "resolveJsonModule": true
  }
}
```
`enviroments.ts`
```typescript
import {name, version} from '../../package.json';
export const environment = {
  production: false,
  NAME: name,
  VERSION: version,
  REST_USER: 'http://localhost:8081',
  REST_CORE: 'http://localhost:8082'
};
```
`enviroments.prod.ts`
```typescript
import {name, version} from '../../package.json';
export const environment = {
  production: true,
  NAME: name,
  VERSION: version,
  REST_USER: 'https://betca-tpv-user.herokuapp.com',
  REST_CORE: 'https://betca-tpv-core.herokuapp.com'
```
### Rutas absolutas: @*

En el fichero `tsconfig`, habilitar las opciones de compilación:
```json
{
  "compilerOptions": {
    "baseUrl": "src",
    "paths": {
      "@env": ["environments/environment"],
      "@shared/*": ["app/shared/*"],
      "@core/*": ["app/core/*"]
    }
  }
}
```
### Scripts: **package.json**
```json
{
  "scripts": {
    "heroku-postbuild": "ng build --prod",
    "build-prod": "ng build --prod",
    "start": "node server.js"
  }
}
```
### GitHub Actions: CI & CD
Se establecen los workflows en la carpeta: `.github/workflows/*.yml`

Conexión con **Sonarcloud**:
1. Se debe crear el proyecto en sonarcloud: `Administration/Projects Management`, botón `Create Project`. 

2. Configurar la rama por defecto, renombrar `master` si esta no es la rama por defecto: `Administración/Branches and Pull Requests`.
Renombrar las `long living branch: (master|develop|release-).*`

3. Se deben establecer la contraseña: `secrets.SONAR_TOKEN` en en proyecto de GitHub, `settings/Secrets`
Para `Sonarcloud`, se debe crear el fichero: `sonar-project.properties`.

Conexión con **Heroku**:
1. Crear la aplicación en Heroku.
2. Se deben establecer la contraseña: `secrets.HEROKU_API_KEY` en en proyecto de GitHub.   
3. Deben existir los scripts para construcción y arranque con `express`:
```json
{
  "scripts": {
    "heroku-postbuild": "ng build --prod",
    "start": "node server.js"
  }
}
```

## Arquitectura del Back-end: Spring-User mediante Arquitectura por Capas

## Arquitectura del Front-end: Spring-Core mediante Arquitectura Hexagonal

