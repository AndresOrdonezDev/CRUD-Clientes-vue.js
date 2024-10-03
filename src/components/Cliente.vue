<script setup>
import {computed} from 'vue'
import { RouterLink } from 'vue-router';

const props = defineProps({
    cliente:{
        type:Object,
        required:true
    }
})

defineEmits(['actualizar-estado','eliminar-cliente'])

const nombreApellido = computed(()=>{
    return props.cliente.nombre + ' '+ props.cliente.apellido
})

const estado = computed(()=>{
    return props.cliente.estado 
})
</script>
<template>
    <tr>
        <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm sm:pl-0">
            <p class="font-medium text-gray-900">{{ nombreApellido }}</p>
            <p class="text-gray-500">{{ cliente.celular }}</p>
            <p class="text-gray-500">{{ cliente.email }}</p>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
            <p class="text-gray-900 font-bold">{{ cliente.empresa }}</p>
            <p class="text-gray-600">{{ cliente.cargo }}</p>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm">
           <button
                class="inline-flex rounded-full px-2 text-xs font-semibold leading-5"
                :class="[estado ? 'bg-green-200 text-green-900' : 'bg-red-200 text-red-900']"
                @click="$emit('actualizar-estado',{id:cliente.id, estado:cliente.estado})"
           >
                {{ estado ? 'Activo' : 'Inactivo' }}
           </button>
        </td>
        <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500 ">
         <router-link
            :to="{name:'editar-cliente',params:{id:cliente.id}}"
            class="text-indigo-600 hover:text-indigo-900 mr-5"
         >Editar</router-link>

         <button 
            class="text-red-600 hover:text-red-900"
            @click="$emit('eliminar-cliente',cliente.id)"
         >
            Eliminar
        </button>
        </td>
    </tr>
</template>