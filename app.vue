<template>
  <div class="container register-container">
    <div class="row justify-content-center align-items-center" style="height:100vh;">
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <h1 class="card-title text-center mb-5">Create account</h1>
            <form @submit.prevent="onSubmit">
              <div class="form-group mb-4">
                <label for="name" class="form-label">Name</label>
                <input v-model="data.form.name" type="text" class="form-control" id="name" placeholder="Enter your name">
              </div>
              <div class="form-group mb-4">
                <label for="email" class="form-label">Email address</label>
                <input v-model="data.form.email" type="email" class="form-control" :class="{ 'is-invalid': hasError('email') }" id="email" placeholder="Enter your email">
                <div v-if="data.errors?.email" class="invalid-feedback">
                  <template v-for="error in data.errors.email" :key="error">
                    <span v-html="error"></span><br>
                  </template>
                </div>
              </div>
              <div class="form-group mb-4">
                <label for="password" class="form-label">Password</label>
                <input v-model="data.form.password" type="password" class="form-control" :class="{ 'is-invalid': hasError('password') }" id="password" placeholder="Enter your password">
                <div v-if="data.errors?.password" class="invalid-feedback">
                  <template v-for="error in data.errors.password" :key="error">
                    <span v-html="error"></span><br>
                  </template>
                </div>
              </div>
              <div class="d-grid gap-2">
                <button type="submit" class="btn btn-primary" :disabled="data.loading">Submit</button>
              </div>
              <p class="mt-4 text-center">Already have an account? <a :href="config.public.loginUrl">Log In</a></p>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const config = useRuntimeConfig();
const hasError = (field) => {
    return !!data.errors[field]
}
const data = reactive({
    loading: false,
    errors: {},
    form: {
        name: null,
        email: null,
        password: null
    }
});

const onSubmit = async () => {
    data.loading = true
    try {
        const response = await $fetch('/auth/register', {
            method: 'post',
            body: data.form
        })
        const { $loginUser } = useNuxtApp()
        await $loginUser(response);
    } catch (error) {
        data.errors = error.response._data.errors
        data.loading = false
    }
}
</script>

<style scoped>
  .register-container .card {
    border: none;
    border-radius: 10px;
    box-shadow: 0px 4px 30px rgba(0, 0, 0, 0.1);
  }
  .register-container .card-title {
    font-weight: bold;
    font-size: 2.5rem;
    color: #333;
  }
  .register-container .form-label {
    font-weight: bold;
    color: #333;
  }
  .register-container .form-control {
    border: 2px solid #f3f3f3;
    border-radius: 8px;
    padding: 1.25rem;
    font-size: 1.125rem;
    color: #333;
  }
  .register-container .form-control:focus {
    border-color: #fa9d00;
    box-shadow: none;
  }
  .register-container button {
    border-radius: 8px;
    font-size: 1.25rem;
    padding: 1.25rem;
    background-color: #fa9d00;
    border-color: #fa9d00;
  }
  .register-container button:hover {
    background-color: #fa7f00;
    border-color: #fa7f00;
  }
  .register-container a {
    color: #333;
  }
  .register-container a:hover {
    color: #F5A623;
text-decoration: none;
}

.register-form {
background: #FFFFFF;
border: 1px solid #E2E8F0;
box-sizing: border-box;
border-radius: 4px;
box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.08);
padding: 50px 30px;
width: 100%;
max-width: 500px;
}

.form-label {
font-weight: bold;
}

.form-control {
border: 1px solid #E2E8F0;
border-radius: 4px;
box-sizing: border-box;
margin-bottom: 20px;
padding: 15px;
width: 100%;
}

.form-control:focus {
border-color: #F5A623;
outline: none;
box-shadow: 0 0 10px rgba(245, 166, 35, 0.5);
}

.btn-primary {
background-color: #F5A623;
border-radius: 4px;
color: #FFFFFF;
font-weight: bold;
padding: 15px 40px;
border: none;
}

.btn-primary:hover {
background-color: #F8D87D;
color: #FFFFFF;
}

@media (max-width: 768px) {
.register-container {
padding: 50px;
}
.register-form {
padding: 40px;
}
.form-control {
margin-bottom: 10px;
}
.btn-primary {
padding: 10px 20px;
}
}

@media (max-width: 576px) {
.register-container {
padding: 30px;
}
.register-form {
padding: 30px;
}
.form-control {
padding: 10px;
font-size: 14px;
}
.btn-primary {
padding: 10px 20px;
}
}
</style>