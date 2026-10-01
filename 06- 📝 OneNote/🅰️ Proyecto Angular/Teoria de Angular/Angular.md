# 🅰️ Preguntas de Angular para Entrevistas

> [!info] **¿Qué es Angular?**
> Es un framework de desarrollo frontend de código abierto basado en TypeScript, mantenido por Google y la comunidad. Está diseñado para construir aplicaciones SPA, aunque también facilita la creación de aplicaciones web, móviles y de escritorio.

---

## 🧰 ¿Qué trae Angular?

> [!abstract] **Características que incluye**
> 1. **Plantillas declarativas:** Describe el HTML de la UI usando sintaxis especial (`*ngIf`, `*ngFor`, etc.).
> 2. **Inyección de dependencias:** Permite proveer servicios a componentes sin instanciarlos manualmente.
> 3. **Two-way Data Binding:** Sincronización automática entre el modelo y la vista.
> 4. **Módulos (NgModules):** Organiza la app en bloques funcionales reutilizables.
> 5. **Routing (Router):** Navegación entre vistas sin recargar la página.
> 6. **RxJS/Observables:** Manejo de programación reactiva y asíncrona.
> 7. **CLI (Command Line Interface):** Herramienta que automatiza la creación, compilación y testing.

> [!success] **¿Por qué usar Angular?**
> Angular es mejor cuando necesitas una app grande, con equipos grandes, donde la estructura y las convenciones importan. Todo viene incluido: router, HTTP client, forms, testing, etc.

---

## 🌐 ¿Qué es una SPA?

> [!info] **SPA (Single Page Application)**
> Es una aplicación web que carga una sola vez el HTML y después actualiza el contenido dinámicamente sin recargar la página completa.

> [!success] **Ventajas**
> * **Velocidad:** No recarga toda la página, solo lo que cambia.
> * **Mejor UX:** Se siente como una app nativa/de escritorio.
> * **Menos carga al servidor:** Solo pide datos (JSON), no HTML completo.
> * **Separación de responsabilidades:** El Frontend maneja la UI, el Backend solo provee datos.

> [!tip] **¿Cómo lo hace Angular?**
> A través de su Router, que intercepta la navegación y decide qué componente mostrar sin ir al servidor.

---

## ⚔️ AngularJS vs Angular

> [!faq]+ **AngularJS (Versión 1)**
> Framework JavaScript lanzado en 2010, arquitectura MVC, usaba `$scope` para conectar datos y vistas. Fue revolucionario en su época pero quedó limitado en rendimiento y escalabilidad. Fin de soporte en 2021.

> [!faq]+ **Angular (Versión 2+)**
> Reescritura total en TypeScript, arquitectura basada en Componentes, mejor rendimiento con Change Detection inteligente, soporte móvil, CLI, RxJS integrado y diseñado para aplicaciones empresariales grandes.

---

## 🔷 ¿Qué es TypeScript?

> [!info]
> Es un superconjunto tipado de JavaScript creado por Microsoft que agrega tipos opcionales.

---

## 🏗️ Arquitectura y Conceptos Core

### 🧩 Componentes Clave de Angular

> [!abstract] **Los componentes fundamentales son:**
> 1. **Componente:** Los componentes básicos de una aplicación Angular para controlar vistas HTML.
> 2. **Módulos:** Un conjunto de bloques de construcción básicos angulares como componentes, directivas, servicios, etc. Una aplicación se divide en partes lógicas y cada fragmento se denomina "módulo".
> 3. **Plantillas:** Representan las vistas con sintaxis especial de una aplicación Angular.
> 4. **Servicios:** Clases especializadas en lógica de negocio que se comparten entre componentes a través de la Inyección de Dependencias.
> 5. **Metadatos:** La información que le das a Angular a través de los **decoradores** para que sepa cómo tratar una clase.

### 🏷️ Metadatos y Decoradores

> [!info] **¿Qué son los metadatos?**
> Son la información de configuración que le damos a Angular sobre cómo debe tratar una clase. Sin metadatos, Angular vería tu clase como una clase TypeScript normal, sin saber que es un componente, servicio o módulo.

> [!code] **Decoradores de Clase**
> * `@Component` → Define un componente
> * `@NgModule` → Define un módulo
> * `@Injectable` → Define un servicio inyectable
> * `@Directive` → Define una directiva
> * `@Pipe` → Define un pipe

