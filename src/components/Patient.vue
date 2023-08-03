<script setup>
const props = defineProps({
  patient: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["edit-patient", "remove-patient"]);
</script>

<template>
  <div class="mx-5 my-10 bg-white shadow-md px-5 py-10 rounded-xl">
    <p class="font-bold mb-3 text-gray-700 uppercase">
      ID:
      <span class="font-normal normal-case"> {{ props.patient.id }}</span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Name:
      <span class="font-normal normal-case"> {{ props.patient.name }}</span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Owner:
      <span class="font-normal normal-case">{{ props.patient.owner }} </span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Email:
      <span class="font-normal normal-case"> {{ props.patient.email }}</span>
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Discharge Date:
      <span class="font-normal normal-case">
        {{ props.patient.discharge }}</span
      >
    </p>

    <p class="font-bold mb-3 text-gray-700 uppercase">
      Symptoms:
      <span class="font-normal normal-case"> {{ props.patient.symptoms }}</span>
    </p>

    <div class="grid md:grid-cols-2 gap-5 mt-10">
      <button
        type="button"
        class="block w-full py-2 px-10 bg-indigo-600 hover:bg-indigo-700 text-white font-bold uppercase rounded-lg"
        @click="emit('edit-patient', props.patient.id)"
      >
        Edit
      </button>

      <button
        type="button"
        class="block w-full py-2 px-10 bg-red-600 hover:bg-red-700 text-white font-bold uppercase rounded-lg"
        @click="emit('remove-patient', props.patient.id)"
      >
        Delete
      </button>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
