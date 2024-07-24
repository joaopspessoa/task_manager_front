<template>
  <div class="container">
    <div class="container-login">
      <h5 class="title">Bem Vindo</h5>

      <a-form
        :model="formState"
        name="normal_login"
        class="login-form"
        @finish="onFinish"
        @finishFailed="onFinishFailed"
      >
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

        <a-form-item>
          <a-form-item name="remember" no-style>
            <a-checkbox v-model:checked="formState.remember"
              >Lembrar de mim</a-checkbox
            >
          </a-form-item>
          <a class="login-form-forgot" href="">Esqueceu a senha?</a>
        </a-form-item>

        <a-form-item>
          <a-button
            :disabled="disabled"
            type="primary"
            html-type="submit"
            class="login-form-button"
          >
            Entrar
          </a-button>
          Ou
          <NuxtLink to="user/create">registre-se agora!</NuxtLink>
        </a-form-item>
      </a-form>
    </div>
  </div>
</template>

<script setup lang="ts">
interface IReponse {
    token: string;
}

import { reactive, computed } from "vue";
import { UserOutlined, LockOutlined } from "@ant-design/icons-vue";
import { setData } from 'nuxt-storage/local-storage';

interface FormState {
  email: string;
  password: string;
  remember: boolean;
}

const formState = reactive<FormState>({
  email: "",
  password: "",
  remember: true,
});

const onFinish = async (values: any) => {
  const { data, status } = await useFetch<IReponse>(`http://localhost:8004/api/login`, {
    method: "post",
    body: {
        email: formState.email,
        password: formState.password
    },
    headers: {
        "Content-Type": "application/json"
    }
  });

  if (status.value === "success") {
    setData('token', data.value?.token ?? "");
    await navigateTo({ path: "/tasks" });
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
  background-image: url("/assets/images/background-login.png");
  background-position-y: center;
  background-size: contain;
  background-repeat: no-repeat;

  display: flex;
  align-items: center;
  justify-content: flex-end;

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

    .login-form {
      max-width: 300px;
    }
    .login-form-forgot {
      float: right;
    }
    .login-form-button {
      width: 100%;
    }

    .title {
      font-size: 3rem;
      color: #153060;
    }
  }
}
</style>