> [!code] **Decoradores de Propiedad**
> * `@Input()` → Recibe datos del padre
> * `@Output()` → Envía eventos al padre
> * `@ViewChild()` → Accede a elemento de la vista propia
> * `@ContentChild()` → Accede a contenido proyectado

> [!code] **Decoradores de Método/Parámetro**
> * `@HostListener()` → Escucha eventos del elemento host
> * `@HostBinding()` → Vincula propiedad al elemento host
> * `@Inject()` → Especifica qué token inyectar
> * `@Optional()` → Hace opcional una dependencia

### 📦 ¿Qué son los NgModules?

> [!info] **NgModule**
> Organiza la aplicación en bloques funcionales reutilizables. Los metadatos `@NgModule` le dicen al compilador AOT qué compilar (`declarations`), de dónde traer dependencias (`imports`), qué exportar y cómo configurar DI.

> [!abstract] **Ejemplos de NgModules**
> | NgModule | ¿Qué habilita? | ¿Dónde importar? |
> | :--- | :--- | :--- |
> | `BrowserModule` | Soporte del navegador, ngIf, ngFor | Solo AppModule |
> | `CommonModule` | ngIf, ngFor, pipes | Módulos hijos |
> | `FormsModule` | ngModel, template-driven forms | Donde se use |
> | `ReactiveFormsModule` | FormGroup, FormControl | Donde se use |
> | `HttpClientModule` | HttpClient | Solo AppModule |
> | `RouterModule` | routerLink, router-outlet | AppModule y hijos |

> [!tip] **CommonModule**
> Es el módulo que agrupa directivas y pipes comunes como `NgIf` y `NgFor`. Se importa en módulos funcionales para habilitar funcionalidades básicas de plantillas. `BrowserModule` ya reexporta `CommonModule`, por eso muchas veces no lo ves importado manualmente en `AppModule`.

---

## 🧩 Componentes

### 📦 ¿Qué es un Componente?

> [!info]
> Un componente es la pieza básica de una aplicación Angular. Controla una parte de la pantalla (una vista HTML) y tiene su propia lógica en TypeScript, sus estilos CSS y su plantilla HTML.

### 🔄 Comunicación entre Componentes

> [!abstract] **Formas de comunicación**
> * **@Input y @Output:** Para relaciones padre-hijo.
> * **Servicios compartidos:** Cuando los componentes no están directamente relacionados.
> * **Parámetros del Router:** Para pasar datos entre vistas.

### 🔀 Componente vs Directiva

| Aspecto | Componente | Directiva |
| :--- | :--- | :--- |
| **Decorador** | `@Component` | `@Directive` |
| **Template** | ✅ Obligatorio | ❌ No tiene |
| **Estilos propios** | ✅ Sí | ❌ No |
| **Por elemento DOM** | Solo 1 | Múltiples |
| **Crea elementos** | ✅ Crea su propio DOM | ❌ Modifica DOM existente |
| **Uso** | Widgets/secciones de UI | Comportamiento reutilizable |
| **Selector típico** | `app-usuario` (etiqueta) | `[resaltado]` (atributo) |

### 🔁 Lifecycle Hooks

> [!info] **¿Qué son los ganchos de ciclo de vida?**
> La aplicación Angular pasa por un conjunto completo de procesos o tiene un ciclo de vida desde su inicio hasta el final de la aplicación.

> [!abstract] **Los Lifecycle Hooks:**
> 1. `ngOnChanges` → Cuando cambia el valor de una propiedad vinculada a datos.
> 2. `ngOnInit` → Inicialización del componente después de que Angular muestra por primera vez las propiedades vinculadas.
> 3. `ngDoCheck` → Detecta y actúa sobre cambios que Angular no puede detectar por sí solo.
> 4. `ngAfterContentInit` → Después de que Angular proyecta contenido externo en la vista.
> 5. `ngAfterContentChecked` → Después de que Angular verifica el contenido proyectado.
> 6. `ngAfterViewInit` → Después de que Angular inicializa las vistas del componente y las vistas secundarias.
> 7. `ngAfterViewChecked` → Después de que Angular verifica las vistas del componente y las vistas secundarias.
> 8. `ngOnDestroy` → Limpieza justo antes de que Angular destruya el componente.

