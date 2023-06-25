<script setup>
import { ref, reactive, onMounted } from "vue";
import { db } from "./data/guitars";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref(db)
const carrito = ref([])
const guitarraHero = ref({})

onMounted(() => {
	guitarras.value = db
	guitarraHero.value= db[3]
})

/**
 * Funcion que agrega una guitarra al carrito y si ya existe solo aumenta la cantidad
 * @param guitarra 
 * @returns void
 */

const agregarCarrito = (guitarra) => {

	// Buscamos en el carrito el index de la guitarra que nos estan pasando si no existe nos devuelve -1
	const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)

	// Si existe ese index en el carrito entonces solo aumentamos la cantidad
	if (existeCarrito >= 0) {
		// Solo se actualiza la cantidad
		carrito.value[existeCarrito].cantidad++;
	} else {
		// Agregamos la cantidad a cada guitarra que nos agregan al carrito pero solo 1 ya que en el carrito se puede modificar la cantidad
		guitarra.cantidad = 1;
		// Si no existe la guitarra en el carrito la agregamos
		carrito.value.push(guitarra);
	}

}

/**
 * Funcion que decrementa la cantidad de una guitarra en el carrito
 * En caso de que la cantidad sea 1 no se puede decrementar mas
 * @param id
 * @returns void
 */
const decrementarCantidad = (id) => {
	
	const index = carrito.value.findIndex(producto => producto.id === id)
	if(carrito.value[index].cantidad <= 1) return
	carrito.value[index].cantidad--;
}

/**
 * Funcion que incrementa la cantidad de una guitarra en el carrito
 * En caso de que la cantidad sea 5 no se puede incrementar mas
 * @param id
 * @returns void
 */
const incrementarCantidad = (id) => {
	const index = carrito.value.findIndex(producto => producto.id === id)
	if(carrito.value[index].cantidad >= 5) return
	carrito.value[index].cantidad++;
}

/**
 * Eliminar un producto del carrito
 * @param id
 * @returns void
 */
const eliminarProducto = (id) => {
	carrito.value = carrito.value.filter(producto => producto.id !== id)
}


</script>

<template>
	<Header
		:carrito="carrito"
		:guitarra="guitarraHero"
		@agregar-carrito="agregarCarrito"
		@decrementar-cantidad="decrementarCantidad"
		@incrementar-cantidad="incrementarCantidad"
		@eliminar-producto="eliminarProducto"
	/>
	<main class="container-xl mt-5">
		<h2 class="text-center">Nuestra Colección</h2>

		<div class="row mt-5">

      <!-- Para pasar los datos tambien podemos usar v-bind:guitarra="guitarra" -->
			<Guitarra 
        v-for="guitarra in guitarras" 
        :guitarra="guitarra"
				@agregar-carrito="agregarCarrito"
        />

		</div>
	</main>
	<Footer />
</template>

<style scoped>

</style>
