<script setup>
import ClienteService from '../services/ClienteService'
import {FormKit} from '@formkit/vue'
import { useRouter} from 'vue-router'
import RouterLink from '../components/interface/RouterLink.vue'
import Heading from '../components/interface/Heading.vue';

const router = useRouter()

defineProps({
    titulo:{
        type:String
    }
})

const handleSubmit = (data)=>{
    data.estado = 1
    ClienteService.nuevoCliente(data)
        .then(respuesta =>{
            router.push({name:'inicio'})
        })
        .catch(error=>console.log('Error al hacer la peticion post'))
    
}

</script>

<template>
    <div>
        <div class="flex justify-end">
            <RouterLink to="inicio">
                Inicio
            </RouterLink>
        </div>
        <Heading>
           {{titulo}}
        </Heading>

        <div class="mx-auto mt-10 bg-white shadow rounded-md"> 
            <div class="mx-auto md:w-2/3 py-10  ">
                <FormKit 
                    type="form" 
                    submit-label="Guardar Cliente"
                    incomplete-message="Todos los campos son obligatorios"
                    @submit="handleSubmit"
                >
                    <FormKit
                        type="text"
                        label="Nombre"
                        name="nombre"
                        placeholder="Nombre del cliente"
                        validation="required"
                        :validation-messages="{required:'El Nombre del cliente es obligatorio'}"
                    />
                    <FormKit
                        type="text"
                        label="Apellido"
                        name="apellido"
                        placeholder="Apellido del cliente"
                        validation="required"
                        :validation-messages="{required:'El Apellido del cliente es obligatorio'}"
                    />
                    <FormKit
                        type="email"
                        label="Email"
                        name="email"
                        placeholder="Email del cliente"
                        validation="required|email"
                        :validation-messages="{required:'El Email del cliente es obligatorio',email:'El correo no es válido'}"
                        
                    />
                    <FormKit
                        type="number"
                        label="Celular"
                        name="celular"
                        placeholder="Celuar del cliente"
                        validation="required|length:10"
                        :validation-messages="{required:'El número de celular es obligatorio',length:'El numero de celular debe contener 10 digitos'}"
                    />
                    <FormKit
                        type="text"
                        label="Empresa"
                        name="empresa"
                        placeholder="Empresa a la que pertenece"
                        validation="required"
                        :validation-messages="{required:'La empresa es obligatoria'}"
                    />
                    <FormKit
                        type="text"
                        label="cargo"
                        name="cargo"
                        placeholder="Cargo que ocupa en la empresa"
                        validation="required"
                        :validation-messages="{required:'El cargo del cliente es obligatorio'}"
                    />
                   
                </FormKit>
            </div>
        </div>
    </div>
</template>
<style>
    .formkit-wrapper {
        max-width: 100%;
    }
</style>