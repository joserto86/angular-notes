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

- `[ngClass]="{className: typescriptVariable === 'value'}"`: Ejemplo para añadir una clase dependiendo de una condición.

- `ngForm`: Útil para el manejo de formularios: 
   Ejemplo de validación de formulario desde el template:
   ```
   <form (ngSubmit)="form.form.valid && method()" #form="ngForm">
    <input name="first" required/>
    <input name="second" required/>
   </form>
   ```
   Valida que los campos con el atributo `required` estén cumplimentados.

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

## Interfaces:

- `OnInit`: Esta interfaz utiliza el método `ngOnInit():void` el cual se ejecuta justo después del constructor de la clase.

## Servicios:

Los servicios son clases que se utilizan mediante inyección de dependencias para compartir rutinas entre componentes. Para utilizarlos se introduce el concepto de `providers`:

```
@Component({
  selector: 'custom-component',
  templateUrl: './custom.component.html',
  styleUrls: ['./custom.component.css'],
  providers: [CustomService]
})

export class CustomComponent {
  constructor(private customService: CustomService ){}
}
```
Para no tener que definir el servicio en cada uno de los componentes se puede definir las clase del servicio dentro de los providers del modulo.

```
@NgModule({
    [...]
    
    providers: [
      CustomService
    ],
    
    [...]
  })
```
Para inyectar un servicio dentro de otro utilizamos el decorador `@Injectable()`. Este decorador se definirá en el servicicio alque se le inyecta el otro servicio.

## Pipes:

Estos son algunos pipes por defecto:

- **currency**: A este pipe se le puede añadir también el tipo de moneda, y el formato de número a mostrar.
- **date**
- **percent**

## Routing en angular:

```
import { RouterModule } from '@angular/router';

@NgModule({
  ....
  imports: [  
    RouterModule.forRoot([
      { path: 'welcome', component: WelcomeComponent },
      { path: 'page', component: PageComponent' },
    ]),
    ...
  ]
  ...
]

<router-outlet></router-outlet>
```
Al añadir el componente `<router-outlet>` la aplicación cargará el component especificado por el array de rutas dentro del decorador del módulo.
Para navegar entre diferentes rutas dentro de la aplicación utilizaremos el servicio de `Router` como sigue:

```
import { Router, ActivatedRoute } from '@angular/router'
export CustoClass implements OnInit{

  constructor(private router: Router, 
              private route: ActivatedRoute // Con este servicio podemos acceder a la información de la ruta actual del navegador 
  ) {}

  method() {
    this.router.navigate(['custom/route']);
  }
  
  ngOnInit() {
    let id = this.route.snapshot.params['id'] // y de esta manera obtener el valor del parametro "id" de nuestra ruta.
  }

```

También podemos utilizar el concepto de routing directamente en nuestros templates de las siguiente forma utilizando propertyBinding y especificando la ruta en la que también se pueden incluir parámetros:

```
<a [routerLink]="['/route', param]">navigate</a> // 
```

    
  
# ng cli

- `ng generate component [component-name]` : Con este comando generaremos un nuevo componente, también se puede utilizar su forma abreviada `ng g c [component-name]` `ng g c [component-name]` -s -t` Para generar el template y las css en linea dentro del fichero .ts

- `ng generate module [module-name]`: Con este otro generaremos un nuevo módulo, también se puede utilizar su forma abreviada como el anterior `(ng g m [component-name])` 


# Otros:

`"strictPropertyInitialization": false`: Añadiendo esta propiedad al archivo tsconfig.json de la raiz del proyecto deja de ser necesaria la inicialización de nuestras variables.

