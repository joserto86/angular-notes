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

### Local Reference:

```
<input type="text" name="name" id="name" #nameInput/>   
<input type="text" name="surname" id="surname" #surnameInput/>
<button type="submit" (click)="addPerson(nameInput, surnameInput)">Agregar</button>

addPerson(nameInput:HTMLInputElement, surnameInput:HTMLInputElement) {
    let person = new Persona(nameInput.value, surnameInput.value);
}
```
### View Child

El concepto es similar a **LocalReference** salvo por la utilización del decorador `@ViewChild('nameInput')` que hace referencia al input de Html y que el tipo de la variable local declarada dentro de la clase typescript es de tipo `ElementRef`. Posteriormente será necesario acceder al atributo `nativeElement` de la variable para acceder a su información.
```
@ViewChild('nameInput') nameInput: ElementRef;
@ViewChild('surnameInput') surnameInput: ElementRef; 

addPerson() {
  let person = new Persona(this.nameInput.nativeElement.value, this.surnameInput.nativeElement.value);
}
```


## Fundamentos de angular:

- **Interpolación:** `{{ variable }}` Se utiliza para añadir valores dinámicos en la plantilla html del componente, la interpolación también permite manejar expresiones como sumas (`{{ variable + 1}}`).

- **Property Binding:** `[html-attr]=class-property`: Con esta propiedad podemos cambiar el valor de atributos html de manera dinámica 
    por ejemplo: `[disabled]="property"` y dentro de la clase del componente la propiedad "property".
    
- **Event Binding:** `(event)=function()` Permite esuchar eventos de los componentes html. 
    Ejemplo

  ```
  <input type="text" class="form-control" (input)='changeTitle($event)'/>
  {{ title }}
  
  changeTitle(event: Event) {
      this.title = (<HTMLInputElement>event.target).value
  }
  
  ```
- **Two way binding:**: `[(ngModel)]=class-variable` Enlaza las variables con el fichero typescript del componente en los dos sentidos, desde la plantilla hacia la clase typescript y viceversa. Va a ser necesario incluir un nuevo módulo a nuestra aplicación angular para permitir este comportamiento.

  Este método combina EventBinding y PropertyBinding, de ahí el nombre Two way binding.

  ```
  import { FormsModule } from '@angular/forms';
  @NgModule({
    [...]
    
    imports: [
      BrowserModule, 
      FormsModule
    ],
    
    [...]
  })
  
  Ejemplo:
  
  <input type="text" class="form-control" [(ngModel)]="title" /> 
  
  ```
  
## Directivas:

- `*ngIf="variable o expression"` 

- `*ngFor="let item of items"`

- `@Input()`: Permite recibir información de un componente hijo desde un componente padre.
  
  Ejemplo:
  ```
  <app-persona
    *ngFor="let personaElement of personas; let i = index"
    [persona] = "personaElement"
    [indice] = "i"
  ></app-persona>
  
  @Input() persona: Persona;
  @Input() indice: number;
  
  ```

- `@Output()`: Permite enviar información de un componente hijo hacia un componente padre.

  Ejemplo
  ```
  
  @Output() personaCreated = new EventEmitter<Persona>();
  
  addPerson(
    person = new Person( ... );
    this.personaCreated.emit(person); //con el método emit se emite hacia el padre el elemento
  )
  
  <app-formulario
        (personaCreated)="personaAdded($event)" //el método `personaAdded($event)` debe declararse en el componente padre y captura el objeto dentro del evento.
  ></app-formulario>
  
  personaAdded(person: Persona) {
    this.personas.push(person);
  }

  ```


## Clases:

```
export class Clase {

  //con esta sintaxis podemos declarar atributos a la clase e inicializarlos en el propio constructor
  constructor(public varA: string, public varB: string){}
}
```
  
# ng cli

- `ng generate component [component-name]` : Con este comando generaremos un nuevo componente, también se puede utilizar su forma abreviada `ng g c [component-name]` `ng g c [component-name]` -s -t` Para generar el template y las css en linea dentro del fichero .ts


# Otros:

`"strictPropertyInitialization": false`: Añadiendo esta propiedad al archivo tsconfig.json de la raiz del proyecto deja de ser necesaria la inicialización de nuestras variables.

