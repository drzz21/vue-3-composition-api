<script>
import YummyMeal from './components/YummyMeal.vue';
//para declarar variables reactivas importamos la api ref
//para usar watch importamos la api watch
import { ref, reactive, watch } from 'vue';

// tenemos que exportar por defecto de esta forma
export default {
	// declaramos nuestro componente para poder utilizarlo
	components: {
		YummyMeal,
	},
	//en vue 3 inicializamos nuestro js con la funcion setup
	//donde creamos las constantes, funciones, etc
	setup() {
		//declaramos nuestra variable que queremos reactiva
		//envolviendola en la api ref
		const name = ref('The Snazzy Burger');

		//otra forma de manejar elementos reactivos
		//es usando reactive, este tipo de variable
		//igual que ref es reactiva, pero en este caso
		//se usa para objetos complejos con varias propiedades
		//no para objetos primitivos, osea solo
		//funciona con arrays y objetos
		//tiene la ventaja de que no necesitamos usar
		//.value para acceder a sus propiedades
		//en este caso podemos acceder directamente a las propiedades del objeto
		//sin necesidad de usar .value
		//pero tiene la desventaja de que no podemos
		//desestructurarlo directamente, ya que perderiamos
		//la reactividad y tampoco podemos reasignar el objeto completo
		//porque tambien perderiamos la reactividad

		const meal = reactive({
			name: 'Pizza',
			price: 12.99,
		});

		//para acceder a su valor dentro de setup usamos
		//el atributo .value de nuestra referencia reactiva
		console.log(name.value);

		//igual para cambiar su valor
		name.value = 'Changed Burger Name';

		const placeOrder = () => {
			alert(`Order placed at ${name}!`);
		};

		//creamos la funcion para imprimir el item agregado
		//vamos a crear un carrito para almacenar los items
		//agregados
		const cart = reactive([]);

		//como ahora los items se guardan en un arreglo
		//que es reactive, los almacenamos usando push
		const addItemToCart = (itemName) => {
			// alert(`Added ${itemName} to cart!`);
			cart.push(itemName);
		};

		//para eliminar el watcher, asignamos la funcion
		//de retorno de watch a una constante
		//y luego la llamamos cuando queramos eliminar el watcher
		//en este caso la llamamos desde un boton en el template
		const removeWatcher = watch(
			//podemos observar arreglos y objetos
			//para lo cual debemos usar la siguiente sintaxis
			//para observar arreglos, debemos retornar una copia del arreglo
			//usando el operador spread o slice
			//esto para que los valores newValue y oldValue
			//se asignen correctamente
			// () => [...cart],
			// () => cart,
			//podemos observar 2 variables a la vez si definimos
			//un array con las variables a observar
			[name, () => [...cart]],

			//despues de las variables o variable a observar tenemos
			//la funcion que se ejecutará cuando cambie el valor
			//de la variable observada
			//en este caso recibimos el nuevo y antiguo valor
			(newValue, oldValue) => {
				//en este caso como ejemplo solo imprimiremos
				//el nuevo valor del carrito en un alert
				alert(newValue.join('\n'));
				//y en consola
				console.log('Cart changed from', oldValue, 'to', newValue);
			},
			{
				//immediate es para que se ejecute tan pronto se renderice el componente
				//en caso de que sea false, se va a esperar a que cambie el valor
				immediate: false,
				//deep es para observar cambios profundos en objetos y arreglos
				//en este caso como cart es un arreglo, es recomendable activarlo
				//si fuera un string no tendria sentido
				deep: true,
			}
		);


		//otro ejemplo donde observamos un array reactive
		const meals = reactive([
			{ name: 'Pasta', price: 9.99 },
			{ name: 'Salad', price: 7.99 },
			{ name: 'Sushi', price: 14.99 },
		]);

		// watch(name, (newValue, oldValue) => {
		// 	console.log(`Name changed from ${oldValue} to ${newValue}`);
		// },{
		// 	immediate: true,
		// 	deep: false,
		// });

		//para usarlo en nuestro código lo tenemos que retornar
		return { placeOrder, name, addItemToCart, meal, meals, removeWatcher };
	},
	//para acceder a las variables reactivas fuera de setup
	//no es necesario usar .value
	//ejemplo en el ciclo de vida created
	//usamos this, para que tenga el contexto del componente
	created() {
		console.log('name', this.name);
	},
	// de esta forma se usaba watch con options api
	// watch: {
	// 	//ejemplo de watch en options api
	// 	name(newValue, oldValue) {
	// 		console.log(`Name changed from ${oldValue} to ${newValue}`);
	// 	},
	// },
};
</script>

<template>
	<!-- en nuestro template tampoco hace falta usar .value
	ya que vue se encarga de eso automaticamente -->
	<h1>{{ name }}</h1>

	<!-- al cambiar el valor vemos que se actualiza automaticamente
	cumpliendo con esto la reactividad -->
	<input type="text" v-model="name" />

	<button @click="placeOrder">Place Order</button>

	<!-- ejecutamos removewatcher al hacer click para dejar de usar el watcher -->
	<button @click="removeWatcher">Remove Watcher / Hide Cart Alerts</button>

	<!-- llamamos la funcion dentro de nuestro evento emitido, y ahi recibimos
	 el nombre del producto que enviamos desde la funcion dentro del componente
	 aqui tambien enviamos las props al componente hijo -->
	<YummyMeal name="Burger" :price="8.99" @add-to-cart="addItemToCart" />
	<!-- enviamos las propiedades de nuestro reactive -->
	<YummyMeal
		:name="meal.name"
		:price="meal.price"
		@add-to-cart="addItemToCart"
	/>
	_____
	<!-- ejemplo con array reactive -->
	 iteramos para tener todas las meals
	<YummyMeal
		v-for="(item, index) in meals"
		:key="index"
		:name="item.name"
		:price="item.price"
		@add-to-cart="addItemToCart"
	/>
</template>

<style scoped>
.logo {
	height: 6em;
	padding: 1.5em;
	will-change: filter;
	transition: filter 300ms;
}
.logo:hover {
	filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
	filter: drop-shadow(0 0 2em #42b883aa);
}
</style>

