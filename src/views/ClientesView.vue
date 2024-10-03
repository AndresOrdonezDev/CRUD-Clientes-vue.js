<script setup>
import { onMounted, ref, computed } from "vue";
import { useRouter} from 'vue-router'
import ClienteService from '../services/ClienteService'
import RouterLink from "../components/interface/RouterLink.vue";
import Heading from "../components/interface/Heading.vue";
import ClienteTabla from '../components/Cliente.vue';

const clientes = ref([]);
const router = useRouter()

onMounted(() => {
  ClienteService.obtenerClientes()
    .then(({ data }) => (clientes.value = data))
    .catch((error) => console.log("Error al consultar todos los clientes"+error));
});
defineProps({
  titulo: {
    type: String,
  },
});

const existenClientes = computed(() => {
  return clientes.value.length > 0;
})

const actualizarEstado = ({id,estado})=>{
  ClienteService.cambiarEstado(id, {estado:!estado})
    .then(()=>{
      const index = clientes.value.findIndex(cliente => cliente.id ===id)
      clientes.value[index].estado = !estado
    })
    .catch(()=>{console.log('No se pudo cambiar el estado')})
}
const eliminarCliente = (id)=>{
  ClienteService.eliminarCliente(id)
    .then(()=>{
      clientes.value = clientes.value.filter(cliente => cliente.id !== id)
    })
    .catch(()=>{console.log('no se pudo eliminar el cliente')})
}
</script>

<template>
  <div>
    <div class="flex justify-end">
      <RouterLink to="formulario-cliente"> Agregar Cliente </RouterLink>
    </div>
    <Heading>
      {{ titulo }}
    </Heading>

    <div
      v-if="existenClientes"
      class="flow-root mx-auto mt-10 p-5 bg-white shadow"
    >
      <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="min-w-full py-2 align-middle sm:px-6 lg:px-8">
          <table class="min-w-full divide-y divide-gray-300">
            <thead>
              <tr>
                <th
                  scope="col"
                  class="p-2 text-left text-sm font-extrabold text-gray-600"
                >
                  Nombre
                </th>
                <th
                  scope="col"
                  class="p-2 text-left text-sm font-extrabold text-gray-600"
                >
                  Empresa
                </th>
                <th
                  scope="col"
                  class="p-2 text-left text-sm font-extrabold text-gray-600"
                >
                  Estado
                </th>
                <th
                  scope="col"
                  class="p-2 text-left text-sm font-extrabold text-gray-600"
                >
                  Acciones
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200 bg-white">
               <ClienteTabla
                    v-for="cliente in clientes" 
                    :key="cliente.id"
                    :cliente="cliente"
                    @actualizar-estado="actualizarEstado"
                    @eliminar-cliente="eliminarCliente"
               />
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <p v-else class="text-center mt-10">No hay clientes agregados</p>
  </div>
</template>