### ⚖️ Constructor vs ngOnInit

> [!faq]+ **Constructor**
> Es un método predeterminado de la clase que se ejecuta cuando se crea una instancia. Se usa principalmente para inyección de dependencias e inicializar valores muy básicos.
> * **Regla:** "Hola Angular, estas son las herramientas que necesito" → Solo declaras dependencias, nada más.

> [!faq]+ **ngOnInit**
> Es un gancho de ciclo de vida llamado por Angular para indicar que ha terminado de crear el componente. Se usa para carga de datos, lógica de inicialización real, llamadas a servicios y trabajo con `@Input()` ya disponible.
> * **Regla:** "Angular terminó de prepararlo todo, ahora sí trabajo" → Aquí ejecutas toda tu lógica real.

> [!important] **Comparación**
> Principalmente usamos `ngOnInit` para toda la inicialización/declaración y evitamos que cosas funcionen en el constructor. El constructor solo debe usarse para inicializar miembros de la clase, pero no debe realizar "trabajo" real.

---

## 🎨 Template y Vistas

### 📝 Sintaxis del Template

> [!abstract] **Los 5 tipos de sintaxis en un Template**
> 1. **Interpolación:** `{{ }}` → Muestra datos de la clase en la vista.
> 2. **Property Binding:** `[ ]` → Pasa datos de la clase al DOM (flujo de una vía).
> 3. **Event Binding:** `( )` → Escucha eventos del DOM hacia la clase (flujo de una vía).
> 4. **Two-way Binding:** `[( )]` → Sincronización en ambas direcciones clase ↔ vista.
> 5. **Directivas estructurales:** `*ngIf`, `*ngFor` → Controlan la estructura y apariencia.

> [!code] **Ejemplos**
> ```html
> <!-- Interpolación → Muestra datos de la clase en la vista -->
> <h1>{{ nombre }}</h1>
>
> <!-- Property Binding → Pasa datos de la clase al DOM -->
> <input [value]="nombre">
>
> <!-- Event Binding → Escucha eventos del DOM hacia la clase -->
> <button (click)="guardar()">Guardar</button>
>
> <!-- Two-way Binding → Sincronización doble -->
> <input [(ngModel)]="nombre">
>
> <!-- Directivas estructurales → Controlan estructura -->
> <p *ngIf="isLoggedIn">Bienvenido</p>
> <li *ngFor="let item of lista">{{ item }}</li>
> ```

> [!abstract] **Tabla de Sintaxis**
> | Sintaxis | Símbolo | Dirección | Para qué |
> | :--- | :---: | :--- | :--- |
> | **Interpolación** | `{{ }}` | Clase → Vista | Mostrar datos |
> | **Property Binding** | `[ ]` | Clase → DOM | Pasar valores a propiedades |
> | **Event Binding** | `( )` | DOM → Clase | Escuchar eventos |
> | **Two-way Binding** | `[( )]` | Clase ↔ Vista | Sincronización doble |
> | **Directivas** | `*ngIf`, `*ngFor` | — | Controlar estructura |
> | **Pipes** | `\|` | — | Transformar datos |

### 🔗 Data Binding

> [!info] **¿Qué es el Data Binding?**
> Es el mecanismo de comunicación automática que sincroniza los datos entre la lógica del componente (el código de TypeScript) y la interfaz gráfica que ve el usuario (la plantilla en HTML).

> [!info] **¿Qué es un enlace de datos?**
> Permite definir la comunicación entre un componente y el DOM, lo que hace que sea muy fácil definir aplicaciones interactivas sin preocuparse por enviar y extraer datos.

> [!abstract] **Cuatro formas de vinculación de datos (3 categorías):**
> * **Del componente al DOM:**
>   - Interpolación: `{{ valor }}` → Agrega el valor de una propiedad del componente.
>   - Vinculación de propiedades: `[propiedad]="valor"` → El valor se pasa del componente a la propiedad del DOM.
> * **Del DOM al componente:**
>   - Enlace de evento: `(evento)="función"` → Cuando ocurre un evento DOM, llama al método del componente.
> * **Bidireccional:**
>   - Vinculación bidireccional: `[(ngModel)]="valor"` → Los datos fluyen en ambos sentidos.

