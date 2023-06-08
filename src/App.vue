<script setup>
import { ref, reactive, onMounted } from "vue";
import { db } from "./data/guitars";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref(db)
const carrito = ref([])

onMounted(() => {
  guitarras.value = db
})

const agregarCarrito = (guitarra) => {

	// Agregamos la cantidad a cada guitarra que nos agregan al carrito pero solo 1 ya que en el carrito se puede modificar la cantidad
	guitarra.cantidad = 1;

	carrito.value.push(guitarra);
}


</script>

<template>
	<Header />
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
