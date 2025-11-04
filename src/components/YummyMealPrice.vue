<template>

	<span>{{ formattedPrice }}</span>
</template>

<script>

// importamos inject para hacer la injection de la variable
import { computed,inject } from 'vue';

export default {

	props: {
		price: Number,
		// currencySymbol :String
	},

	

	setup(props) {

		//inyectamos la variable currencySymbol, que recordar está en App
		//el arbol sería así App -> YummyMeal -> YummyMealPrice
		//donde como podemos ver no hay una relación directa entre App y YummyMealPrice
		//evitamos el prop drilling pasando la prop desde App a YummyMealPrice
		//con valor por defecto '$' como fallback
		//notar que usamos ref para que sea reactiva nuestro fallback
		//para poder acceder al value
		const currencySymbol = inject('currencySymbol', ref('$'));

		// creamos nuestra computed property que está tomada tal cual del componente donde se estaba usando
		//la diferencia esque ahora usamos tambien currencySymbol que es inyectada
		//se accede una variable normal, pero como es un ref, le agregamos el .value
		const formattedPrice = computed({
			//retornamos el valor formateado
			//como anteriormente
			get() {
				return `${currencySymbol.value}${props.price.toFixed(2)}`;
			},
			//en este caso mostramos mensaje de error
			//al intentar asignar un valor
			set(newValue) {
				console.log('We cant accept', newValue, 'as a price!');
			},
		});

		return { formattedPrice };
	},
};
</script>