### 🎯 Directivas

> [!info] **¿Qué son las Directivas?**
> Son instrucciones (clases) que modifican el comportamiento o apariencia del DOM.

> [!abstract] **Tipos de directivas:**
> * **Estructurales:** Cambian la estructura del DOM.
>   - `*ngIf` → Inserta o elimina elementos según una condición booleana.
>   - `*ngFor` → Permite iterar sobre una colección y renderizar un bloque HTML por cada elemento.
> * **De atributo:** Cambia apariencia (estilos) o comportamiento.
>   - `ngClass` → Agrega/quita clases CSS.
>   - `ngStyle` → Cambia estilos inline.
> * **Personalizadas:** Que podemos crear nosotros como desarrolladores.

### 🔄 Pipes (Tuberías)

> [!info] **¿Qué son las Pipes?**
> Son operadores de plantilla que transforman datos de forma declarativa antes de mostrarlos en la vista. Se usan para formatear fechas, monedas, texto y otros valores sin ensuciar el componente.

> [!code] **Ejemplos**
> ```html
> <!-- Sin pipe -->
> <p>{{ precio }}</p> <!-- 1000 -->
>
> <!-- Con pipes -->
> <p>{{ precio | currency:'USD' }}</p> <!-- $1,000.00 -->
> <p>{{ nombre | uppercase }}</p> <!-- JORDANY -->
> <p>{{ fecha | date:'dd/MM/yyyy' }}</p> <!-- 09/04/2026 -->
> ```

> [!abstract] **Pipes más comunes**
> | Pipe | Ejemplo | Resultado |
> | :--- | :--- | :--- |
> | `uppercase` | `'hola' \| uppercase` | HOLA |
> | `lowercase` | `'HOLA' \| lowercase` | hola |
> | `titlecase` | `'hola mundo' \| titlecase` | Hola Mundo |
> | `date` | `hoy \| date:'dd/MM/yyyy'` | 12/04/2026 |
> | `currency` | `1000 \| currency:'MXN'` | MX$1,000.00 |
> | `number` | `1234.5 \| number:'1.2-2'` | 1,234.50 |
> | `percent` | `0.85 \| percent` | 85% |
> | `slice` | `'Hola' \| slice:0:2` | Ho |
> | `json` | `obj \| json` | `{"key":"value"}` |
> | `async` | `obs$ \| async` | valor del Observable |

---

## 🔧 Servicios e Inyección de Dependencias

### 📦 ¿Qué es un Servicio?

> [!info] **Servicio en Angular**
> Un servicio es una clase reutilizable decorada con `@Injectable` que centraliza lógica compartida y separa responsabilidades. Angular lo provee a los componentes a través de la Inyección de Dependencias, creando por defecto una única instancia (Singleton) compartida en toda la app.

> [!success] **¿Qué hace?**
> * Centraliza la lógica en común.
> * Evita duplicar código.
> * Facilita pruebas.
> * Hace el código más limpio y mantenible.

> [!tip] **¿Cuándo usarla?**
> * Cuando varias partes de la app usan la misma lógica.
> * Cuando quieres separar acceso a datos de la UI.
> * Cuando necesitas reutilizar funciones como: Llamadas HTTP, Autenticación, Validaciones, Manejo de estado, Helpers compartidos.

### 🎯 Patrón Singleton

> [!info] **¿Qué es el patrón Singleton?**
> Es un patrón de diseño que garantiza que una clase tenga una sola instancia en toda la aplicación y que esa instancia sea compartida por todos los que la necesiten.

> [!abstract] **Comparación**
> * **Sin Singleton:** Cada componente crea una instancia nueva → datos NO compartidos.
> * **Con Singleton:** La instancia se crea UNA sola vez → todos comparten la misma → datos compartidos.

### 💉 Inyección de Dependencias (DI)

> [!info] **¿Qué es la Inyección de Dependencias en Angular?**
> Es un patrón de diseño donde una clase declara lo que necesita en su constructor y Angular se encarga de crearlo y entregarlo automáticamente, sin que la clase sepa cómo se construye esa dependencia. Esto reduce el acoplamiento, facilita las pruebas y hace el código más mantenible.

