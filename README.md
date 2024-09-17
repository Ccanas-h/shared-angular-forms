# ¿Qué es ControlContainer?

ControlContainer es una clase de Angular Forms que actúa como un contenedor para formularios. Es una forma de hacer referencia a un FormGroup o FormArray que está "conteniendo" el formulario en un nivel más alto en la jerarquía de componentes.

##¿Por qué se usa ControlContainer?
Cuando trabajamos con formularios anidados en Angular, como tener componentes separados que gestionan partes de un formulario más grande, necesitamos una manera de acceder al formulario padre (FormGroup) desde un componente hijo. Es decir, el componente hijo (por ejemplo, AddressGroupComponent) necesita saber en qué formulario más grande está incluido para agregar o quitar controles.

Aquí es donde entra en juego ControlContainer. Nos permite acceder al FormGroup que está más arriba en la jerarquía de componentes, y de esta manera, podemos "anidar" formularios sin problemas.

## ¿Cómo funciona la inyección de ControlContainer?

``````
viewProviders: [
  {
    provide: ControlContainer,
    useFactory: () => inject(ControlContainer, {skipSelf: true})
  }
]
``````
### Inyección de ControlContainer: Este código dice que queremos inyectar el ControlContainer desde un nivel superior (padre) en el árbol de componentes.

La opción skipSelf: true asegura que Angular busque el ControlContainer desde el padre del componente actual, ignorando el propio componente si lo tiene. Esto es importante porque estamos asumiendo que el FormGroup que nos interesa está definido en un nivel superior, no dentro del componente hijo.
Vinculación con el Formulario Padre: Con ControlContainer inyectado, en el componente podemos acceder al FormGroup padre usando la referencia a parentContainer:


``````
parentContainer = inject(ControlContainer);
Acceder al FormGroup: Una vez que tenemos el ControlContainer padre, podemos obtener el FormGroup donde este componente hijo está siendo usado. Lo hacemos a través de:
``````

``````
get parentFormGroup() {
  return this.parentContainer.control as FormGroup;
}
``````
Esta función obtiene el FormGroup del padre, permitiendo que el componente hijo (por ejemplo, AddressGroupComponent) interactúe con el formulario más grande, ya sea agregando nuevos controles o eliminándolos.

Resumen:
Inyectar el ControlContainer nos permite "conectar" un componente hijo con el FormGroup que está más arriba en la jerarquía de componentes. Esto facilita la comunicación entre formularios anidados, permitiendo que el componente hijo agregue o elimine sus propios campos al formulario padre sin necesidad de pasar referencias de FormGroup de manera explícita a través de @Input. Es una forma elegante de hacer que los formularios sean más modulares y reutilizables.
