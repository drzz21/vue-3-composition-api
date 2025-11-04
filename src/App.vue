<script>
import YummyMeal from './components/YummyMeal.vue';

// tenemos que exportar por defecto de esta forma
export default {
	// declaramos nuestro componente para poder utilizarlo
	components: {
		YummyMeal,
	},
	//en vue 3 inicializamos nuestro js con la funcion setup
	//donde creamos las constantes, funciones, etc
	setup() {
		const name = 'The Snazzy Burger';

		const placeOrder = () => {
			alert(`Order placed at ${name}!`);
		};

		//creamos la funcion para imprimir el item agregado

		const addItemToCart = (itemName) => {
			alert(`Added ${itemName} to cart!`);
		};

		//para usarlo en nuestro código lo tenemos que retornar
		return { placeOrder, name, addItemToCart };
	},
};
</script>

<template>
	<h1>{{ name }}</h1>
	<button @click="placeOrder">Place Order</button>
	<!-- llamamos la funcion dentro de nuestro evento emitido, y ahi recibimos
	 el nombre del producto que enviamos desde la funcion dentro del componente
	 aqui tambien enviamos las props al componente hijo -->
	<YummyMeal name="Burger" :price="8.99" @add-to-cart="addItemToCart" />
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