> [!abstract] **Tres pilares de la DI:**
> * **Proveedor:** Registra el servicio.
> * **Inyector:** Lo crea y gestiona.
> * **Dependencia:** Lo que se solicita.

---

## ⚡ RxJS y Observables

### 🔄 ¿Qué es RxJS?

> [!info]
> Es una librería que nos da herramientas para la programación reactiva.

> [!abstract] **Componentes de RxJS:**
> * **Observables:** La tubería de datos que puede emitir valores a lo largo del tiempo, el cual representa los datos que VAN A LLEGAR (o pueden llegar), normalmente se hace en el servidor.
> * **Operators:** Transforman, filtran o combinan datos (`map`, `filter`, `mergeMap`).
> * **Subject:** Un Observable que también puede emitir valores manualmente.
> * **Subscription:** El control de la Suscripción, es donde recibo y manipulo esos datos.

> [!abstract] **Callbacks de la Suscripción:**
> | Callback | ¿Cuándo se ejecuta? | ¿Para qué usarlo? |
> | :--- | :--- | :--- |
> | `next` | Cada vez que llega un dato | Manipular y mostrar en la vista |
> | `error` | Si algo falla | Mostrar mensajes de error al usuario |
> | `complete` | Cuando el Observable termina | Limpiar, redirigir, notificar |

### ⚖️ Observable vs Promise

> [!faq]+ **Observable**
> Puede devolver múltiples valores en diferentes momentos.

> [!faq]+ **Promise**
> Solo devuelve un solo valor a la vez.

> [!important] **Diferencia clave**
> Una Promise resuelve un solo valor, mientras que un Observable puede emitir muchos valores a lo largo del tiempo. Por eso Angular usa Observables para cosas reactivas como formularios, eventos y respuestas HTTP.

### 📡 ¿Qué es subscribe()?

> [!info]
> Es el método que activa un Observable y permite escuchar sus emisiones. Recibe callbacks para `next`, `error` y `complete`, y devuelve una `Subscription` que puede cancelarse con `unsubscribe()`.

> [!code] **Ejemplo**
> ```typescript
> myObservable.subscribe({
>   next: x => console.log('Observer got a next value: ' + x),
>   error: err => console.error('Observer got an error: ' + err),
>   complete: () => console.log('Observer got a complete notification')
> });
> ```

---

## 🌐 Comunicación HTTP

### 📡 ¿Qué es HttpClient?

> [!info]
> Angular proporciona una API HTTP de cliente simplificada conocida como `HttpClient` que se basa en `XMLHttpRequest`. Es la API de Angular para hacer peticiones HTTP al Backend.

> [!success] **Beneficios**
> * **Más fácil de probar:** Se integra bien con tests unitarios y mocks.
> * **Tipado fuerte:** Puedes definir el tipo de respuesta con TypeScript.
> * **Interceptors:** Permite modificar solicitudes y respuestas de forma centralizada.
> * **Observables:** Devuelve Observable, lo que encaja con RxJS y operaciones reactivas.
> * **Manejo de errores:** Se combina muy bien con `catchError` y flujos de error controlados.

> [!tip] **¿Cuándo se usa?**
> * Leer datos de una API.
> * Enviar formularios.
> * Actualizar registros.
> * Eliminar información.
> * Manejar autenticación con tokens.

### 📋 ¿Cómo usar HttpClient?

> [!code] **Pasos**
> 1. Importar `HttpClient` en el módulo raíz.
> 2. Inyectar el `HttpClient` en el TS del componente.

### 📨 Respuesta Completa del Servidor

> [!info]
> Por defecto `HttpClient` solo devuelve el body de la respuesta. Usando `observe: 'response'` obtienes el objeto `HttpResponse` completo que incluye el body tipado, el `status code`, `statusText`, la URL y todos los headers.

> [!code] **Ejemplo**
> ```typescript
> getUserResponse(): Observable<HttpResponse<User>> {
>   return this.http.get<User>(
>     this.userUrl, { observe: 'response' });
> }
> ```

### 📤 Headers HTTP

> [!code] **Formas de pasar headers**
> ```typescript
> // Opción 1: Mapa de objetos
> this._http.get('someUrl', {
>   headers: {'header1':'value1','header2':'value2'}
> });
>
> // Opción 2: HttpHeaders
> let headers = new HttpHeaders().set('header1', headerValue1);
> headers = headers.append('header2', headerValue2);
>
> let params = new HttpParams().set('param1', value1);
> params = params.append('param2', value2);
>
> return this._http.get<any[]>('someUrl', { headers: headers, params: params });
> ```

