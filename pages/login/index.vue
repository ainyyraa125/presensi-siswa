<script setup>
useHead({
  title: "login",
  meta: [
    {
      name: "description",
      content: "Selamat mengisi",
    },
  ],
});

const supabase = useSupabaseClient();
const email = ref("");
const password = ref("");
const loading = ref(false);
const error = ref("");

const Login = async () => {
  loading.value = true;
  error.value = "";
  const { data, error: loginError } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });

  if (loginError) {
    error.value = loginError.message;
  } else if (data) {
    navigateTo('/');
  }

  loading.value = false;
};
</script>

<template>
  <div class="container-fluid mt-5">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <h2>Login</h2>
        <form @submit.prevent="Login">
          <div class="mb-3">
            <label for="email" class="form-label">Email address</label>
            <input 
              v-model="email" 
              type="email" 
              class="form-control" 
              id="email"
              autocomplete="email"
            />
          </div>
          <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input 
              v-model="password" 
              type="password" 
              class="form-control" 
              id="password"
              autocomplete="current-password"
            />
          </div>
          <button 
            type="submit" 
            class="btn btn-primary" 
            :disabled="loading"
          >
            <span v-if="loading">Loading...</span>
            <span v-else>Login</span>
          </button>
          <div v-if="error" class="alert alert-danger mt-3">
            {{ error }}
          </div>
        </form>     
      </div>
    </div>
  </div>
</template>
