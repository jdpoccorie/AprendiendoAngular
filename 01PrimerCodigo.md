# Angular Code

Primero

```
npm install -g @angular/cli
```

Segundo
```
ng new --standalone Angular00
```

**OJO:**

Si creamos nuestra app con modulos pero queremos convertirlo a standalone components tenemos un script para eso:
```
ng generate @angular/core:standalone
```

A continuacion nos mostrará algunas opciones:
* Convert all components, directives and pipes to standalone
* Remove unnecessary NgModule classes
* Bootstrap the aplication using standalone APIs

deberemos ejecutar cada una de estas opciones en ese orden, Primero convert all components, luego volveremos a ejecutar el comando `ng generate @angular/core:standalone` y seguidamente la de Remove unnecesary y finalmente el ultimo.

Con estos pasos sencillos tendremos nuestros componentes como standalone

Tercero

## Configuraciones iniciales

En el archivo `angular.json`

en el archivo:
```json
...
architect: {
  build: {
    "builder": "@angular-devkit/build-angular:browser"
  }
}
```
se debe modificar a:
```json
...
architect: {
  build: {
    "builder": "@angular-devkit/build-angular:browser-esbuild"
  }
}
```

Con esta modificacion ya contamos con ViteJS.

En el archivo `app.config.ts`

```ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';

import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes) ]
};
```

se debe de modificar a:
```ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';

import { provideClientHydration } from '@angular/platform-browser';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes), provideClientHydration() ]
};
```

Con esta linea ya contamos con SSR server side rendering

## Carpetas

La estructura de carpetas sigue siendo muy similar a la que teniamos, en nuestro componente principal tenemos:
```ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RouterOutlet } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CommonModule, RouterOutlet],
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})

export class AppComponent {
  title = 'Angular00';
}
```

se puede apreciar los que es un `standalone component` observamos que contiene un **imports** que es basicamente como si tendriamos el module implicito en el componente

## Crear nuevos componentes

```
ng g c test --standalone
```

Ahora si queremos usar este componente test en el componente app simplemtete en el imports le asignamos:

```ts
@Component({
  ...
  imports: [CommonModule, RouterOutlet, TestComponent],
  ...
})
```

## Sobre las directivas

Antes todas las directivas se metian dentro de **CommonModule**, ahi se encontraban los ngIf, ngFor, etc

con los standalone components lo que ahora se debe hacer es:
```ts
@Component({
  ...
  imports: [NgIf, CommonModule, RouterOutlet, TestComponent],
  ...
})
```

## El package.json

Es el manejador de paquetes (librerías)

El package.json contiene todas las dependencias que nuestra aplicacion funcione:

en:
* dependencies: Estan todos nuestros paquetes que hacen que nuestra aplicacion funcione en producción
  * estan rxjs, zonejs que pronto cambiará
* devDependencies: Estan todos nuestros paquetes que con los que trabajamos, paquetes para el desarrollo de la app
  * estan el cli de angular, paquetes de testing, etc

## El angular.json

Define como va a funcionar, trabajar nuestro angular.

En este archivo se puede modificar el builder para que trabaje con el Vite.js