### ⚠️ Manejo de Errores

> [!abstract] **3 niveles de manejo de errores:**
> 1. **Nivel 1 → En el `subscribe()` del componente** (básico)
> 2. **Nivel 2 → Con `catchError()` en el servicio** (recomendado) ⭐
> 3. **Nivel 3 → Con interceptores HTTP** (centralizado)

> [!code] **Nivel 2 — Con catchError en el servicio (Recomendado)**
> ```typescript
> import { catchError, throwError } from 'rxjs';
> import { HttpErrorResponse } from '@angular/common/http';
>
> @Injectable({ providedIn: 'root' })
> export class UsuarioService {
>   constructor(private http: HttpClient) { }
>
>   obtenerUsuario(id: number): Observable<Usuario> {
>     return this.http.get<Usuario>(`${this.apiUrl}/${id}`)
>       .pipe(
>         catchError(this.manejarError)
>       );
>   }
>
>   private manejarError(error: HttpErrorResponse): Observable<never> {
>     let mensajeError = '';
>     if (error.status === 0) {
>       mensajeError = 'Sin conexión a internet. Verifica tu red.';
>     } else {
>       switch(error.status) {
>         case 400: mensajeError = 'Solicitud incorrecta'; break;
>         case 401: mensajeError = 'No autorizado. Inicia sesión nuevamente'; break;
>         case 403: mensajeError = 'No tienes permisos para esta acción'; break;
>         case 404: mensajeError = 'El recurso solicitado no existe'; break;
>         case 500: mensajeError = 'Error interno del servidor'; break;
>         default: mensajeError = `Error inesperado: ${error.status}`;
>       }
>     }
>     console.error('Error HTTP:', error);
>     return throwError(() => new Error(mensajeError));
>   }
> }
> ```

### 🔗 Interceptores HTTP

> [!info] **¿Qué son los interceptores HTTP?**
> Son parte de `@angular/common/http`, que inspeccionan y transforman las solicitudes HTTP de su aplicación al servidor y viceversa en las respuestas HTTP.

> [!abstract] **Casos comunes de uso:**
> | Tarea | Ejemplo |
> | :--- | :--- |
> | **Autenticación** | Agregar `Authorization: Bearer token` |
> | **Logging** | Registrar tiempo de respuesta |
> | **Errores** | Manejar 401/403 globalmente |
> | **Caching** | Guardar respuestas frecuentes |

> [!code] **Sintaxis**
> ```typescript
> interface HttpInterceptor {
>   intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>>
> }
> ```

> [!tip] **¿Cómo usar interceptores en toda la aplicación?**
> 1. Crear los interceptores implementando `HttpInterceptor`.
> 2. Importar `HttpClientModule` una sola vez en `AppModule`.
> 3. Registrar cada interceptor en `providers` usando el token `HTTP_INTERCEPTORS` con `multi: true`.

---

## 🧭 Enrutamiento

### 🗺️ ¿Qué es Angular Router?

> [!info]
> Angular Router es el sistema de enrutamiento de Angular que permite navegar entre vistas dentro de una SPA sin recargar la página. Asocia URLs con componentes y usa `routerLink` y `router-outlet` para mostrar la vista activa.

> [!success] **¿Qué hace?**
> Angular Router decide qué componente se debe mostrar cuando el usuario entra a una ruta específica. También permite enlazar navegación con `routerLink`, usar parámetros en la URL y cargar contenido dentro de un `router-outlet`.

> [!tip] **¿Cuándo usarlo?**
> * Varias pantallas o secciones.
> * Navegación entre páginas.
> * Rutas con parámetros.
> * Protección de acceso con guards.
> * Lazy loading de módulos o componentes.

> [!important] **¿Por qué es útil?**
> Hace que la aplicación sea más rápida y fluida porque no recarga toda la página, solo cambia la parte que corresponde a la vista activa. También mantiene la URL organizada y permite compartir enlaces directos a pantallas específicas.

### 🛡️ Guards en Angular Router

