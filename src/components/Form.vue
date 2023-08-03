<script setup>
import { reactive, computed } from "vue";
import Alert from "./Alert.vue";

const alert = reactive({
  type: "",
  message: "",
});

const emit = defineEmits([
  "update:name",
  "update:owner",
  "update:discharge",
  "update:symptoms",
  "update:email",
  "save-patient",
]);

const props = defineProps({
  owner: {
    type: String,
    required: true,
  },
  name: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  discharge: {
    type: String,
    required: true,
  },
  symptoms: {
    type: String,
    required: true,
  },
  id: {
    type: [String, null],
    required: true,
  },
});

const validate = () => {
  if (Object.values(props).includes("")) {
    alert.message = "All the fields are required";
    alert.type = "error";
    return;
  }
  emit("save-patient");
  alert.message = "Patient stored successfully";
  alert.type = "success";

  setTimeout(() => {
    Object.assign(alert, {
      type: "",
      message: "",
    });
  }, 3000);
};

const isEditing = computed(() => {
  return props.id;
});
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Patient Tracking</h2>

    <p class="text-lg mt-5 text-center mb-10">
      Add patients and
      <span class="text-indigo-600 font-bold"> manage them</span>
    </p>

    <Alert v-if="alert.message" :alert="alert" />
    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validate"
    >
      <div class="mb-5">
        <label for="pet" class="block text-gray-700 uppercase font-bold">
          Pet's Name</label
        >
        <input
          id="pet"
          type="text"
          placeholder="Pet name"
          class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md mx-auto"
          :value="name"
          @input="emit('update:name', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="owner" class="block text-gray-700 uppercase font-bold">
          Owner's Name</label
        >
        <input
          id="owner"
          type="text"
          placeholder="Owner name"
          class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md mx-auto"
          :value="owner"
          @input="emit('update:owner', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">
          email</label
        >
        <input
          id="email"
          type="email"
          placeholder="Pet owner's Email"
          class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md mx-auto"
          :value="email"
          @input="emit('update:email', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="discharge" class="block text-gray-700 uppercase font-bold">
          discharge</label
        >
        <input
          id="discharge"
          type="date"
          class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md mx-auto"
          :value="discharge"
          @input="emit('update:discharge', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="symptoms" class="block text-gray-700 uppercase font-bold">
          symptoms</label
        >
        <textarea
          id="symptoms"
          placeholder="Describe this patient's symptoms"
          class="border-2 w-full p-2 m-2 placeholder-gray-400 rounded-md h-40 mx-auto"
          :value="symptoms"
          @input="emit('update:symptoms', $event.target.value)"
        />
      </div>
      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor pointer transition-colors"
        :value="[isEditing ? 'Save Changes' : 'Register Patient']"
      />
    </form>
  </div>
</template>
