# Empezando con REACT:

## React:

### *NO es un MVC Framework. React es una librería de Javascript creada por los desarrolladores de Facebook para construir interfaces de usuario de forma modular.*

**Entidades fuertes usando Reactjs:**

Facebook (donde se creó), Netflix, PayPal, AirBnB, Coursera, Asana, Atlassian, Dropbox, Expedia, Flipboard, HipChat, Instagram, Khan Academy, Periscope, Reddit, Salesforce, Twitter - Fabric, Uber, Venmo, Whatsapp, Yahoo, Zendesk y algunos mas.

**SINTAXIS JSX:**

Es una extensión de la sintaxis XML.
Está destinado a ser utilizado por varios preprocesadores(transpiler) para transformarlo en un estándar ECMAScript.

No necesariamente tiene que usar JSX con React,si usted desea puede usar solo JavaScript, claro está que se recomienda utilizar JSX por que es más legible a la hora de desarrollar.

Para transpilar JSX puedes usar babel.

*Sin JSX:*
```javascript
React.render(React.createElement('h1', null, 'Hello, world!'), document.getElementById('example'));
```
*Con JSX:*
```javascript
React.render(<h1>Hello, world!</h1>, document.getElementById('example'));
```
### Especificaciones de Componentes.

Los principales son:

**render()**: La función render debe retornar un solo objeto en representacion de la interfaz. Tambien se puede retornar `null` o `undefined` para no mostrar ninguna interfaz.

**constructor()**: Se ejecuta una sola vez al inicio de montarse el componente en la pantalla y debe retornar el estado inicial del componente mendiante un objeto javascript.

**propTypes**: es un objeto que indica los tipos de las propiedades que recibe el componente en su instanciacion.

### CICLO DE VIDA DE LOS COMPONENTES

Se definen 3 etapas de un componente.

**MOUNTING:**
Cuando el componente se está montando en el Dom.

**UPDATING:**
Cuando se están modificando los `props` y `states`.

**UNMOUNTING:**
Cuando el componente se está quitando del Dom.

Existen varios métodos que provee React que ayudarán a la hora de crear nuestros componentes para distintos escenarios.

**MOUNTING: COMPONENTWILLMOUNT**
Se invoca una vez tanto en el cliente y el servidor.
Se ejecuta antes de que el componente sea montado en el DOM.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MyComponent extends React.Component {
  constructor(props){
    super(props);
  }
  componentWillMount(){
    console.log('Before mount...');  
  }
  render(){
    return <div>My Component</div>
  }
}

ReactDOM.render(<MyComponent />, document.getElementById('container'));
```

**MOUNTING: COMPONENTDIDMOUNT**
Se ejecuta inmediatamente después que sea montado en el DOM.
Si deseas hacer peticiones AJAX o integrar con librerías de terceros como jquery u otros, este método es el ideal.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MyComponent extends React.Component {
  constructor(props){
    super(props);
    console.log('Before mount');
  }

  componentDidMount(){
    console.log('Mount');
  }
  
  render(){
    return <div>MyComponent</div>
  }
}

ReactDOM.render(
  <MyComponent />, 
  document.getElementById('container')
);
```

**UPDATING: COMPONENTWILLRECEIVEPROPS**
Este método es invocado cuando un componente está recibiendo nuevas props.
Este método no es invocado en el primer render.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MyComponent extends React.Component {
  componentWillReceiveProps(nextProps) {
    console.log('nextProps ',nextProps.name);
    console.log('props ',this.props.name);
  }
  render() {
    return <div>Bar {this.props.name}!</div>;
  }
}

ReactDOM.render(<MyComponent name="jupiter" />, document.getElementById('container'));

ReactDOM.render(<MyComponent name="saturno" />, document.getElementById('container'));

ReactDOM.render(<MyComponent name="ganimedes" />, document.getElementById('container'));
```

**UPDATING: COMPONENTWILLUPDATE**
Se invocará antes del render, justo antes que tu componente se haya actualizado (recibiendo nuevas props o state).
Excelente para procesos que necesiten hacer antes de hacer la actualización.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MyComponent extends React.Component{
  componentWillUpdate(){
    console.log('Update MyComponent');
  }
  render(){
    return <div>my Component {this.props.name}</div>
  }
}

ReactDOM.render(<MyComponent name="jupiter"/>,document.getElementById('container'));
ReactDOM.render(<MyComponent name="neptuno"/>,document.getElementById('container'));
```

**UPDATING: COMPONENTDIDUPDATE:**
Se invoca inmediatamente después del render, justo cuando tu componente a cambiado.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MyComponent extends React.Component{
  componentDidUpdate(prevProps,prevState){
    console.log('prevPros or prevState');
  }
  render(){
    return <div>my Component {this.props.name}</div>
  }
}

ReactDOM.render(<MyComponent name="jupiter"/>,document.getElementById('container'));
ReactDOM.render(<MyComponent name="neptuno"/>,document.getElementById('container'));
```

**UNMOUNTING: COMPONENTWILLUNMOUNT**
Se invoca inmediatamente antes de que un componente sea desmontado del DOM.
Aquí puedes realizar la limpieza de cualquier referencia de memoria que nos hayan quedado.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class MyComponent extends React.Component {
  constructor(props){
    super(props);
  }

  componentWillMount(){
    console.log('Mount');
  }
  componentWillUnmount(){
    console.log('Unmount');
  }
  render(){
    console.log('Rendering');
    return <div>I am component</div>
  }
}

class MyApp extends React.Component{
  constructor(pros){
    super(pros);
  }

  mount(){
    ReactDOM.render(<MyComponent />,document.getElementById('component'));
  }

  unmount(){
    ReactDOM.unmountComponentAtNode(document.getElementById('component'));
  }

  render(){
    return( 
      <div>
        <button onClick={this.mount.bind(this)}>Mount component</button>
        <button onClick={this.unmount.bind(this)}>Unmount component</button>
        <div id="component"></div>
      </div>
    )
  }
}

ReactDOM.render(<MyApp />, document.getElementById('container'));
```

