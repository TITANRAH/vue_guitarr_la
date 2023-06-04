<script setup>


const props = defineProps({
    guitarra: {
        // defino el tipo de props
        type: Object,
        required: true,
    }
})

defineEmits(['agregar-carrito'])

// const incrementar = ()=>{
//     numero.value ++
// }
</script>
<template>
   <div class="col-md-6 col-lg-4 my-4 row align-items-center">
                <div class="col-4">
                    <img class="img-fluid" :src="'/img/' + guitarra.imagen + '.jpg'" :alt="'imagen guitarra' + guitarra.nombre">
                </div>
                <div class="col-8">
                    <h3 class="text-black fs-4 fw-bold text-uppercase">{{ guitarra.nombre  }}</h3>
                   
                    <p>{{ guitarra.descripcion }}</p>
                    <p class="fw-black text-primary fs-3">{{ guitarra.precio }}</p>
                    <!-- la funcion vive en el componente padre App la paso 
                    en el componente guitarra que es este y que vive en el componente padre 
                    la recibo en el defineEmit y la utilizo aca en este boton 
                    por lo que realizara la funcionalidad realizada en el componente padre
                    esto es desde el hijo hacia el padre
                    si quiero enviar algo del hijo al padre uso emit 
                    si quiero enviar algo del padre al hijo uso props
                -->
                    <button 
                        @click="$emit('agregar-carrito', guitarra)"
                        type="button"
                        class="btn btn-dark w-100 "
                    >Agregar al Carrito</button>
                </div>
            </div><!-- FIN GUITARRA -->
</template>


<style lang="scss" scoped></style>