<script setup>
    import { computed } from 'vue';
    import CircleProgress from 'vue3-circle-progress'
    import "vue3-circle-progress/dist/circle-progress.css"
    import { formatearCantidad } from '../helpers'
    defineEmits(['reset-app'])

    const props = defineProps({
        presupuesto: {
            type: Number,
            required: true
        },
        disponible: {
            type: Number,
            required: true
        },
        gastado: {
            type: Number,
            required: true
        },
    })

    const porcentaje = computed(() => {
        return parseInt(((props.presupuesto -props.disponible) / props.presupuesto) * 100)
    })
    
</script>

<template>
    <div
        class="dos-columnas"
    >
        <div class="contenedor-grafico">

            <p class="porcentaje">{{ porcentaje }}%</p>
            <CircleProgress 
                :percent="porcentaje"
                :is-gradient="true" 
                :size="250"
                :border-width="30"
                :border-bg-width="30"
                
            />
        </div>

        <div class="contenedor-presupuesto">
            <button 
                class="reset-app"
                type="button"
                @click="$emit('reset-app')"
            >
                Resetear App
            </button>

            <p>
                <span>Presupuesto:</span>
                {{ formatearCantidad(presupuesto) }}
            </p>
            <p>
                <span>Disponible:</span>
                {{ formatearCantidad(disponible) }} 
            </p>
            <p>
                <span>Gastado:</span>
                {{ formatearCantidad(gastado) }} 
            </p>
        </div>
    </div>
</template>

<style scoped>
    .contenedor-grafico{
        position: relative;
    }
    .porcentaje{
        position: absolute;
        margin: auto;
        top: calc(50% - 1.5rem);
        left: 0;
        right: 0;
        text-align: center;
        z-index: 100;
        font-size: 3rem;
        font-weight: 900;
    }
    .dos-columnas{
        display: flex;
        flex-direction: column;
    }

    .reset-app{
        background-color: #c20f0f;
        border: none;
        padding: 1rem;
        width: 100%;
        font-size: 16px;
        transition: 0.5s all ease;
        color: var(--blanco);
        border-radius: 4px;
        font-weight: 900;
        text-transform: uppercase;
    }

    .reset-app:hover{
        background-color: #c20f0f;
        cursor: pointer;
    }

    .dos-columnas > :first-child{
        margin-bottom: 3rem;
    }

    @media (min-width: 768px) {
        
        .dos-columnas{
            flex-direction: row;
            gap: 4rem;
            align-items: center;
        }

        .dos-columnas > :first-child{
        margin-bottom: 0;
    }
    }

    .contenedor-presupuesto{
        width: 100%;
    }
    .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: center;
    }

    @media (min-width: 768px) {
        
        .contenedor-presupuesto p{
        font-size: 2.4rem;
        text-align: left;
    }
    }

    .contenedor-presupuesto span{
        font-weight: 900;
        color: var(--negro);
    }
</style>