* ***El método constructor se ejecuta antes componentWillMount.***

### Funciones de Operación

Estas funciones permiten cambiar el estado del componente o forzar que se refresque.

**setState()**: Permite cambiar el estado actual del componente, forzando con el mismo que se intente realizar un repintado del mismo de acuerdo al estado que se cambie. Adicionalmente se le puede especificar una función de callback para una vez termine el cambio de estado poder realizar operaciones.

**forceUpdate (No debe ser usada)**: Fuerza el repintado del componente manteniendo el estado anterior.

**Otras**: `replaceState`, `getDOMNode`, `isMounted`, `setProps`, `replaceProps`

Las funciones y metodos que he descrito son las que más he usado, normalmente son las esenciales para cualquier componente.

Un componente luce más o menos así:

```javascript
class Button extends React.Component {
  constructor () {
    super();
    this.state = { amount: 0 };
  }

  render () {
    return (
      <button type="button" onClick={this.adding}>
        Pressed {this.state.amount} times
      </button>);
  }
  
  adding = (evt) => {
    this.setState({amount: this.state.amount + 1});
    evt.preventDefault();
  }
};

React.render(<Button/>, document.getElementById('container'));
```

## Ejemplos de Código

Este ejemplo es usado para mostrar un simple componente que es un botón.
Acá se verán las maneras para esconder y mostrar un elemento. Y como cambiar el `state` de nuestro componente.

```javascript
/* jshint esnext: true */

class Button extends React.Component {

  constructor() {
    super();
    this.state = {amount: 0}; // Se setea el estado inicial del componente
  }

  render () {

    let showEl;
    let showEl2;
    let showEl3;

    // Los elementos se mostrarán si el state.amount es mayor a 5
    showEl = (this.state.amount > 5) ? <p>Amount 1: {this.state.amount}</p> : null;
    // showEl2 no tiene ningún condicional
    // Su condicional para que aparezca está dentro del return
    showEl2 = <p>Amount 2: {this.state.amount + 5}</p>

    if (this.state.amount > 5) {
      showEl3 = <button type="button" className="btn btn-default" onClick={this.addition}>
        New Button
      </button>
    }

    return (
        <div className="jumbotron container">

          {this.state.amount < 6
            <button type="button" className="btn btn-default" onClick={this.addition}>
              Pressed {this.state.amount} times
            </button>
          }
          // Acá se llaman los elementos que estaban escondidos
          // Aparecerán hasta que el state.amount cumpla con el condicional
          {showEl}
          {(this.state.amount > 5) && showEl2}
          {showEl3}
          
       </div>
    );
  }

  addition = (evt) => {
    this.setState({amount: this.state.amount + 1});
    evt.preventDefault();
  }
};

React.render(<Button/>, document.getElementById('container'));
```

## App `Todo List` completa para ver como se mueven los `props` y `state`s de un componente a otro

```javascript
/* jshint esnext: true */

class TodoList extends React.Component {

  static propTypes = {
    todos: React.PropTypes.array
  }

  constructor(props) {
    super(props)
    this.state = { todos: this.props.todos || [] }
  }
  
  addTodo = (item) => {
    this.setState({todos: this.state.todos.concat([item])});
  }

  render () {
    return (
      <div>
        <h3>TODO List</h3>
        <TodoItems items={this.state.todos}/>
        <TodoInput addTodo={this.addTodo}/>
      </div>
    );    
  }
};

class TodoItems extends React.Component {

  static propTypes = {
    items: React.PropTypes.array.isRequired
  }
  
  constructor(props) {
    super(props);
  }
  
  render () {
	let createItem;
      
    createItem = (item, index) => {
      return (
        <li key={index}>{item}</li>
      );
    };
	return <ul>{this.props.items.map(createItem)}</ul>;
  }
};

class TodoInput extends React.Component {
  
  constructor (props) {
     super(props);
     this.state = {item: ''};
  }
  
  onChange = (e) => {
    this.setState({item: e.target.value});
  }
  
  handleSubmit = (e) => {
    e.preventDefault();
    this.props.addTodo(this.state.item);
    this.setState({item: ''});
  }
  
  render () {
    return (
      <form onSubmit={this.handleSubmit}>
        <input type="text" onChange={this.onChange} value={this.state.item}/>
        <input type="submit" value="Add"/>
      </form>
    );
  }
};

   
React.render(<TodoList todos={['red','blue']}/>, document.getElementById('container'));
```

La lista de `todos` es pasada hacia abajo desde el `state` de un componente como propiedad a un componente subyacente (TodoItems). El componente `TodoList` es el responsable de mantener la lista de `Todos`, esa es la razón por la cual se mantiene en el estado de este componente. Cada cambio en el estado del componente va a provocar que el método `render` se llame.

El componente `TodoItem` suple los nuevos items al componente `TodoList`, esto es gracias al callback `addTodo` que se llama desde `TodoInput` cada vez que el usuario agrega nueva información. 

Cómo explicamos que está haciendo la función `onChange` en el componente `TodoItems`:

`onChange` está llamando `this.setState()`, función de React que setea el nuevo valor de `this.state` lo cual significa que el método `render` va a ser llamado nuevamente. 

Hay que hacerse de cuenta que los props trabajan a modo de parametros/variables que son pasadas de un componente a otro. 

También se puede Javascript adentro de los componentes cuando son llamados.

Por ejemplo:

`<TodoItems items={this.state.todos.filter((x) => x.whatever === true)}/>`


