<script setup>
import { ref, reactive, watch, onMounted } from "vue";
import { uid } from "uid";

import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";
import Paciente from "./components/Paciente.vue";

const pacientes = ref([]);
const paciente = reactive({
  id: null,
  nombre: "Max",
  propietario: "Propietarios",
  email: "umansilla@avaya.com",
  alta: "2024-05-06",
  sintomas: "Sintomas",
});

watch(
  pacientes,
  () => {
    guardarLocalStorage();
  },
  { deep: true }
);

const guardarLocalStorage = () => {
  localStorage.setItem("pacientes", JSON.stringify(pacientes.value));
};

onMounted(() => {
  const pacientesStorage = localStorage.getItem("pacientes");
  if (pacientesStorage) {
    pacientes.value = JSON.parse(pacientesStorage);
  }
});

const guardarPaciente = () => {
  if (paciente.id) {
    //Funcionalidad para editar el paciente, identificando que el pcaiente ya tiene un id
    const { id } = paciente;
    const i = pacientes.value.findIndex((pacienteState) => pacienteState.id === id);
    pacientes.value[i] = { ...paciente };
  } else {
    pacientes.value.push({ ...paciente, id: uid() });
  }

  //Reiniciar objeto
  // paciente.nombre = "";
  // paciente.propietario = "";
  // paciente.email = "";
  // paciente.alta = "";
  // paciente.sintomas = "";

  //Reiniciar objeto de otra forma, el objeto puede ir vacio tambien.
  Object.assign(paciente, {
    nombre: "Max",
    propietario: "Propietarios",
    email: "umansilla@avaya.com",
    alta: "2024-05-06",
    sintomas: "Sintomas",
    id: null,
  });
};

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.filter((paciente) => paciente.id === id)[0];
  Object.assign(paciente, pacienteEditar);
};

const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter((paciente) => paciente.id !== id);
};
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-2 md:flex">
      <Formulario
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="paciente.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus Pacientes</h3>
        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo font-bold">Pacientes</span>
          </p>
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>
        <p v-else class="mt-10 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
