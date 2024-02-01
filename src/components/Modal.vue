<script setup>
    import { ref } from 'vue'
    import Alerta from './Alerta.vue'
    import cerrarModal from '../assets/img/cerrar.svg'

    const error = ref('')

    const emit = defineEmits(['ocultar-modal', 'guardar-gasto', 'update:nombre', 
    'update:cantidad', 'update:categoria', 'eliminar-gasto']);

    const props = defineProps({
        modal: {
            type: Object, 
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        cantidad: {
            type: [String, Number],
            required: true
        },
        categoria: {
            type: String,
            required: true
        },
        disponible: {
            type: Number,
            required: true
        },
        id: {
            type: [String, null],
            required: true
        }
    })

    const old = props.cantidad

    // Validar la data
    const agreglarGasto = () => {
        // validar que no haya campo vacios
        const { cantidad, categoria, nombre, disponible, id } = props

        if([nombre, cantidad, categoria].includes('')){
            error.value = 'Todos los campos son obligatorios'

            setTimeout(() => {
                error.value = ''
            }, 3000);

            return
        }

        // validar la cantidad
        if(cantidad <= 0){
            error.value = 'Cantidad no validad'

            setTimeout(() => {
                error.value = ''
            }, 3000);

            return
        }

        //Validar que el usuario no gaste mas de lo disponible
        if(id){
            // tomar en cuenta el gasto ya realizado
            if(cantidad > old + disponible){
            error.value = 'has excedido el presupuesto'
            setTimeout(() => {
                error.value = ''
            }, 3000);

            return
            }
        }else{
            if(cantidad > disponible){
            error.value = 'has excedido el presupuesto'
            setTimeout(() => {
                error.value = ''
            }, 3000);

            return
            }
        }
      

        emit('guardar-gasto');
    }
</script>

<template>
    <div class="modal">
        <!-- Boton de cerrar modal -->
        <div class="cerrar-modal">
            <img 
                :src="cerrarModal" 
                alt="icono de cerrar modal"
                @click="$event => $emit('ocultar-modal')"
            >
        </div>

        <!-- Formulario -->
        <div 
            class="contenedor contenedor-formulario"
            :class="[modal.animar ? 'animar' : 'cerrar']"
        >
            <form 
                class="nuevo-gasto"
                @submit.prevent="agreglarGasto"
            >
    
                <legend >{{ id ? 'Guardar Cambios' : 'Añadir Gasto' }}</legend>

                <Alerta v-if="error">
                    {{ error }}    
                </Alerta>

                <div class="campo">
                    <label for="nombre">Nombre Gasto:</label>
                    <input 
                        type="text"
                        id="nombre"
                        placeholder="Añade el Nombre del Gasto"
                        :value="nombre"
                        @input="$event => $emit('update:nombre', $event.target.value)"
                    >
                </div>

                <div class="campo">
                    <label for="cantidad">Cantidad:</label>
                    <input 
                        type="number" 
                        id="cantidaad"
                        placeholder="Añade la Cantidad del Gasto, el. 300"
                        :value="cantidad"
                        @input="$event => $emit('update:cantidad', +$event.target.value)"
                    >
                </div>

                <div class="campo">
                    <label for="categoria">Categoria:</label>
                    <select
                        id="categoria"
                        :value="categoria"
                        @input="$event => $emit('update:categoria', $event.target.value)"
                    >
                        <option disabled readonly value="">-- Seleccione --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos Varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripciones</option>
                    </select>
                </div>

                <input 
                    type="submit"
                    :value="[ id ? 'Guardar Cambios' : 'Añadir Gasto']"
                >
            </form>

            <button 
                type="button"
                class="btn-eliminar"
                v-if="id"
                @click="$emit('eliminar-gasto')"
            >
                Eliminar Gasto
            </button>
        </div>
    </div>
</template>

<style scoped>
    .modal{
        position: absolute;
        background-color: rgb(0 0 0 / 0.9);
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }

    .cerrar-modal{
        position: absolute;
        right: 3rem;
        top: 3rem;
    }
    .cerrar-modal img{
        width: 2.5rem;
        cursor: pointer;
    }

    .contenedor-formulario{
        transition-property: all;
        transition-duration: 300ms;
        transition-timing-function: ease-in;
        opacity: 0;
    }

    .contenedor-formulario.animar{
        opacity: 1;
    }

    .contenedor-formulario.cerrar{
        opacity: 0;
    }

    .nuevo-gasto{
        margin: 10rem auto 0 auto;
        display: grid;
        gap: 2rem;
    }

    .nuevo-gasto legend {
        text-align: center;
        color: var(--blanco);
        font-size: 3rem;
        font-weight: 700;
    }

    .campo{
        display: grid;
        gap: 2rem;
    }

.nuevo-gasto input,
.nuevo-gasto select{
    background-color: var(--gris-claro);
    border-radius: 6px;
    padding: 1rem;
    border: none;
    font-size: 2rem;
}

.nuevo-gasto label{
    color: var(--blanco);
    font-size: 2.3rem;
}
.nuevo-gasto input[type="submit"]{
    background-color: var(--primario);
    font-weight:700;
    transition: 0.3s all ease;
}
.nuevo-gasto input[type="submit"]:hover{
    background-color: #dbb700;
    cursor: pointer;
}

.btn-eliminar{
    border: none;
    padding: 1rem;
    width: 100%;
    background-color: #ef4444;
    font-weight: 700;
    font-size: 1.5rem;
    color: var(--blanco);
    margin-top: 10rem;
    cursor: pointer;
    border-radius: 4px;
}

</style>

