---
title: "¿Cómo manejar los datos de tu aplicación react?"
subtitle: "Sin Flux, React es solo una linda librería de front-end. React Flux lo convertirá en un marco de trabajo, que le dará a tu aplicación una estructura definida, ocupándose de la capa de procesamiento de datos y más cosas de flux."
cover_local: "../../assets/images/b84e07e5-5761-469b-85bb-f7afc87d4dc9.png"
textColor: "white"
date: "2020-10-19T16:36:31+00:00"
tags: ["reactjs","javascript"]
status: "published"

---

En el eterno de debate de como manejar un estado centralizado han habido muchos planteamientos y en la misma medida que las necesidades de las aplicaciones evolucionan, también lo hacen las estrategias para resolver este problema.

Una solución recomendada y que escala muy bien es el uso combinado de los hooks `useContext` y `useReducer`. Con el contexto puedes exponer un estado y funciones para todos los componentes que se encuentren dentro, esto va a prevenir que caigas en el abismo del 'prop drilling' y solo hacer llamado al contexto desde el componente que lo necesita. [Aquí puedes ver](https://4geeks.com/es/lesson/context-api-es) mas en detalle como funciona este hook.

La otra pieza del rompecabezas es el reducer, este permite encapsular lógica y estado, de manera que se puede reutilizar y compartir entre varios componentes. El reducer permite administrar estados mas complejos y también hace posible escribir pruebas unitarias para las acciones.

## Vamos a implementar una lista de tareas

### 1) Crearemos un reducer para manejar el estado y las acciones

Para poder tomar el control del flujo de los datos en nuestra aplicación utilizaremos un `reducer` para agrupar las funciones y la lógica de la aplicación (actions) junto con los datos que manejan y que tienen que estar disponible para los componentes (state).

Por ahora solo diremos que el reducer es una función que genera un estado nuevo cada vez que se ejecuta y lo que haga dependerá de la información que reciba en la función `action`. Esto nos permitirá llamar a las `actions` para actualizar el estado como lo indica el patrón flux. Para entender en detalle como funciona un reducer, puedes [leer esté artículo](https://4geeks.com/es/lesson/que-es-usereducer-react) donde lo explicamos a profundidad. 

```javascript
// Esta es la función reducer
const TaskReducer = (state, action) => {
  // Dependiendo del type de la acción realiza una tarea distinta
 switch (action.type) {
    case "add":
      return [...state, action.payload];
    case "remove":
      let newState=[...state]
      newState.splice(action.index, 1);
      return newState
    default:
      return state;
  }
};
```

El siguiente paso es hacer que esta función esté disponible para todos los componentes de mi aplicación, para eso utilizaremos un contexto con el hook `useReducer`, el cual nos va a permitir crear el estado y la función `actions` para ponerla a disposición del resto de la aplicación.

```jsx
//TaskContext.jsx
import { useReducer, createContext } from "react";

// Creamos el contexto vacío
const TaskContext = createContext(null);

const TaskReducer = (state, action) => {
 // Aquí va el reducer que se definió anteriormente👆
};

// Crearemos un componente que va a envolver nuestra aplicación en el contexto
export function TaskProvider({ children }) {
  // Creamos el state 'tasks' y el despachador 'taskActions'
  // adicionalmente pasamos como estado inicial un arreglo vacío
  const [tasks, taskActions ]= useReducer(TaskReducer, []);
  return (
    {/* Creamos el contexto con nuestro state y actions */}
    <TaskContext.Provider value={{tasks, taskActions}}>{children}</TaskContext.Provider>
  );
}

// Es necesario exportar el contexto para usarlo en otros componentes
export default TaskContext;
```

Ya con esto tenemos listo nuestro contexto con las tasks, ahora solo falta envolver nuestra aplicación en este componente para empezar a utilizarlo.

```jsx
//index.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { TaskProvider } from "./TaskContext.jsx";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <TaskProvider>
      <App />
    </TaskProvider>
  </React.StrictMode>,
);

```
Ahora solo queda llamar al contexto desde los componentes que necesiten hacer uso de nuestras tareas.

### 2) Empecemos por agregar una nueva tarea

Para ello usaremos un componente que muestre una caja de texto y un botón que realiza la acción de agregar la tarea, todo dentro de un formulario para facilitar el manejo del evento de envío(submit).

Todo esto es básico de un formulario en react, pero como queremos utilizar las `actions` del contexto, necesitamos llamarlo en el componente.

```jsx
import { tasks, useContext } from "react";
import TaskContext from "./TaskContext.jsx";

export default function AddItem() {
  const { taskActions } = useContext(TaskContext);
  // A partir de este punto tenemos disponibles las actions del reducer
  // ...
}
```

Para hacer uso de estas `actions` se llama a la función y se le pasa como parámetro un objeto con la propiedades de la acción que queremos realizar, siendo la mas importante `type` que indica la acción especifica a ejecutar. El resto de las propiedades son datos opcionales que pueden ser requeridos por la acción.

```jsx
// AddItem.jsx
import { useContext } from "react";
import TaskContext from "./TaskContext.jsx";

export default function AddItem() {
  // Hacemos uso del contexto y accedemos a la función 'taskActions'
  const { taskActions } = useContext(TaskContext);
  function handleAddTask(e) {
    e.preventDefault();
    // Llamamos al actions especificándole 'type'
    // asi como también la tarea que se va a agregar
    let textbox = e.target.elements.task;
    taskActions({ type: "add", payload: textbox.value });
    textbox.value = "";
  }
  return (
    <li>
      <form onSubmit={handleAddTask}>
        <input name="task" type="text"/>
        <button type="submit">+</button>
      </form>
    </li>
  );
}
```

### 3) Ahora vamos a mostrar la lista

De la misma forma como accedemos a `taskActions`, también podemos acceder al objeto `tasks` que contiene el estado con la lista. Igual que antes, debemos hacer uso de `useContext` en nuestro componente.

```jsx
import { useContext } from "react";
import "./App.css";
import TaskContext from "./TaskContext.jsx";
import ListItem from "./ListItem.jsx";
import AddItem from "./AddItem.jsx";

export default function App() {
  // Accedemos al contexto, pero esta vez solo vamos a usar 'tasks'
  const {tasks} = useContext(TaskContext);

  return (
    <main>
      <h2>Todo list</h2>
      <ul className="list-group w-50">
        <AddItem />
        {tasks.map((task, index) => (
          <ListItem key={index} task={task} index={index} />
        ))}
      </ul>
    </main>
  );
}
```

Puedes notar que aparece el componente `AddItem` que vimos previamente y desde donde se pueden agregar tarea. Luego de eso se hace el renderizado de la lista con la función `map`, pero notamos que se esta usando un componente `ListItem` para mostrar los elementos, no solo eso sino que ahi también corresponde hacer la eliminación de la tarea, veamos ese componente.

### 4) Eliminación de items

Si bien el renderizado es básico (un elemento `li` con el texto y un botón), lo interesante es como hacemos la eliminación del item con las actions.

***Todo comienza cuando el usuario haga clic en el icono de la papelera. Es por eso que necesitamos iniciar nuestra aplicación escuchando el típico evento onClick en el botón de eliminar.***

```jsx
  onClick={() => taskActions({ type: "remove", index })}
```

Notamos que el llamado al action es parecido al que usamos para agregar items, pero se le esta pasando un parámetro distinto llamado `index`, que le indica al dispatcher que elemento va a eliminar. Asi como vimos en ambos ejemplos, podemos pasar la data que necesite nuestra action al momento de llamarla como parámetros adicionales.

```jsx
import { useContext } from "react";
import TaskContext from "./TaskContext.jsx";

export default function ListItem({ task, index }) {
  const { taskActions } = useContext(TaskContext);

  return (
    <li>
      {task}
      <button
        onClick={() => taskActions({ type: "remove", index })}
      >
        {/* Icono de papelera */}
        <i className="bi bi-trash3"></i>
      </button>
    </li>
  );
}
````

## Resultado final

Ya hemos implementado la lógica de nuestra aplicación en un contexto aplicando el patrón flux, permitiendo su uso en distintos componentes. A continuación podemos ver el resultado final.

<iframe src="<iframe src="https://playcode.io/1764043"></iframe>
