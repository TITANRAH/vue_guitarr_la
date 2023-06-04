<script setup>
import { onMounted, ref, watch } from 'vue';
import Footer from './components/Footer.vue';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';
import { db } from './data/guitarras';


const guitarras = ref([])
const carrito = ref([])
const guitarra = ref({})

// escucha el cambio de un state y realiza la accion requerida
// dep true busca en elementos que sean iguales
watch(carrito, ()=>{
    guardarLocalStorage()
},{
    deep: true
})

onMounted(() => {

    guitarras.value = db;
    guitarra.value = db[3]

    const carritoStorage = localStorage.getItem('carrito')
    if (carritoStorage) {
        carrito.value = JSON.parse(carritoStorage)
    }
})

const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

console.log(guitarras.value)

// defino la funcion que pide como parametro en este caso una guitarra 
// y el console logo recibe esa guitarra, en este caso
// esta siendo emitida desde el componente hijo, que a su vez lo recibio 
// la guitarra desde el arreglo al realizar el v-for
// si quiero realizar desde el padre al hijo uso props 
// si quiero desde el hijo al padre uso emit

// em vue consola puedo ver las priopiedades claramente  
// por ejemplo el array de carrito
const agregarCarrito = (guitarra) => {

    // si un elemento no existe retorna -1 si existe retorna su posicion en el arreglo
    // esrto busca el indice del elemento, si el indice existe retorna su posicion que siempre sera 
    // mayor o igual a cero , y si no existe devolvera -1 por que no encontro el elemento
    const existeCarrito = carrito.value.findIndex(producto => producto.id == guitarra.id)

    // si existe (osea que encontro el indice y es mayor o igual a cero) 
    if (existeCarrito >= 0) {

        // si existe solo aumenta la cantidad de guitarras al darle agregar
        carrito.value[existeCarrito].cantidad++
    } else { //-1

        guitarra.cantidad = 1;
        carrito.value.push(guitarra)
    }

   
}

const decrementarCantidad = (id) => {
    // identidicamos la posicion del elemento
    const index = carrito.value.findIndex(producto => producto.id == id)

    if (carrito.value[index].cantidad <= 1) return;
    // disminuimos la propiedad del elemento
    carrito.value[index].cantidad--

   
}

const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id == id)
    // disminuimos la propiedad del elemento
    if (carrito.value[index].cantidad >= 5) return;
    carrito.value[index].cantidad++

   
}

const eliminarProducto = (id) => {
    // traeme los elementos que sean diferentes al elementos que estoy eliminando 
    // por eso trae todo menos la seleccionada y se provoca el efecto de eliminar
    // y por eso igual al resultado a carrito.value
    carrito.value = carrito.value.filter(producto => producto.id != id)
   

}

const vaciarCarrito = () => {
    carrito.value = []
   
}

</script>

<template>
    <Header :carrito="carrito" :guitarra="guitarra" @decrementar-cantidad="decrementarCantidad"
        @incrementar-cantidad="incrementarCantidad" @agregar-carrito="agregarCarrito" @eliminar-producto="eliminarProducto"
        @vaciar-carrito="vaciarCarrito"></Header>
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">

            <Guitarra v-for="guitarra in guitarras" :guitarra="guitarra" @agregar-carrito="agregarCarrito" />

        </div>
    </main>

    <Footer></Footer>
</template>

<style></style>
