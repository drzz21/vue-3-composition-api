<template>
	<p>
		<strong>{{ name }} </strong> 

		<!-- quitamos la computed formated price porque ahora
		la manejará otro componente exclusivo, y lo hacemos así para validar
		el provide/inject -->

		<!-- importamos el componente y solo le pasaremos como
		prop el precio -->
		<YummyMealPrice :price="price"/>

		<button @click="addToCart">Add to Cart</button>
	</p>
</template>

<script>
// para usar computed properties con composition API
// importamos computed desde vue
// import { computed } from 'vue';

// importamos y declaramos nuestro componente
import YummyMealPrice from './YummyMealPrice.vue';

export default {

	components: {
		YummyMealPrice,
	},

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
		// console.log(props.name);
		// console.log(props.price);

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
// eliminamos la computed propertie
		//probamoslas implementaciones del getter y setter
		// console.log(formattedPrice.value);
		// formattedPrice.value = 20; // Probando el setter

		//retornamos para poder usar en el template
		// return { addToCart, formattedPrice };
		return {addToCart};
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
