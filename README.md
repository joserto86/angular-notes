# angular-notes
Mis apuntes de angular

## Primeros pasos

```
ng new [project-name]
```

El comando se encargara de descargar las dependencias necesarias creando una nuevo directorio en la ruta actual con la estructura de carpetas necesarias para una nueva aplicación de angular.

Para iniciar un servidor de desarrollo:

```
ng serve -o
```

- `/main.ts`: Es el archivo principal de la aplicación angular. Este a su vez hace referencia a un módulo de la aplicación (`AppModule`) que normalmente será el módulo principal de la aplicación. 

- Estructura de AppModule:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component'; //ruta del componente
import { PersonasComponent } from './personas/personas.component'; //ruta del componente

@NgModule({
  declarations: [
    AppComponent,
    PersonasComponent // en este apartados iremos añadiendo componentes asociados al módulo
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Instalar paquetes node:

```
npm install [package-name] --save //instala y guarda el paquete dentro del directorio node-modules del proyecto (jquery, bootstrap ...)
```

### Añadir js, css de terceros:

Para añadir los estilos de bootrap, la libreria de jquery u otras dependencias externas a nuestra aplicación de angular, hay que añadir la ruta de las mismas dentro del archivo `angular.js` de la raiz del proyecto tal que así:

```
      [...]
      "styles": [
        "src/styles.css",
        "node_modules/bootstrap/dist/css/bootstrap.min.css"
      ],
      "scripts": [
        "node_modules/jquery/dist/jquery.slim.min.js",
        "node_modules/popper.js/dist/umd/popper.min.js",
        "node_modules/bootstrap/dist/js/bootstrap.min.js"
      ]
      [...]
```

## Fundamentos de angular:

- Interpolación `{{ variable }}` Se utiliza para añadir valores dinámicos en la plantilla html del componente, la interpolación también permite manejar expresiones como sumas (`{{ variable + 1}}`)

# ng cli

- `ng generate component [component-name]` : Con este comando generaremos un nuevo componente, también se puede utilizar su forma abreviada `ng g c [component-name]` `ng g c [component-name]` -s -t` Para generar el template y las css en linea dentro del fichero .ts




