<script setup>
import { ref, reactive, computed } from "vue";
import Alerta from "./Alerta.vue";
//Utilizando Ref tendriamos que usar las mismas variables del formulario
//const nombre = ref("");
// const leerNombre = e => {
//     nombre.value = e.target.value;
// }

const alerta = reactive({
  tipo: "",
  mensaje: "",
});

const props = defineProps({
  id: {
    type: [String, null],
    required: true,
  },
  nombre: {
    type: String,
    required: true,
  },
  propietario: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  alta: {
    type: String,
    required: true,
  },
  sintomas: {
    type: String,
    required: true,
  },
});

const emit = defineEmits([
  "update:nombre",
  "update:propietario",
  "update:email",
  "update:alta",
  "update:sintomas",
  "guardar-paciente",
]);

const validar = () => {
  // if([paciente.nombre, paciente.propietario].includes('')){
  //     console.log("Almenos uno de esllos está vacio")
  // }

  console.log(Object.values(props));
  if (Object.values(props).includes("")) {
    console.log("Si están vacios");
    alerta.mensaje = "Todos los campos son obligatorios";
    alerta.tipo = "error";
    return;
  }

  emit("guardar-paciente");
  alerta.mensaje = "Paciente Almacenado Correctamente";
  alerta.tipo = "exito";

  setTimeout(() => {
    Object.assign(alerta, {
      tipo: "",
      mensaje: "",
    });
  }, 3000);
};

const editando = computed(() => {
  return props.id;
});
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Añade Pacientes y
      <span class="text-indigo font-bold">Adminístralos</span>
    </p>
    <Alerta v-if="alerta.mensaje" :alerta="alerta" />

    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validar"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">
          Nombre Mascota
        </label>
        <!-- 
                    Formas de usar Input, lineal y con método
                    @input="(e) => nombre = e.target.value"
                      @input="leerNombre"
                -->
        <input
          id="mascota"
          type="text"
          placeholder="Nombre de la mascota"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold">
          Nombre Propieatrio
        </label>
        <input
          id="propietario"
          type="text"
          placeholder="Nombre del propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">
          Email del Propieatrio
        </label>
        <input
          id="email"
          type="email"
          placeholder="Email del propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold"> Alta </label>
        <input
          id="alta"
          type="date"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">
          Síntomas
        </label>
        <textarea
          id="sintomas"
          placeholder="Describe los síntomas de este paciente"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        />
      </div>

      <input
        type="submit"
        :class="[
          editando
            ? 'bg-orange-600 hover:bg-orange-700'
            : 'bg-indigo-600 hover:bg-indigo-700',
        ]"
        class="w-full p-3 text-white uppercase font-bold cursor-pointer transition-colors"
        :value="[editando ? 'Guardar Cambios' : 'Registrar Paciente']"
      />
    </form>
  </div>
</template>
