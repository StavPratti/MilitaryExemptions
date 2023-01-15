<template>
  <v-sheet class="bg-white pa-12" rounded>
    <v-card class="mx-auto px-6 py-8" >
      <v-form v-model="form" @submit.prevent="onSubmit">
        <v-text-field
          v-model="user.username"
          :readonly="loading"
          :rules="[required]"
          class="mb-2"
          clearable
          label="Username"
        ></v-text-field>
        <v-text-field
          v-model="user.password"
          :readonly="loading"
          :rules="[required]"
          clearable
          type="password"
          label="Password"
          placeholder="Enter your password"
        ></v-text-field>
        <v-text-field
          v-model="confirmPassword"
          :readonly="loading"
          :rules="[required]"
          clearable
          type="password"
          label="Confirm Password"
          placeholder="Enter confirm password"
        ></v-text-field>
        <v-text-field
          v-model="user.firstName"
          :readonly="loading"
          :rules="[required]"
          clearable
          label="First Name"
          placeholder="Enter First Name"
        ></v-text-field>
        <v-text-field
          v-model="user.lastName"
          :readonly="loading"
          :rules="[required]"
          clearable
          label="Last Name"
          placeholder="Enter Last Name"
        ></v-text-field>
        <br />
        <v-btn
          :disabled="!form"
          :loading="loading"
          block
          color="success"
          size="large"
          type="submit"
          variant="elevated"
        >
          Sign In
        </v-btn>
      </v-form>
    </v-card>
  </v-sheet>
</template>
  
  <script setup>
import { reactive, ref, watch } from "vue";
import { useAuthStore } from "../stores/auth";
import { useMessageStore } from "../stores/message";
import { computed } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const authStore = useAuthStore();
const messageStore = useMessageStore();

const isLoggedIn = computed(() => authStore.loggedIn);

const user = reactive({
  username: "",
  password: "",
  firtsName: "",
  lastName: "",
});

const form = ref(false);

const confirmPassword = ref("");

const loading = ref(false);

(async () => {
  if (isLoggedIn.value) {
    router.push("/applications");
  }
})();

async function onSubmit() {
  if (!form.value) return;
  if (user.password != confirmPassword.value) {
    messageStore.error = "Passwords not Match !";
    return;
  }
  loading.value = true;
  await authStore
    .register(user)
    .then(setTimeout(() => (loading.value = false), 2000));
}

function required(v) {
  return !!v || "Field is required";
}
</script>