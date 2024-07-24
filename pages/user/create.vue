<template>
  <div class="container">
    <div class="container-login">
      <h5 class="title">Cadastre-se</h5>

      <a-form
        :model="formState"
        name="normal_login"
        class="create-form"
        @finish="onFinish"
        @finishFailed="onFinishFailed"
      >
        <a-form-item
          label="Nome"
          name="name"
          :rules="[{ required: true, message: 'Por favor, insira seu nome!' }]"
        >
          <a-input v-model:value="formState.name">
            <template #prefix>
              <UserOutlined class="site-form-item-icon" />
            </template>
          </a-input>
        </a-form-item>

        <a-form-item
          label="Email"
          name="email"
          :rules="[{ required: true, message: 'Por favor, insira seu email!' }]"
        >
          <a-input v-model:value="formState.email">
            <template #prefix>
              <UserOutlined class="site-form-item-icon" />
            </template>
          </a-input>
        </a-form-item>

        <a-form-item
          label="Senha"
          name="password"
          :rules="[{ required: true, message: 'Por favor insira sua senha!' }]"
        >
          <a-input-password v-model:value="formState.password">
            <template #prefix>
              <LockOutlined class="site-form-item-icon" />
            </template>
          </a-input-password>
        </a-form-item>

        <a-form-item
          label="Confirmar Senha"
          name="password_confirmation"
          :rules="[{ required: true, message: 'Por favor insira sua senha!' }]"
        >
          <a-input-password v-model:value="formState.password_confirmation">
            <template #prefix>
              <LockOutlined class="site-form-item-icon" />
            </template>
          </a-input-password>
        </a-form-item>

        <a-form-item>
          <a-button
            :disabled="disabled"
            type="primary"
            html-type="submit"
            class="create-button"
          >
            Cadastrar
          </a-button>
          Ou
          <NuxtLink to="/login">faça login!</NuxtLink>
        </a-form-item>
      </a-form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, computed } from "vue";
import { UserOutlined, LockOutlined } from "@ant-design/icons-vue";
interface FormState {
  name: string;
  email: string;
  password: string;
  password_confirmation: string;
}

const formState = reactive<FormState>({
  name: "",
  email: "",
  password: "",
  password_confirmation: "",
});

const onFinish = async (values: any) => {

  const { data, status } = await useFetch(
    `http://localhost:8004/api/register`,
    {
      method: "post",
      body: formState,
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json",
      },
    }
  );

  if (status.value === 'success') {
    await navigateTo('/login');
  }
};

const onFinishFailed = (errorInfo: any) => {
};

const disabled = computed(() => {
  return !(formState.email && formState.password);
});
</script>

<style lang="scss" scoped>
.container {
  width: 100vw;
  height: 100vh;
  background-image: url("/assets/images/background.png");
  background-size: cover;
  background-repeat: no-repeat;

  display: flex;
  align-items: center;
  justify-content: center;

  .container-login {
    width: 60vh;
    height: 60vh;
    background-color: #e7ebf2;
    margin-right: 100px;
    border-radius: 10px;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    .create-form {
      max-width: 300px;
    }
    .create-forgot {
      float: right;
    }
    .create-button {
      width: 100%;
    }

    .title {
      font-size: 3rem;
      color: #153060;
    }
  }
}
</style>
