<template>
	<p>
		<strong>{{ name }}</strong> - ${{ formattedPrice }}

		<button @click="addToCart">Add to Cart</button>
	</p>
</template>

<script>
// para usar computed properties con composition API
// importamos computed desde vue
import { computed } from 'vue';

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
		//y creamos la funcion computed
		//que retornará el valor formateado de la prop price
		//en este caso con 2 decimales
		//forma simple sin setter
		//donde podemos usar una arrow function

		// const formattedPrice = computed(() => {
		// 	return `${props.price.toFixed(2)}`;
		// });

		//si queremos usar el setter para asignar
		//un valor a la propiedad computada
		//debemos usar la sintaxis de objeto
		//y no usar arrow functions en get/set
		const formattedPrice = computed({
			//retornamos el valor formateado
			//como anteriormente
			get() {
				return `${props.price.toFixed(2)}`;
			},
			//en este caso mostramos mensaje de error
			//al intentar asignar un valor
			set(newValue) {
				console.log('We cant accept', newValue, 'as a price!');
			},
		});
		//probamoslas implementaciones del getter y setter
		console.log(formattedPrice.value);
		formattedPrice.value = 20; // Probando el setter

		//retornamos para poder usar en el template
		return { addToCart, formattedPrice };
	},
	//en options api esto del computed funcionaria así
	// computed: {
	// 	//podemos acceder a las props usando this
	// 	//ejemplo en una propiedad computada
	// 	formattedPrice() {
	// 		return `${this.price.toFixed(2)}`;
	// 	},
	// },
};
</script>
