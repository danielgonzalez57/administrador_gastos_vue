<script setup>
    import {ref} from 'vue'
    import Alerta from './Alerta.vue'

    const presupuesto = ref(0);
    const error = ref('');

    const emit = defineEmits(['definir-presupuesto'])

    const definirPresupuesto = () =>{
        if(presupuesto.value <= 0 || presupuesto.value === ''){
            error.value = 'Presupuesto no valido'

            setTimeout(() =>{
                error.value = ''
            }, 3000);

            return
        }

        emit('definir-presupuesto', presupuesto.value)
    }
</script>

<template>
    <form 
        class="presupuesto"
        @submit.prevent="definirPresupuesto"    
    >
        <Alerta v-if="error">
            {{ error }}
        </Alerta>

        <div class="campo">
            <label for="nuevo-presupuesto">Definir Presupuesto</label>

            <input 
                id="nuevo-presupuesto"
                class="nuevo-presupuesto"
                placeholder="Añade tu presupuesto"
                type="number"
                v-model.number="presupuesto"
            />
        </div>
        
        <input type="submit" value="Definir Presupuesto" />
    </form>
</template>

<style scoped>
    .presupuesto{
        width: 100%;
    }

    .campo{
        display: grid;
        gap: 2rem;
    }

    .presupuesto input[type="number"]{
        background-color: var(--gris-claro);
        border-radius: 1rem;
        padding: 1rem;
        border: none;
        font-size: 2rem;
        text-align: center;
    }
    .presupuesto input[type="submit"]{
        background-color: var(--negro);
        border: none;
        padding: 1rem;
        text-align: center;
        font-size: 1.5rem;
        margin-top: 2rem;
        color: var(--gris-claro);
        border-radius: 4px;
        font-weight: 600;
        width: 100%;
        transition: 0.3s all ease;
    }

    .presupuesto input[type="submit"]:hover{
        background-color: #202020;
        cursor: pointer;
    }

    .presupuesto label{
        font-size: 2.2rem;
        text-align: center;
        color: var(--negro);

    }


</style>