> [!info] **¿Qué es un Guard?**
> Es una herramienta de seguridad y control de flujo que se utiliza en el sistema de rutas para decidir si un usuario tiene permiso o no de navegar hacia una pantalla en específico.

> [!abstract] **Funcionamiento:**
> * Si devuelve `true`, Angular renderiza el componente de la pantalla.
> * Si devuelve `false`, la navegación se cancela y el Guard puede redirigir al usuario (como a Login o "Acceso Denegado").

> [!abstract] **Tipos de Guards:**
> * **CanActivate:** Controla si se puede entrar a una ruta.
> * **CanDeactivate:** Controla si el usuario puede salir de la ruta actual (útil para "Tienes cambios sin guardar, ¿seguro que quieres salir?").
> * **CanMatch:** Controla si una ruta coincide y puede ser cargada, muy usado junto con Lazy Loading.

### 🧭 Formas de Navegar

> [!faq]+ **`routerLink`**
> Es la opción de plantilla. La usas en HTML cuando quieres navegar con un enlace o botón sin escribir lógica en TypeScript.
> ```html
> <a routerLink="/users">Usuarios</a>
> ```

> [!faq]+ **`router.navigate()`**
> Es la opción programática. La usas en TypeScript cuando la navegación depende de una condición, validación o lógica del componente.
> ```typescript
> this.router.navigate(['/users']);
> ```

> [!faq]+ **`router.navigateByUrl()`**
> También es programática, pero recibe la URL completa como string. Es útil cuando ya tienes la ruta exacta y quieres navegar de forma directa.
> ```typescript
> this.router.navigateByUrl('/users');
> ```

### 📺 RouterOutlet

> [!info]
> Es una directiva estructural que actúa como el "espacio reservado" o "pantalla de proyección" donde Angular renderiza el componente que corresponde a la URL activa.
> ```html
> <router-outlet></router-outlet>
> <!-- Los componentes enrutados van aquí -->
> ```

### 🎨 RouterLinkActive

> [!info]
> Es una directiva que alterna clases CSS para enlaces `RouterLink` activos según el `RouterState` actual.
> ```html
> <h1>Angular Router</h1>
> <nav>
>   <a routerLink="/todosList" routerLinkActive="active">List of todos</a>
>   <a routerLink="/completed" routerLinkActive="active">Completed todos</a>
> </nav>
> <router-outlet></router-outlet>
> ```

---

## ⚙️ Compilación y Herramientas

### 🔧 JIT vs AOT

> [!info] **¿Qué significa compilar en Angular?**
> Angular necesita convertir tus templates HTML y decoradores TypeScript en código JavaScript que el navegador pueda ejecutar.

> [!abstract] **Comparación JIT vs AOT**
> | Aspecto | JIT | AOT |
> | :--- | :--- | :--- |
> | **¿Cuándo compila?** | En el navegador | Antes del deploy |
> | **Bundle size** | Grande (incluye compilador) | Pequeño ✅ |
> | **Velocidad inicial** | Más lenta 🐌 | Más rápida ⚡ |
> | **Detección de errores** | En ejecución | En compilación ✅ |
> | **Seguridad** | Menor | Mayor ✅ |
> | **Uso recomendado** | Desarrollo | Producción ✅ |
> | **Desde Angular 9** | Opcional | Por defecto ✅ |

### 🛠️ CLI Angular

> [!info] **¿Qué es la CLI Angular?**
> Es una interfaz de línea de comandos para estructurar y crear aplicaciones angulares utilizando módulos de estilos Node.js.

> [!code] **Instalación**
> ```bash
> npm install @angular/cli@latest
> ```

> [!code] **Comandos principales**
> * `ng new` → Crea un nuevo proyecto
> * `ng generate class my-new-class` → Agrega una clase
> * `ng generate component my-new-component` → Agrega un componente
> * `ng generate directive my-new-directive` → Agrega una directiva
> * `ng generate enum my-new-enum` → Agrega una enumeración
> * `ng generate module my-new-module` → Agrega un módulo
> * `ng generate pipe my-new-pipe` → Agrega un pipe
> * `ng generate service my-new-service` → Agrega un servicio

### 📝 Macros

> [!info] **¿Qué son las macros?**
> Son funciones o métodos estáticos con una sola expresión de retorno que el compilador AOT puede evaluar en compilación. Se usan para simplificar y generar metadatos o configuración de manera estática.

