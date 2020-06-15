# clean-code-typescript [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Clean%20Code%20Typescript&url=https://github.com/labs42io/clean-code-typescript)

Conceptos del "Clean Code (código limpio)" adaptados a TypeScript.
Inspirado del repositorio [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript).
Traducción del repositorio [clean-code-typescript](https://github.com/labs42io/clean-code-typescript).

## Tabla de contenidos

  1. [Introducción](#introducción)
  2. [Variables](#variables)
  3. [Funciones](#funciones)
  4. [Objetos y estructura de datos](#objetos-y-estructura-de-datos)
  5. [Clases](#clases)
  6. [Sólido](#sólido)
  7. [Pruebas](#pruebas)
  8. [Concurrencia](#concurrencia)
  9. [Solución de errores](#solución-de-errores)
  10. [Formato](#formato)
  11. [Comentarios TODO](#comentarios-todo)
  12. [Traducciones](#traducciones)

## Introducción

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

"Principios de la ingeniería de software", extraídos del libro de Robert C. Martin's
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adaptados a TypeScript. Esta no es una guía de estilos. Es una guía para hacer softwares
[legibles, reusables y refactorizables](https://github.com/ryanmcdermott/3rs-of-software-architecture) utilizando TypeScript.

No todos los principios aquí mencionados tienen que ser estrictamente seguidos, menos aún tienen
que ser aceptados universalmente. Estas son tan solo directrices.
Directrices que se han definido a lo largo de muchos años de experiencia colectiva por los autores de
*Clean Code*.

Nuestro trabajo como ingenieros de software tiene poco más de 50 años. Aún seguimos
aprendiendo muchas cosas. Cuando la arquitectura de software sea tan antigua como
lo es la misma arquitectura, tal vez tendremos que seguir directrices más duras. Por ahora,
dejemos que estas directrices funcionen como punto para evaluar la calidad del
código de TypeScript que tú y tu equipo producen.

Una cosa más: saber esto no te hará un mejor desarrollador de software y haber
trabajado con esta tecnología por muchos años no quiere decir que no cometerás
errores. Cada pieza de código comienza como un borrador, como arcilla mojada
que se transforma luego en la forma deseada. Finalmente, limpiamos las imperfecciones
cuando las identificamos con la ayuda de nuestro equipo. No te mortifiques por códigos
en borrador que necesiten mejoras. En vez de eso, ¡enfócate en mejorar el código!

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Variables

### Usa nombres de variables que tengan sentido.

Distingue los nombres de las variables para que el lector sepa a qué se refieren.

**Mal:**

```ts
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}

```

**Bien:**

```ts
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Usa nombres de variables que sean pronunciables.

Si no puedes pronunciarlo, no puedes discutir sobre la variable sin sonar como idiota.

**Mal:**

```ts
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
}
```

**Bien:**

```ts
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Usa el mismo vocabulario para el mismo tipo de variable.

**Mal:**

```ts
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**Bien:**

```ts
function getUser(): User;
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Usa nombres que puedas encontrar fácilmente.

Leeremos más código del que escribiremos. Es importante que el código que escribamos sea legible y pueda ser encontrado con facilidad. Cuando nombramos variables con nombres sin sentido o que son difíciles de encontrar, nuestros lectores sufren. Elige un nombre que sea fácil de encontrar. Herramientas como [TSLint](https://palantir.github.io/tslint/rules/no-magic-numbers/) pueden ayudar a identificar constantes.

**Mal:**

```ts
// What the heck is 86400000 for?
setTimeout(restart, 86400000);
```

**Bien:**

```ts
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 24 * 60 * 60 * 1000;

setTimeout(restart, MILLISECONDS_IN_A_DAY);
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Usa nombres de variables que expliquen a qué se refieren.

**Mal:**

```ts
declare const users: Map<string, User>;

for (const keyValue of users) {
  // iterate through users map
}
```

**Bien:**

```ts
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // iterate through users map
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Evita el mapeo mental.

Lo explícito es mejor que lo implícito.
*La claridad es escencial.*

**Mal:**

```ts
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**Bien:**

```ts
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### No agregues contexto innecesario.

Si el nombre de tu clase/tipo/objeto expresa algo, no lo repitas en el nombre de tu variable.

**Mal:**

```ts
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
}

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**Bien:**

```ts
type Car = {
  make: string;
  model: string;
  color: string;
}

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Usa argumentos por defecto en lugar de condicionales.

Los argumentos por defecto son generalmente más legibles que las condicionales.

**Mal:**

```ts
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**Bien:**

```ts
function loadPages(count: number = 10) {
  // ...
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Utiliza "enum" para darle sentido al código.

Los "enums" pueden ayudarte a darle sentido al código. Por ejemplo, cuando nos preocupa que algunos valores sean
diferentes en lugar de que tengan el valor exacto.

**Mal:**

```ts
const GENRE = {
  ROMANTIC: 'romantic',
  DRAMA: 'drama',
  COMEDY: 'comedy',
  DOCUMENTARY: 'documentary',
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // some logic to be executed 
    }
  }
}
```

**Bien:**

```ts
enum GENRE {
  ROMANTIC,
  DRAMA,
  COMEDY,
  DOCUMENTARY,
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // some logic to be executed 
    }
  }
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Funciones

### Argumentos de las funciones (2 o menos idealmente).

Limitar la cantidad de los parámetros de las funciones es increíblemente importante porque hace que sea más fácil probar tus funciones.
Tener más de tres conduce a una explosión combinatoria donde tienes que probar una cantidad muy grande de casos diferentes con cada
argumento por separado. 

Uno o dos argumentos es lo ideal. Deberías evitar tres argumentos en la medida de lo posible. Cualquier cosa más que eso debería ser
consolidada.
Usualmente, si tienes más de dos argumentos, tu función está intendando hacer más de lo que puede.
En casos en los que no sea así, la mayoría de las veces bastará como un objeto de nivel superior como argumento.

Considera el uso de objetos literales si necesitas muchos argumentos. 

Para que sea obvio cuales propiedades son las que la función espera, puedes usar la sintaxis ["destructuring (destructuración)"](https://basarat.gitbook.io/typescript/future-javascript/destructuring).
Esto tiene algunas ventajas:

1. Cuando alguien lee la firma de una función, inmediatamente queda claro qué propiedades se están utilizando.

2. Puede ser utilizado para simular parámetros con nombres.

3. La destructuración también clona los valores del primitivo específico del objeto del argumento pasado en la función. Esto ayuda a prevenir efectos secundarios. Nota: los objetos y los arreglos que son destructurados del objeto del argumento NO SON clonados.

4. TypeScript te avisa sobre propiedades no utilizadas, cosa que sería imposible sin la destructuración.

**Mal:**

```ts
function createMenu(title: string, body: string, buttonText: string, cancellable: boolean) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Bien:**

```ts
function createMenu(options: { title: string, body: string, buttonText: string, cancellable: boolean }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

Puedes mejorar aún más la legibilidad utilizando ["type aliases (tipos de alias)"](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases):

```ts

type MenuOptions = { title: string, body: string, buttonText: string, cancellable: boolean };

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Las funciones deben realizar solo una cosa.

Esta es sin duda la regla más importante en la ingeniería de software. Cuando las funciones realizan más de una cosa, se vuelven más difíciles de componer, probar y razonar. Cuando puedes aislar una función a una sola cosa, esta puede ser fácilmente refactorizada y tu código se verá mucho más limpio. Si tan solo tomaras esta declaración de esta guía, estarías por delante de muchos desarrolladores.

**Mal:**

```ts
function emailClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Bien:**

```ts
function emailClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Los nombres de las funciones deben expresar qué accion ejercen.

**Mal:**

```ts
function addToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Bien:**

```ts
function addMonthToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();
addMonthToDate(date, 1);
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Las funciones deben ser de solo un nivel de abstracción.

Cuando tienes más de un nivel de abstracción tu función usualmente estará sobrecargada. Partir las funciones provee reutilización y a la vez son más fáciles de probar.

**Mal:**

```ts
function parseCode(code: string) {
  const REGEXES = [ /* ... */ ];
  const statements = code.split(' ');
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Bien:**

```ts
const REGEXES = [ /* ... */ ];

function parseCode(code: string) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code: string): Token[] {
  const statements = code.split(' ');
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function parse(tokens: Token[]): SyntaxTree {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push( /* ... */ );
  });

  return syntaxTree;
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Elimina código que esté duplicado.

Enfócate en evitar código duplicado.
El código que está duplicado es malo ya que hay un trozo de código más que modificar si necesitas cambiar algo.

Imagina que tienes un restaurante y controlas tu inventario: todos los tomates, las cebollas, las especias, etc.
Si tienes múltiples listas en las que está esta información, tendrás que actualizarlas todas cuando sirves un plato con tomates.
Si solo tienes una lista, ¡solo tendrás que actualizarla en un mismo lugar!

A veces tienes piezas de código duplicado porque difieren en pequeñas cosas, que comparten mucho en común, pero que sus diferencias te fuerzan a tener dos o más funciones separadas que hacen mucho de las mismas cosas. Eliminar el código duplicado significa crear una abstracción que pueda soportar este conjunto de cosas diferentes con tan solo una función, módulo o clase.

Tener la abstracción correcta es escencial, es por esto que deberías seguir los principios ["Solid (sólido)"](#sólido). Las malas abstracciones pueden ser peores que el código duplicado, así que, ¡ten cuidado! Habiendo dicho esto, si puedes hacer una buena abstracción, ¡hazlo! No repitas el código, si no, te encontrarás teniendo que actualizar el código en diferentes lugares cada vez que quieras cambiar tan solo una cosa.

**Mal:**

```ts
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Bien:**

```ts
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    }
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    }
  }
}

function showEmployeeList(employee: Developer | Manager) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

Deberías ser crítico sobre el código duplicado. A veces hay más intercambio entre código duplicado y se vuelve más complejo cuando introduces una abstracción innecesaria. Cuando dos implementaciones de dos módulos diferentes se ven similares pero se encuentran en diferentes dominios, duplicar el código es aceptado y preferible a la extracción del código común. El código común extraído en este caso introduce una dependencia indirecta entre los dos módulos.

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Establece objetos predeterminados con "Object.assign" o destructurando.

**Mal:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: 'Bar' });
```

**Bien:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // ...
}

createMenu({ body: 'Bar' });
```

Alternativamente, puedes destructurar con valores por defecto:

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu({ title = 'Foo', body = 'Bar', buttonText = 'Baz', cancellable = true }: MenuConfig) {
  // ...
}

createMenu({ body: 'Bar' });
```

Para evitar cualquier efecto secundario o comportamiento inesperado pasando explícitamente el valor `undefined` o `null`, puedes decirle al compilador de TypeScript que no lo permita.
Lee la opción [`--strictNullChecks`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks) en TypeScript.

**[⬆ volver al inicio](#tabla-de-contenidos)**

### No uses indicadores como parámetros de funciones.

Las indicaciones le dicen al usuario que una función hace más que una sola cosa.
Las funciones deben hacer solo una cosa. Divide tus funciones si siguen diferentes rutas de código basadas en boolean (booleano).

**Mal:**

```ts
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Bien:**

```ts
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Evita efectos secundarios (parte 1).

Una función produce un efecto secundario si no hace más que tomar un valor y devolver otro(s) valor(es).
Un efecto secundario pudiera ser: modificar un archivo, modificar una variable global o accidentalmente enviar todo tu dinero a un extraño.

Ahora, necesitas tener efectos secundarios en un programa en ciertas ocasiones. Al igual que el ejemplo anterior, tal vez tendrás que modificar un archivo. Lo ideal es centralizar dónde estás haciendo esto. No tengas demasiadas funciones y clases que modifiquen un archivo en particular.
Ten un servicio que lo haga. Solo uno.

El punto principal es evitar problemas como compartir el estado entre objetos sin ninguna estructura, usatilizando tipos de datos mutables que puedan ser escritos por cualquier cosa y no centralizar dónde ocurren los efectos secundarios. Si puedes hacer esto, serás más feliz que la gran mayoría de los demás programadores.

**Mal:**

```ts
// Global variable referenced by following function.
let name = 'Robert C. Martin';

function toBase64() {
  name = btoa(name);
}

toBase64();
// If we had another function that used this name, now it'd be a Base64 value

console.log(name); // expected to print 'Robert C. Martin' but instead 'Um9iZXJ0IEMuIE1hcnRpbg=='
```

**Bien:**

```ts
const name = 'Robert C. Martin';

function toBase64(text: string): string {
  return btoa(text);
}

const encodedName = toBase64(name);
console.log(name);
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Evita efectos secundarios (parte 2).

En JavaScript, los primitivos son pasados por valor y los objetos y arreglos son pasados por referencia. En el caso de objetos y arreglos, si tu función hace un cambio en el arreglo de un carrito de compra, por ejemplo, añadiéndole un objeto, cualquier otra función que use ese arreglo de un carrito de compra `cart` será afectado por esta adición. Eso puede ser bueno pero a la vez puede ser malo. Imaginemos una mala situación:

El usuario hace click en el botón de "Comprar", que llama a la función `purchase` que aparece, esta es solicitada y envía el arreglo `cart` al servidor. Si hay una mala conexión con el servidor, la función de compra tiene que seguir intentando solicitar la información. Ahora, ¿que ocurre si mientras tanto el usuario hace click accidentalmente al botón "Añadir al carrito" en un artículo que no quiere antes de que la solicitud de la conexión comience? Si eso ocurre y la solicitud de la conexión empieza, la función de esa compra enviará el artículo añadido accidentalmente ya que está referenciado al arreglo del carrito de la compra que la función `addItemToCart` ha modificado añadiendo el item no deseado.

Una buena solución podria ser que la función `addItemToCart` siempre clone el `cart`, lo edite y devuelva la copia. Esto asegura que ninguna otra función referenciada al carrito de compra será afectada por algún cambio.

Dos advertencias que mencionar:

1. Pueden haber casos en los que quieres modificar el objeto enviado, pero cuando adoptas esta práctica de programación te darás cuenta que esos casos son muy raros. ¡La mayoría de cosas pueden ser refactorizadas para que no ocurra ningún efecto secundario! (ve [pure function](https://en.wikipedia.org/wiki/Pure_function))

2. Clonar objetos grandes puede ser muy caro en temas de rendimiento. Por suerte, este no es un gran problema en la práctica ya que hay grandiosas librerías que permiten este tipo de usos para que sean rápidos y no consuman tanta memoria como lo sería clonar objetos y arreglos manualmente.

**Mal:**

```ts
function addItemToCart(cart: CartItem[], item: Item): void {
  cart.push({ item, date: Date.now() });
};
```

**Bien:**

```ts
function addItemToCart(cart: CartItem[], item: Item): CartItem[] {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### No escribas a funciones globales.

Contaminar las funciones globales es una mala práctica en JavaScript ya que podría interferir con otra librería y el usuario de tu API no sería el más afortunado hasta que consiga una excepción en la producción. Imaginemos un ejemplo: ¿qué pasa si quisieras extender el método de arreglo nativo de JavaScript para tener un método (`diff`) que pudiera mostrarte la diferencia entre dos arreglos? Podrías escribir una nueva función con `Array.prototype`, pero que esta pudiera interferir con otra librería que intentara hacer lo mismo. ¿Qué ocurre entonces si esa librería simplemente estuviera utilizando `diff` para encontrar la diferencia entre el primero y el último elemento dentro de un arreglo? Es por esto que sería mucho mejor tan solo utilizar clases y extender el `Array`(arreglo) global.

**Mal:**

```ts
declare global {
  interface Array<T> {
    diff(other: T[]): Array<T>;
  }
}

if (!Array.prototype.diff) {
  Array.prototype.diff = function <T>(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**Bien:**

```ts
class MyArray<T> extends Array<T> {
  diff(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Favorece a la programación funcional sobre la programación imperativa.

Favorece este estilo de programación siempre que puedas.

**Mal:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < contributions.length; i++) {
  totalOutput += contributions[i].linesOfCode;
}
```

**Bien:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const totalOutput = contributions
  .reduce((totalLines, output) => totalLines + output.linesOfCode, 0);
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Encapsula condicionales.

**Mal:**

```ts
if (subscription.isTrial || account.balance > 0) {
  // ...
}
```

**Bien:**

```ts
function canActivateService(subscription: Subscription, account: Account) {
  return subscription.isTrial || account.balance > 0;
}

if (canActivateService(subscription, account)) {
  // ...
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Evita condicionales negativas.

**Mal:**

```ts
function isEmailNotUsed(email: string): boolean {
  // ...
}

if (isEmailNotUsed(email)) {
  // ...
}
```

**Bien:**

```ts
function isEmailUsed(email: string): boolean {
  // ...
}

if (!isEmailUsed(node)) {
  // ...
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Evita las condicionales.

Esta parece una tarea imposible. Cuando las personas escuchan esto por primera vez, la mayoría dice, "¿cómo se supone que haga algo sin una declaración `if`?" La respuesta es: puedes utilizar el polimorfismo para lograr la misma tarea en muchos casos. La segunda pregunta es una muy usual, "bueno, eso suena bien, pero, ¿por qué lo haría así?" La respuesta es un concepto del código limpio que ya hemos aprendido anteriormente: las funciones deberían hacer una sola cosa. Cuando tienes clases y funciones que tienen declaraciones `if`, estás expresando que tu función hace más de una cosa. Recuerda, solo una cosa.

**Mal:**

```ts
class Airplane {
  private type: string;
  // ...

  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
      default:
        throw new Error('Unknown airplane type.');
    }
  }

  private getMaxAltitude(): number {
    // ...
  }
}
```

**Bien:**

```ts
abstract class Airplane {
  protected getMaxAltitude(): number {
    // shared logic with subclasses ...
  }

  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Evita la comprobación del tipo de letra.

TypeScript es un superconjunto de JavaScript estricto sintácticamente que añade la comprobación del tipo de letra. Siempre es preferible especificar tipos de variables, parámetros y devolver valores para aprovechar la máxima potencia de las funcionalidades de TypeScript. Esto hace que la refactorización sea más simple.

**Mal:**

```ts
function travelToTexas(vehicle: Bicycle | Car) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(currentLocation, new Location('texas'));
  }
}
```

**Bien:**

```ts
type Vehicle = Bicycle | Car;

function travelToTexas(vehicle: Vehicle) {
  vehicle.move(currentLocation, new Location('texas'));
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### No sobre-optimices.

Los navegadores modernos hacen un montón de optimización por detrás de la pantalla cuando estos se están ejecutando. Muchas veces, cuando estás "optimizando", la realidad es que estás gastando tu tiempo. Hay buenos [recursos](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) para ver dónde falta optimización. Anota estas faltas mientras tanto, al menos hasta que sean arregladas (si es que pueden serlo).

**Mal:**

```ts
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Bien:**

```ts
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Elimina código que no se use.

El código que no se usa es tan malo como el código duplicado. No hay razón por la que dejar código sin usar en tu archivo.
Si no está siento llamado, ¡elimínalo! Todavía estará guardado en la historia del control de versiones si aún lo necesitas.

**Mal:**

```ts
function oldRequestModule(url: string) {
  // ...
}

function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**Bien:**

```ts
function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Usa iteradores y generadores.

Usa generadores e iterables cuando estés trabajando con conjuntos de datos utilizados como flujo.
Hay buenas razones para esto:

- Separa el "callee" de la implementación del generador para lograr que el "callee" decida a cuantos artículos acceder.
- Ejecución perezosa. Los artículos son transmitidos cuando se necesiten.
- Sistema de soporte integrado para iterar artículos utilizando `for-of`.
- Los iterables permiten implementar patrones de iteradores optimizados.

**Mal:**

```ts
function fibonacci(n: number): number[] {
  if (n === 1) return [0];
  if (n === 2) return [0, 1];

  const items: number[] = [0, 1];
  while (items.length < n) {
    items.push(items[items.length - 2] + items[items.length - 1]);
  }

  return items;
}

function print(n: number) {
  fibonacci(n).forEach(fib => console.log(fib));
}

// Print first 10 Fibonacci numbers.
print(10);
```

**Bien:**

```ts
// Generates an infinite stream of Fibonacci numbers.
// The generator doesn't keep the array of all numbers.
function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

function print(n: number) {
  let i = 0;
  for (const fib of fibonacci()) {
    if (i++ === n) break;  
    console.log(fib);
  }  
}

// Print first 10 Fibonacci numbers.
print(10);
```

Hay librerías que permiten trabajar con iterables de forma similar a los arreglos nativos al concatenar métodos como `map`, `slice`, `forEach`, etc. Ve [itiriri](https://www.npmjs.com/package/itiriri) para
un ejemplo de manipulación avanzada con iterables (o [itiriri-async](https://www.npmjs.com/package/itiriri-async) para un ejemplo de manipulación de iterables async).

```ts
import itiriri from 'itiriri';

function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];
 
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

itiriri(fibonacci())
  .take(10)
  .forEach(fib => console.log(fib));
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Objetos y estructura de datos

### Usa "getters" y "setters".

TypeScript soporta la sintaxis getter/setter.
Utilizar getters y setters para acceder a datos desde objetos que encapsulan el comportamiento podría ser mejor que simplemente buscar una propiedad de un objeto.
"¿Por qué?" preguntarás. Bueno, aquí hay una lista de razones:

- Cuando quieres hacer más que obtener la propiedad de un objeto, no tienes que cambiar cada acesor en el código de tu archivo.
- Valida de una manera simple utilizando `set`.
- Encapsula la representación interna.
- Fácil de añadir un registro y manejo de errores.
- Puedes cargar las propiedades de tus objetos, como por ejemplo, cargándolas desde un servidor.

**Mal:**

```ts
type BankAccount = {
  balance: number;
  // ...
}

const value = 100;
const account: BankAccount = {
  balance: 0,
  // ...
};

if (value < 0) {
  throw new Error('Cannot set negative balance.');
}

account.balance = value;
```

**Bien:**

```ts
class BankAccount {
  private accountBalance: number = 0;

  get balance(): number {
    return this.accountBalance;
  }

  set balance(value: number) {
    if (value < 0) {
      throw new Error('Cannot set negative balance.');
    }

    this.accountBalance = value;
  }

  // ...
}

// Now `BankAccount` encapsulates the validation logic.
// If one day the specifications change, and we need extra validation rule,
// we would have to alter only the `setter` implementation,
// leaving all dependent code unchanged.
const account = new BankAccount();
account.balance = 100;
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Haz que los objetos tengan miembros privados o protegidos.

TypeScript soporta los acesores `public` *(por defecto)*, `protected` y `private` en los miembros de la clase.

**Mal:**

```ts
class Circle {
  radius: number;
  
  constructor(radius: number) {
    this.radius = radius;
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**Bien:**

```ts
class Circle {
  constructor(private readonly radius: number) {
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Es preferible la inmutabilidad.

El sistema de tipo de TypeScript te permite marcar propiedades individuales en un interfaz/clase como *readonly*. Esto te permite trabajar de una manera funcional (la mutación inesperada es mala).
Para escenarios más avanzados hay un tipo de `Readonly` integrado que toma el tipo `T` y marca todas sus propiedades como "readonly" utilizando el mapeado de tipos (ve [mapped types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)).

**Mal:**

```ts
interface Config {
  host: string;
  port: string;
  db: string;
}
```

**Bien:**

```ts
interface Config {
  readonly host: string;
  readonly port: string;
  readonly db: string;
}
```

En caso de un arreglo, puedes crear un arreglo "read-only" utilizando `ReadonlyArray<T>`.
No permitas cambios como `push()` y `fill()`, puedes usar funciones como `concat()` y `slice()` ya que estas no cambian el valor.

**Mal:**

```ts
const array: number[] = [ 1, 3, 5 ];
array = []; // error
array.push(100); // array will updated
```

**Bien:**

```ts
const array: ReadonlyArray<number> = [ 1, 3, 5 ];
array = []; // error
array.push(100); // error
```

Declarar argumentos "read-only" en [TypeScript 3.4 es un poco más fácil](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#improvements-for-readonlyarray-and-readonly-tuples).

```ts
function hoge(args: readonly string[]) {
  args.push(1); // error
}
```

Son preferibles las [const assertions](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#const-assertions) para valores literales.

**Mal:**

```ts
const config = {
  hello: 'world'
};
config.hello = 'world'; // value is changed

const array  = [ 1, 3, 5 ];
array[0] = 10; // value is changed

// writable objects is returned
function readonlyData(value: number) {
  return { value };
}

const result = readonlyData(100);
result.value = 200; // value is changed
```

**Bien:**

```ts
// read-only object
const config = {
  hello: 'world'
} as const;
config.hello = 'world'; // error

// read-only array
const array  = [ 1, 3, 5 ] as const;
array[0] = 10; // error

// You can return read-only objects
function readonlyData(value: number) {
  return { value } as const;
}

const result = readonlyData(100);
result.value = 200; // error
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Tipo vs. interfaz.

Usa tipo cuando necesites una unión o intersección. Usa interfaz cuando necesites `extends` o `implements`. No hay ninguna regla estricta, usa la que te funcione mejor.
Para una explicación más detallada referente a esta [respuesta](https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/54101543#54101543) sobre las diferencias entre `type` e `interface` en TypeScript.

**Mal:**

```ts
interface EmailConfig {
  // ...
}

interface DbConfig {
  // ...
}

interface Config {
  // ...
}

//...

type Shape = {
  // ...
}
```

**Bien:**

```ts

type EmailConfig = {
  // ...
}

type DbConfig = {
  // ...
}

type Config  = EmailConfig | DbConfig;

// ...

interface Shape {
  // ...
}

class Circle implements Shape {
  // ...
}

class Square implements Shape {
  // ...
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Clases

### Las clases deberían ser pequeñas.

El tamaño de la clase es medido por su responsabilidad. Siguiendo el *Principio de responsabilidad única*, una clase debería ser pequeña.

**Mal:**

```ts
class Dashboard {
  getLanguage(): string { /* ... */ }
  setLanguage(language: string): void { /* ... */ }
  showProgress(): void { /* ... */ }
  hideProgress(): void { /* ... */ }
  isDirty(): boolean { /* ... */ }
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  addSubscription(subscription: Subscription): void { /* ... */ }
  removeSubscription(subscription: Subscription): void { /* ... */ }
  addUser(user: User): void { /* ... */ }
  removeUser(user: User): void { /* ... */ }
  goToHomePage(): void { /* ... */ }
  updateProfile(details: UserDetails): void { /* ... */ }
  getVersion(): string { /* ... */ }
  // ...
}

```

**Bien:**

```ts
class Dashboard {
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  getVersion(): string { /* ... */ }
}

// split the responsibilities by moving the remaining methods to other classes
// ...
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Alta cohesión y bajo acoplamiento.

La cohesión define el grado en el que los miembros de la clase están relacionados los unos con los otros. Idealmente, todos los campos entre una clase deberían ser utilizados por cada método.
Decimos entonces que la clase es *masivamente cohesiva*. Sin embargo, en la práctica, esto no es siempre posible, ni siquiera es aconsejable. Aún así, es preferible que la cohesión sea alta.

El acoplamiento se refiere a cómo se relacionan o dependen dos clases entre sí. Se dice que las clases están en bajo acoplamiento si los cambios en una de ellas no afecta a la otra.
  
Un buen diseño de software tiene **alta cohesión** y **bajo acoplamiento**.

**Mal:**

```ts
class UserManager {
  // Bad: each private variable is used by one or another group of methods.
  // It makes clear evidence that the class is holding more than a single responsibility.
  // If I need only to create the service to get the transactions for a user,
  // I'm still forced to pass and instance of `emailSender`.
  constructor(
    private readonly db: Database,
    private readonly emailSender: EmailSender) {
  }

  async getUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await db.transactions.find({ userId });
  }

  async sendGreeting(): Promise<void> {
    await emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**Bien:**

```ts
class UserService {
  constructor(private readonly db: Database) {
  }

  async getUser(id: number): Promise<User> {
    return await this.db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await this.db.transactions.find({ userId });
  }
}

class UserNotifier {
  constructor(private readonly emailSender: EmailSender) {
  }

  async sendGreeting(): Promise<void> {
    await this.emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await this.emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Es más preferible la composición que la herencia.

Como está estipulado en [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) por "Gang of Four", *deberías preferir la composición sobre la herencia* siempre que puedas. Hay muchas buenas razones para usar la herencia al igual que hay muchas buenas razones para usar la composición. El punto principal es que si tu mente instintivamente decide ir por la herencia, trata de pensar como si la composición pudiera solucionar mejor el problema. En algunos casos es así.

Debes de preguntarte, "¿cuando debería utilizar la herencia?" Depende del problema, pero aquí hay una decente lista donde puedes ver cuando la herencia tiene más sentido que la composición:

1. Tu herencia representa una relación (es) y no una relación (tiene) (Humano->Animal vs. Usuario->DetallesDelUsuario).

2. Puedes reusar el código desde la base de las clases (Los humanos pueden moverse como todos los animales).

3. Quieres hacer cambios globales a clases derivadas cambiando la base de la clase (Cambia el gasto calórico de todos los animales cuando se mueven).

**Mal:**

```ts
class Employee {
  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(
    name: string,
    email: string,
    private readonly ssn: string,
    private readonly salary: number) {
    super(name, email);
  }

  // ...
}
```

**Bien:**

```ts
class Employee {
  private taxData: EmployeeTaxData;

  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  setTaxData(ssn: string, salary: number): Employee {
    this.taxData = new EmployeeTaxData(ssn, salary);
    return this;
  }

  // ...
}

class EmployeeTaxData {
  constructor(
    public readonly ssn: string,
    public readonly salary: number) {
  }

  // ...
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Usa el método de encadenamiento.

Este patrón es muy útil y muy usado en muchas librerías. Permite que tu código sea expresivo y menos ampuloso. Por esa razón, usa el método de encadenamiento y observa que limpio será tu código.

**Mal:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): void {
    this.collection = collection;
  }

  page(number: number, itemsPerPage: number = 100): void {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
  }

  orderBy(...fields: string[]): void {
    this.orderByFields = fields;
  }

  build(): Query {
    // ...
  }
}

// ...

const queryBuilder = new QueryBuilder();
queryBuilder.from('users');
queryBuilder.page(1, 100);
queryBuilder.orderBy('firstName', 'lastName');

const query = queryBuilder.build();
```

**Bien:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): this {
    this.collection = collection;
    return this;
  }

  page(number: number, itemsPerPage: number = 100): this {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
    return this;
  }

  orderBy(...fields: string[]): this {
    this.orderByFields = fields;
    return this;
  }

  build(): Query {
    // ...
  }
}

// ...

const query = new QueryBuilder()
  .from('users')
  .page(1, 100)
  .orderBy('firstName', 'lastName')
  .build();
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Sólido

### Principio de responsabilidad único (Single Responsibility Principle (SRP)).

Como se declara en el código limpio, "Nunca debe haber más de una razón para cambiar una clase". Es tentador llenar una clase con muchas funcionalidades, como cuando solo puedes llevar una maleta en tu vuelo. El problema con esto es que tu clase no será conceptualmente cohesiva y habrán muchas razones para cambiarla. Minimizar la cantidad de veces que necesitas cambiar una clase es importante. Es importante ya que si hay muchas funcionalidades en una clase y modificas una parte de ella, puede ser difícil de entender cómo afectará a otros módulos dependientes en tu código.

**Mal:**

```ts
class UserSettings {
  constructor(private readonly user: User) {
  }

  changeSettings(settings: UserSettings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Bien:**

```ts
class UserAuth {
  constructor(private readonly user: User) {
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  private readonly auth: UserAuth;

  constructor(private readonly user: User) {
    this.auth = new UserAuth(user);
  }

  changeSettings(settings: UserSettings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Principio abierto/cerrado (Open/Closed Principle (OCP))

Como lo declara Bertrand Meyer, "las entidades del software (clases, módulos, funciones, etc.) deberían estar abiertas para su extensión, pero cerradas a modificación". Pero, ¿qué quiere decir esa oración? Este principio básicamente declara que deberías permitir a los usuarios que añadan nuevas funcionalidades sin cambiar código existente.

**Mal:**

```ts
class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    if (this.adapter instanceof AjaxAdapter) {
      const response = await makeAjaxCall<T>(url);
      // transform response and return
    } else if (this.adapter instanceof NodeAdapter) {
      const response = await makeHttpCall<T>(url);
      // transform response and return
    }
  }
}

function makeAjaxCall<T>(url: string): Promise<T> {
  // request and return promise
}

function makeHttpCall<T>(url: string): Promise<T> {
  // request and return promise
}
```

**Bien:**

```ts
abstract class Adapter {
  abstract async request<T>(url: string): Promise<T>;

  // code shared to subclasses ...
}

class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // request and return promise
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // request and return promise
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    const response = await this.adapter.request<T>(url);
    // transform response and return
  }
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Principio de sustitución Liskov (Liskov Substitution Principle (LSP))

Este es un término muy temido por un simple concepto. Está formalmente definido como "Si S es un subtipo de T, entonces los objetos de tipo T podrían ser reemplazados por objetos de tipo S (ej.: objetos de tipo S podrías sustituir objetos de tipo T) sin alterar ninguna propiedad deseada del programa (la correción, la tarea realizada, etc.)." Esa es una definición aún más aterradora.

La mejor explicación para esto es que si tienes una clase padre y una clase hijo, la clase padre y la clase hijo pueden ser usadas indistintamente sin obtener resultados incorrectos. Esto aún puede ser confuso, así que veamos el clásico ejemplo del Cuadrado-Rectpangulo (Square-Rectangle). Matemáticamente, un cuadrado es un rectángulo, pero si lo modelas usando la relación "es" a través de la herencia, rápidamente te encontrarás con problemas.

**Mal:**

```ts
class Rectangle {
  constructor(
    protected width: number = 0,
    protected height: number = 0) {

  }

  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  setWidth(width: number): this {
    this.width = width;
    return this;
  }

  setHeight(height: number): this {
    this.height = height;
    return this;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width: number): this {
    this.width = width;
    this.height = width;
    return this;
  }

  setHeight(height: number): this {
    this.width = height;
    this.height = height;
    return this;
  }
}

function renderLargeRectangles(rectangles: Rectangle[]) {
  rectangles.forEach((rectangle) => {
    const area = rectangle
      .setWidth(4)
      .setHeight(5)
      .getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Bien:**

```ts
abstract class Shape {
  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  abstract getArea(): number;
}

class Rectangle extends Shape {
  constructor(
    private readonly width = 0,
    private readonly height = 0) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(private readonly length: number) {
    super();
  }

  getArea(): number {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes: Shape[]) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Principio de segregación de interfaz (Interface Segregation Principle (ISP))

El ISP declara que "Los clientes no deben ser forzados a depender de interfaces que ellos no utilizan.". Este principio se relaciona mucho con el Principio de responsabilidad único.
Lo que quiere decir en realidad es que deberías siempre diseñar tus abstracciones de manera que los clientes que estén utilizando los métodos expuestos no estén recibiendo el pie completo. Esto también incluye imponer a los clientes la carga de implementar métodos que ellos no necesitan realmente.

**Mal:**

```ts
interface SmartPrinter {
  print();
  fax();
  scan();
}

class AllInOnePrinter implements SmartPrinter {
  print() {
    // ...
  }  
  
  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements SmartPrinter {
  print() {
    // ...
  }  
  
  fax() {
    throw new Error('Fax not supported.');
  }

  scan() {
    throw new Error('Scan not supported.');
  }
}
```

**Bien:**

```ts
interface Printer {
  print();
}

interface Fax {
  fax();
}

interface Scanner {
  scan();
}

class AllInOnePrinter implements Printer, Fax, Scanner {
  print() {
    // ...
  }  
  
  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements Printer {
  print() {
    // ...
  }
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Principio de inversión dependiente (Dependency Inversion Principle (DIP))

Este principio declara dos cosas escenciales:

1. Los módulos de alto nivel no deberían depender de módulos de bajo nivel. Ambos deberían depender de las abstracciones.

2. Las abstracciones no deberían depender de los detalles. Los detalles deberían depender de las abstracciones.

Esto puede ser difícil de entender al principio, pero si has trabajado con Angular, has visto la implementación de este principio en la forma de Inyección dependiente (Dependency Injection (DI)). Aunque estos no son los mismos conceptos, el DIP mantiene módulos de alto nivel que conocen los detalles de sus módulos de bajo nivel y los configura. Se puede lograr esto a través de una DI. Un gran beneficio de esto es que reduce el emparejamiento entre módulos. El emparejamiento es un muy mal patrón de desarrollo ya que hace a tu código más difícil de refactorizar.

El DIP se logra usualmente mediante el uso de un contenedor de inversión de control (IoC). Un ejemplo de un IoC muy poderoso para TypeScript es [InversifyJs](https://www.npmjs.com/package/inversify).

**Mal:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

class XmlFormatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}

class ReportReader {

  // BAD: We have created a dependency on a specific request implementation.
  // We should just have ReportReader depend on a parse method: `parse`
  private readonly formatter = new XmlFormatter();

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader();
await report = await reader.read('report.xml');
```

**Bien:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

interface Formatter {
  parse<T>(content: string): T;
}

class XmlFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}


class JsonFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts a JSON string to an object T
  }
}

class ReportReader {
  constructor(private readonly formatter: Formatter) {
  }

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader(new XmlFormatter());
await report = await reader.read('report.xml');

// or if we had to read a json report
const reader = new ReportReader(new JsonFormatter());
await report = await reader.read('report.json');
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Pruebas

Las pruebas son más importantes que el envío. Si no tienes pruebas o una cantidad inadecuada, cada vez que envies código no estarás seguro que no rompiste nada.
Decidir lo que constituye una cantidad adecuada depende de tu equipo, pero tener el 100% de cobertura (todas las declaraciones y las ramas)
es como se logra una muy alta confianza y la paz mental del desarrollador. Esto significa que, aparte de tener un buen framework de pruebas, también necesitas usar una buena [herramienta de cobertura](https://github.com/gotwarlost/istanbul).

No hay excusa para no escribir pruebas. Hay muchos [frameworks de pruebas con JS](http://jstherightway.org/#testing-tools) con soporte para TypeScript, así que encuentra uno que a tu equipo le guste. Cuando encuentres uno que funcione para tu equipo, escribe pruebas para cada mejora/módulo nuevo que introducen. Si tu método preferido es el Desarrollo de pruebas controlado (Test Driven Development (TDD)), eso está bien, pero el punto principal es asegurarte que cubras tus metas antes de publicar una mejora o de refactorizar una existente.

### Las tres leyes del TDD.

1. No puedes escribir ningún código de producción a menos que sea para aprobar la prueba de una unidad.

2. No puedes escribir más de una unidad de prueba hasta que sea suficiente para que falle; los fallos de compilación siguen siendo fallos.

3. No puedes escribir más código de producción de lo que es suficiente para pasar una prueba única de unidad que esté fallando.

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Reglas F.I.R.S.T. (R.I.R.A.O en español).

El código limpio debe seguir las reglas:

- **Fast (Rápido)**. Las pruebas rápidas deberían ser rápidas porque queremos ejecutarlas frecuentemente.

- **Independent (Independiente)**. Las pruebas independientes no deberían depender unas de las otras. Ellas deberían proporcionar el mismo resultado independientemente de si se ejecutan todas juntas en cualquier orden.

- **Repeatable (Repetitivo)**. Las pruebas deberían ser repetitivas en cualquier entorno y no debería haber ninguna excusa para fallar.

- **Self-Validating (Autovalidación)**. Autovalidar una prueba debería ser respondida con *Pasada* o *Fallida*. No necesitas comparar los archivos de registro para saber si una prueba fue pasada.

- **Timely (Oportuno)**. Las pruebas oportunas deberían ser esritas antes del código de producción. Si escribes las pruebas antes del código de producción, tal vez te encuentres con que escribir las pruebas será muy difícil.

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Un solo concepto por prueba.

Las pruebas también deben seguir el Principio de responsabilidad único. Realiza solo un acierto por prueba.

**Mal:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles date boundaries', () => {
    let date: AwesomeDate;

    date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));

    date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));

    date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**Bien:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles 30-day months', () => {
    const date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));
  });

  it('handles leap year', () => {
    const date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));
  });

  it('handles non-leap year', () => {
    const date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### El nombre de la prueba debe revelar cual es su intención.

Cuando una prueba falla, su nombre es la primera indicación de qué puede haber salido mal.

**Mal:**

```ts
describe('Calendar', () => {
  it('2/29/2020', () => {
    // ...
  });

  it('throws', () => {
    // ...
  });
});
```

**Bien:**

```ts
describe('Calendar', () => {
  it('should handle leap year', () => {
    // ...
  });

  it('should throw when format is invalid', () => {
    // ...
  });
});
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Concurrencia

### Son preferibles las promesas a las "callbacks".

Las "Callbacks" no son limpias, y, pueden causar anidación excesiva *(el infierno de las "Callbacks")*.
Hay utilidades que transforman funciones existentes utilizando el estilo callback a una versión que retorna promesas
(para Node.js ve [`util.promisify`](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original), para propósitos generales ve [pify](https://www.npmjs.com/package/pify), [es6-promisify](https://www.npmjs.com/package/es6-promisify))

**Mal:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';

function downloadPage(url: string, saveTo: string, callback: (error: Error, content?: string) => void) {
  get(url, (error, response) => {
    if (error) {
      callback(error);
    } else {
      writeFile(saveTo, response.body, (error) => {
        if (error) {
          callback(error);
        } else {
          callback(null, response.body);
        }
      });
    }
  });
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html', (error, content) => {
  if (error) {
    console.error(error);
  } else {
    console.log(content);
  }
});
```

**Bien:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url)
    .then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

Las promesas soportan algunos métodos de utilidad para hacer tu código más conciso:  

| Patrón                   | Descripción                                |  
| ------------------------ | -----------------------------------------  |  
| `Promise.resolve(value)` | Convertir un valor en una promesa resuelta.   |  
| `Promise.reject(error)`  | Convertir un error en una promesa rechazada.  |  
| `Promise.all(promises)`  | Devuelve una nueva promesa la cual está compuesta por un arreglo de valores por las promesas pasadas o rechazadas con la razón de la primera promesa que rechaza. |
| `Promise.race(promises)`| Devuelve una nueva promesa la cual es llenada/rechazada con el resultado/error de la primera promesa establecida del arreglo de promesas pasadas. |

`Promise.all` es especialmente utilizada cuando se necesita correr tareas en paralelo. `Promise.race` hace que implementar cosas como el tiempo de espera a las promesas sea más fácil.

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Async/Await son aún más limpios que las promesas.

Con la sintaxis de `async`/`await` puedes escribir código que sea mucho más limpio y más legible que las promesas encadenadas. En una función prefijada con la palabra clave `async` tienes la capacidad de decirle a JavaScript que pause la ejecución del código con la palabra clave `await` (cuando es usado en una promesa).

**Mal:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = util.promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

**Bien:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

async function downloadPage(url: string, saveTo: string): Promise<string> {
  const response = await get(url);
  await write(saveTo, response);
  return response;
}

// somewhere in an async function
try {
  const content = await downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html');
  console.log(content);
} catch (error) {
  console.error(error);
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Solución de errores

¡Los errores son buenos! Significan que tu programa identificó satisfactoriamente cuando algo salió mal dentro de él y te lo hace saber al parar la ejecución de la función actual, matando el proceso (en Node) y notificándote en la consola con un rastro para que puedas dirigirte hasta el error.

### Siempre utiliza Error para el lanzamiento o el rechazamiento.

JavaScript, al igual que TypeScript, te permiten lanzar (`throw`) cualquier objeto. Una promesa puede también ser rechazada por cualquier objeto de razón.
Es aconsejable que utilices la sintaxis de `throw` con la del tipo `Error`. Esto es porque el error puede ser avistado en un código de nivel superior con la sintaxis de `catch`.
Sería confuso atrapar un mensaje tipo string y [la depuración sería más dolorosa](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error).  
Por la misma razón deberías rechazar promesas con el tipo `Error`.

**Mal:**

```ts
function calculateTotal(items: Item[]): number {
  throw 'Not implemented.';
}

function get(): Promise<Item[]> {
  return Promise.reject('Not implemented.');
}
```

**Bien:**

```ts
function calculateTotal(items: Item[]): number {
  throw new Error('Not implemented.');
}

function get(): Promise<Item[]> {
  return Promise.reject(new Error('Not implemented.'));
}

// or equivalent to:

async function get(): Promise<Item[]> {
  throw new Error('Not implemented.');
}
```

El beneficio de utilizar el tipo `Error` es que es soportado por la sintaxis `try/catch/finally` e implícitamente todos los errores tienen la propiedad `stack`,
la cual es muy poderosa a la hora de la depuración.
Existen también otras alternativas, no utilizar la sintaxis de `throw` y en vez de eso siempre devolver un error personalizado. TypeScript hace que esto sea aún más fácil.
Considera el siguiente ejemplo:

```ts
type Result<R> = { isError: false, value: R };
type Failure<E> = { isError: true, error: E };
type Failable<R, E> = Result<R> | Failure<E>;

function calculateTotal(items: Item[]): Failable<number, 'empty'> {
  if (items.length === 0) {
    return { isError: true, error: 'empty' };
  }

  // ...
  return { isError: false, value: 42 };
}
```

Para una explicación más detallada de esta idea lee el [post original](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9).

**[⬆ volver al inicio](#tabla-de-contenidos)**

### No ignores errores avistados.

No hacer nada con un error avistado no te da la habilidad ni de solucionarlo ni de reaccionar a tal error. Imprimir el error en la consola (`console.log`) no es tampoco muy bueno ya que a veces puede perderse en el mar de líneas imprimidas en la consola. Si envuelves cualquier bit de código en `try/catch` significa que crees que el error puede ocurrir allí y por lo tanto deberías tener un plan o crear una ruta de código para cuando esto suceda.

**Mal:**

```ts
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}

// or even worse

try {
  functionThatMightThrow();
} catch (error) {
  // ignore error
}
```

**Bien:**

```ts
import { logger } from './logging'

try {
  functionThatMightThrow();
} catch (error) {
  logger.log(error);
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### No ignores promesas rechazadas.

Por la misma razón por la cual no deberías ignorar errores avistados de `try/catch`.

**Mal:**

```ts
getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    console.log(error);
  });
```

**Bien:**

```ts
import { logger } from './logging'

getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    logger.log(error);
  });

// or using the async/await syntax:

try {
  const user = await getUser();
  await sendEmail(user.email, 'Welcome!');
} catch (error) {
  logger.log(error);
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Formato

El formato es subjetivo. Al igual que muchas reglas en esta guía, no hay ninguna regla dura que debas seguir. El punto principal es *NO DISCUTIR* sobre el formato. Hay muchas herramientas para automatizar esto. ¡Usa una de ellas! Es una pérdida de tiempo y dinero para los ingenieros discutir sobre el formato. La regla general es *mantener reglas de formato consistentes*.

Para TypeScript hay una poderosa herramienta llamada [TSLint](https://palantir.github.io/tslint/). Es una herramienta de análisis estático que puede ayudarte a mejorar dramáticamente la legibilidad y el mantenimiento de tu código. Hay configuraciones de TSLint que están listas para usar en tus proyectos:

- [TSLint Config Standard](https://www.npmjs.com/package/tslint-config-standard) - Reglas estándares de estilos.

- [TSLint Config Airbnb](https://www.npmjs.com/package/tslint-config-airbnb) - Guía de estilos de Airbnb.

- [TSLint Clean Code](https://www.npmjs.com/package/tslint-clean-code) - Reglas de TSLint inspiradas de [Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.ca/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882).

- [TSLint react](https://www.npmjs.com/package/tslint-react) - Reglas lint relacionadas a React y JSX.

- [TSLint + Prettier](https://www.npmjs.com/package/tslint-config-prettier) - Reglas de lint para [Prettier](https://github.com/prettier/prettier).

- [ESLint rules for TSLint](https://www.npmjs.com/package/tslint-eslint-rules) - Reglas de ESLint para TypeScript.

- [Immutable](https://www.npmjs.com/package/tslint-immutable) - Reglas para desabilitar la mutación en TypeScript.

Pásate también por esta increíble herramienta [TypeScript StyleGuide and Coding Conventions](https://basarat.gitbook.io/typescript/styleguide).

### Utiliza capitalización consistente.

La capitalización te dice mucho sobre tus variables, funciones, etc. Estas reglas son subjetivas, así que tu equipo puede escoger cualquiera. El punto es, no importa cual elijas, simplemente se *consistente*.

**Mal:**

```ts
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

type animal = { /* ... */ }
type Container = { /* ... */ }
```

**Bien:**

```ts
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

type Animal = { /* ... */ }
type Container = { /* ... */ }
```

Es preferible utilizar `PascalCase` para las clases, para el interfaz, el tipo y los nombres.
Es preferible utilizar `camelCase` para las variables, las funciones y los miembros de las clases.

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Las funciones que llaman y las que son llamadas deben estar cerca unas de la otras.

Si una función llama a otra, mantén a esas funciones verticalmente cerca en el archivo. Idealmente, mantén a la función que llama justo encima de la que es llamada.
Tendemos a leer el código de arriba a abajo, como un periódico. Es por esto que, haz que tu código se lea de esta manera.

**Mal:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**Bien:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Organiza los importes.

Con declaraciones de importes limpias y fáciles de leer puedes rápidamente ver las dependecias del código actual. Asegúrate de aplicar las siguientes prácticas para las declaraciones de importes (`import`):

- Las declaraciones de importes deben ser ordenadas alfabéticamente y agrupadas.
- Las declaraciones de importes que no se utilicen deben ser eliminadas.
- Las declaraciones de importes nombradas deben ser ordenadas alfabéticamente (ej. `import {A, B, C} from 'foo';`).
- Las fuentes de importes deben ser ordenadas alfabéticamente entre los grupos, ej.: `import * as foo from 'a'; import * as bar from 'b';`.
- Los grupos de importes deben ser delineados por líneas en blanco.
- Los grupos deben seguir el siguiente orden:
  - Polifunciones (ej. `import 'reflect-metadata';`).
  - Node builtin modules (i.e. `import fs from 'fs';`).
  - Módulos externos (ej. `import { query } from 'itiriri';`).
  - Módulos internos (ej `import { UserService } from 'src/services/userService';`).
  - Módulos de un directorio superior (ej. `import foo from '../foo'; import qux from '../../foo/qux';`).
  - Módulos del mismo directorio o de un directorio primo (ej. `import bar from './bar'; import baz from './bar/baz';.

**Mal:**

```ts
import { TypeDefinition } from '../types/typeDefinition';
import { AttributeTypes } from '../model/attribute';
import { ApiCredentials, Adapters } from './common/api/authorization';
import fs from 'fs';
import { ConfigPlugin } from './plugins/config/configPlugin';
import { BindingScopeEnum, Container } from 'inversify';
import 'reflect-metadata';
```

**Bien:**

```ts
import 'reflect-metadata';

import fs from 'fs';
import { BindingScopeEnum, Container } from 'inversify';

import { AttributeTypes } from '../model/attribute';
import { TypeDefinition } from '../types/typeDefinition';

import { ApiCredentials, Adapters } from './common/api/authorization';
import { ConfigPlugin } from './plugins/config/configPlugin';
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Usa las alias de TypeScript.

Crea importes más bonitos definiendo las direcciones y las propiedades baseUrl en la sección compilerOptions en el archivo `tsconfig.json`.

Esto evitará largas direcciones relativas al hacer importes.

**Mal:**

```ts
import { UserService } from '../../../services/UserService';
```

**Bien:**

```ts
import { UserService } from '@services/UserService';
```

```js
// tsconfig.json
...
  "compilerOptions": {
    ...
    "baseUrl": "src",
    "paths": {
      "@services": ["services/*"]
    }
    ...
  }
...
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Comentarios.

El uso de comentarios indica que hay una falta de expresión sin ellos. El código en sí debería ser la única fuente verdadera.
  
> Don’t comment bad code—rewrite it.  
> — *Brian W. Kernighan and P. J. Plaugher*

### Es preferible el código que se entiende por sí mismo a tener que utilizar comentarios.

Los comentarios son una analogía, no un requerimiento. Un buen código *casi siempre* se entiende por sí mismo.

**Mal:**

```ts
// Check if subscription is active.
if (subscription.endDate > Date.now) {  }
```

**Bien:**

```ts
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) { /* ... */ }
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### No dejes código comentado en tu archivo base.

El control de versiones existe por una razón. Deja el código viejo en tu historia.

**Mal:**

```ts
type User = {
  name: string;
  email: string;
  // age: number;
  // jobPosition: string;
}
```

**Bien:**

```ts
type User = {
  name: string;
  email: string;
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### No tengas comentarios de diario.

Recuerda, ¡usa el control de versiones! No hay necesidad de tener código sin usar, código comentado y especialmente comentarios de diario. ¡Usa `git log` para ver la historia!

**Mal:**

```ts
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Added type-checking (LI)
 * 2015-03-14: Implemented combine (JR)
 */
function combine(a: number, b: number): number {
  return a + b;
}
```

**Bien:**

```ts
function combine(a: number, b: number): number {
  return a + b;
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Evita marcas posicionales.

Ellas usualmente solo añaden ruido. Deja que las funciones y los nombres de las variales, junto con la indentación y el formato den a tu código una estructura visual.
La mayoría de los IDEs soportan el código dentro de carpetas, lo que te permite colapsar y expandir bloques de código (ve Visual Studio Code ["folding regions (regiones de carpetas)"](https://code.visualstudio.com/updates/v1_17#_folding-regions)).

**Mal:**

```ts
////////////////////////////////////////////////////////////////////////////////
// Client class
////////////////////////////////////////////////////////////////////////////////
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  ////////////////////////////////////////////////////////////////////////////////
  // public methods
  ////////////////////////////////////////////////////////////////////////////////
  public describe(): string {
    // ...
  }

  ////////////////////////////////////////////////////////////////////////////////
  // private methods
  ////////////////////////////////////////////////////////////////////////////////
  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**Bien:**

```ts
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  public describe(): string {
    // ...
  }

  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

### Comentarios TODO

Cuando necesitas dejar notas en el código para futuras mejoras, utiliza
los comentarios `// TODO`. La mayoría de los IDEs tienen soporte de este tipo de comentarios para que
puedas navegar rápidamente por la lista completa de comentarios "TODO". 

Recuerda igualmente que un comentario *TODO* no es una excusa de un mal código.

**Mal:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**Bien:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**[⬆ volver al inicio](#tabla-de-contenidos)**

## Traducciones

Esta guía está también disponible en los siguientes idiomas:
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portugués de Brasil**: [vitorfreitas/clean-code-typescript](https://github.com/vitorfreitas/clean-code-typescript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chino**: 
  - [beginor/clean-code-typescript](https://github.com/beginor/clean-code-typescript)
  - [pipiliang/clean-code-typescript](https://github.com/pipiliang/clean-code-typescript)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japonés**: [MSakamaki/clean-code-typescript](https://github.com/MSakamaki/clean-code-typescript)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Ruso**: [Real001/clean-code-typescript](https://github.com/Real001/clean-code-typescript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turco**: [ozanhonamlioglu/clean-code-typescript](https://github.com/ozanhonamlioglu/clean-code-typescript)
- ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Coreano**: [738/clean-code-typescript](https://github.com/738/clean-code-typescript)

Las referencias serán añadidas cuando las traducciones sean completadas.
Pásate por esta [discusión](https://github.com/labs42io/clean-code-typescript/issues/15) para más detalles sobre el progreso.
Puedes hacer una indispensable contribución a la comunidad de *Clean Code* traduciendo esta guía a tu idioma.
