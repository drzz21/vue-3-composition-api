<script>
import YummyMeal from './components/YummyMeal.vue';
//para declarar variables reactivas importamos la api ref
import { ref, reactive } from 'vue';

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

		const addItemToCart = (itemName) => {
			alert(`Added ${itemName} to cart!`);
		};

		//para usarlo en nuestro código lo tenemos que retornar
		return { placeOrder, name, addItemToCart,meal };
	},
	//para acceder a las variables reactivas fuera de setup
	//no es necesario usar .value
	//ejemplo en el ciclo de vida created
	//usamos this, para que tenga el contexto del componente
	created(){
		console.log('name', this.name);
	}
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
	<!-- llamamos la funcion dentro de nuestro evento emitido, y ahi recibimos
	 el nombre del producto que enviamos desde la funcion dentro del componente
	 aqui tambien enviamos las props al componente hijo -->
	<YummyMeal name="Burger" :price="8.99" @add-to-cart="addItemToCart" />
	<!-- enviamos las propiedades de nuestro reactive -->
	<YummyMeal :name="meal.name" :price="meal.price" @add-to-cart="addItemToCart" />
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

