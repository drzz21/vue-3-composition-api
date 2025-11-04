<template>
	<p>
		<strong>{{ name }}</strong> - ${{ price }}

		<button @click="addToCart">Add to Cart</button>

	</p>
</template>

<script>
export default {
	//de esta forma declaramos las props que recibiremos y el tipo de dato que serán
	props: {
		name: String,
		price: Number,
	},
	//recibimos las props como argumento de nuestro setup
	// y el segundo argumento tiene opciones como attrs, slots y emit
	//podemos desestructurar el segundo argumento para obtener solo lo que necesitamos
	//pero no el primero ya que perderiamos la reactividad
	setup(props, { attrs, slots, emit }) {
		//imprimimos el valor de las props
		console.log(props.name);
		console.log(props.price);

		//emiitmos un evento al hacer click en el boton
		const addToCart = () => {
			emit('add-to-cart', props.name);
		};
		//retornamos para poder usar en el template
		return { addToCart };
	},
};
</script>