> [!warning]
> Las macros son un concepto avanzado y específico. En la mayoría de proyectos Angular nunca necesitarás crearlas.

> [!code] **Ejemplo**
> ```typescript
> export function wrapInArray<T>(value: T): T[] {
>   return [value];
> }
> ```

### 🧪 TestBed

> [!info]
> TestBed es la utility de Angular que configura un entorno de testing completo (DI, módulos, compilación) para ejecutar pruebas unitarias de componentes, directivas, pipes y servicios de forma aislada.

### 🔍 trackBy en *ngFor

> [!info] **¿Cuál es el propósito de `trackBy`?**
> La función `trackBy` en `*ngFor` proporciona un identificador único para cada elemento de la lista, permitiendo a Angular detectar solo los cambios reales (agregados/eliminados) en lugar de reconstruir toda la lista.

> [!code] **Ejemplo**
> ```html
> <div *ngFor="let todo of todos; trackBy: trackByTodos">
>   ({{todo.id}}) {{todo.name}}
> </div>
> ```

> [!abstract] **Casos críticos donde lo necesitas:**
> | Escenario | Sin trackBy | Con trackBy |
> | :--- | :--- | :--- |
> | **Listas grandes** (>100 items) | ❌ Recreación total | ✅ Solo cambios |
> | **Filtros/búsquedas** | ❌ Parpadea toda la lista | ✅ Solo resultados nuevos |
> | **Arrays reordenados** | ❌ Reconstruye todo | ✅ Mantiene elementos |

---

## 💾 Persistencia de Datos

### 📦 LocalStorage vs SessionStorage

> [!faq]+ **LocalStorage (Persiste)**
> Es una forma de guardar datos en el navegador que no se borran cuando salimos de la página.

> [!faq]+ **SessionStorage**
> Es una forma de tener la vista y los recursos temporalmente si se inició sesión, pero una vez saliendo entonces la vista junto con toda la información también se va.

---

## 🗫 Preguntas de Comportamiento para Entrevistas

### 🟩 ¿Cómo mejorarías la experiencia del usuario en una aplicación web que ya está en producción?

> [!faq]+ **Respuesta**
> Primero identificaría dónde están los puntos de fricción, ya sea por feedback de usuarios o revisando qué procesos toman más tiempo. En Textiles por ejemplo, los operarios usaban la plataforma desde móviles, así que optimicé la interfaz para pantallas pequeñas y reduje los pasos para registrar un lote. En general me enfocaría en tiempos de carga, simplicidad en formularios y mensajes de error claros que guíen al usuario en lugar de confundirlo.

### 🟩 ¿Qué esperas de nosotros como empresa?

> [!faq]+ **Respuesta**
> Espero un ambiente donde pueda seguir aprendiendo, ya sea con retos técnicos reales o con retroalimentación del equipo. También espero claridad en los procesos y buena comunicación, porque en mis experiencias anteriores trabajar con metodologías ágiles me enseñó que la comunicación constante hace la diferencia. Y honestamente espero la oportunidad de crecer dentro de la empresa a medida que demuestre resultados.

### 🟩 ¿Prefieres trabajar solo o en equipo?

> [!faq]+ **Respuesta**
> Prefiero el trabajo en equipo porque permite dividir responsabilidades y avanzar más rápido. En Polihules lo viví con XP, donde cada quien tenía su área clara y eso agilizó mucho las entregas. Dicho eso, también puedo trabajar de forma autónoma cuando se necesita, en Textiles hubo etapas donde trabajé solo en la automatización de procesos y me organicé bien.

### 🟩 ¿Cómo manejas la frustración cuando algo no funciona?

> [!faq]+ **Respuesta**
> Primero me tomo unos minutos, salgo a tomar aire y me despejo. Después regreso con la mente fría y me enfoco en encontrar la raíz del problema, muchas veces volviendo a las bases. Una vez en Textiles estuve atorado configurando permisos por rol para ciertas vistas en Django, me frustré bastante. Me alejé un momento, volví, revisé la documentación oficial de Django desde cero y encontré que estaba aplicando el decorador en el lugar incorrecto. Lo resolví en una hora y de paso documenté la solución para el equipo.
