<script setup>
import { ref, reactive, onMounted, watch } from "vue";
import { uid } from "uid";
import Header from "./components/Header.vue";
import Form from "./components/Form.vue";
import Patient from "./components/Patient.vue";

const patients = ref([]);

const patient = reactive({
  id: null,
  name: "",
  owner: "",
  email: "",
  discharge: "",
  symptoms: "",
});

watch(
  patients,
  () => {
    saveToLocalStorage();
  },
  {
    deep: true,
  }
);

onMounted(() => {
  const storedPatients = localStorage.getItem("patients");
  if (storedPatients) {
    patients.value = JSON.parse(storedPatients);
  }
});

const saveToLocalStorage = () => {
  localStorage.setItem("patients", JSON.stringify(patients.value));
};

const savePatient = () => {
  if (patient.id) {
    const { id } = patient;
    const i = patients.value.findIndex((patient) => patient.id === id);
    patients.value[i] = { ...patient };
  } else {
    patients.value.push({
      ...patient,
      id: uid(),
    });
  }

  Object.assign(patient, {
    name: "",
    owner: "",
    email: "",
    discharge: "",
    symptoms: "",
    id: null,
  });
};

const editPatient = (id) => {
  const updatedPatient = patients.value.filter(
    (patient) => patient.id === id
  )[0];
  Object.assign(patient, updatedPatient);
};

const removePatient = (id) => {
  patients.value = patients.value.filter((patient) => patient.id !== id);
};
</script>
<template>
  <div class="mx-auto container mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Form
        v-model:name="patient.name"
        v-model:owner="patient.owner"
        v-model:email="patient.email"
        v-model:discharge="patient.discharge"
        v-model:symptoms="patient.symptoms"
        @save-patient="savePatient"
        :id="patient.id"
      ></Form>
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Manage your Patients</h3>
        <div v-if="patients.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Patient
            <span class="text-indigo-600 font-bold"> INFO</span>
          </p>
          <Patient
            v-for="patient in patients"
            :patient="patient"
            :key="patient.name"
            @edit-patient="editPatient"
            @remove-patient="removePatient"
          />
        </div>
        <p v-else class="mt-20 text-2xl text-center">There are no Patients</p>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
