<template>
  <div class="container">
    <h1>Tarefas</h1>
    <div class="username">
      <h4>{{ userLogged?.user?.name }}</h4>
      <LogoutOutlined class="logout" @click="logout" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { getData } from 'nuxt-storage/local-storage';

const userLogged = ref<any>({});

await getProfile();

async function getProfile() {
  const { data } = await useFetch("http://localhost:8004/api/profile", {
    method: "get",
    headers: {
      Authorization: `Bearer ${getData("token")}`,
      Accept: "application/json",
      "Content-Type": "application/json",
    },
  });

  userLogged.value = data.value;
}

async function logout() {
    const { data, status } = await useFetch(`http://localhost:8004/api/logout`, {
    method: "post",
    body: {},
    headers: {
        Authorization: `Bearer ${getData("token")}`,
        Accept: "application/json",
        "Content-Type": "application/json"
    }
  });

  await navigateTo({ path: "/login" });
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-inline: 10px;
  
  .username {
    display: flex; 
    align-items: center;
  }

  .logout {
    margin-inline: 10px;
    cursor: pointer;
  }
}
</style>
