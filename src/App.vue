<script>
// tenemos que exportar por defecto de esta forma
export default {
	//en vue 3 inicializamos nuestro js con la funcion setup
	//donde creamos las constantes, funciones, etc
	setup() {
		const name = 'The Snazzy Burger';

		const placeOrder = () => {
			alert(`Order placed at ${name}!`);
		};

		//para usarlo en nuestro código lo tenemos que retornar
		return { placeOrder, name };
	},
};
</script>

<template>
	<h1>{{ name }}</h1>
	<button @click="placeOrder">Place Order</button>
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

