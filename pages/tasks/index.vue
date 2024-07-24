<template>
  <div class="container">
    <div class="filters">
      <a-select
        v-model:value="filter"
        size="middle"
        style="width: 200px; margin-right: 10px"
        @change="handleChange"
        allowClear
        :options="options"
      ></a-select>

      <a-button
        type="primary"
        @click="showModal"
        shape="circle"
        :icon="h(PlusOutlined)"
      />
    </div>

    <a-row :gutter="{ xs: 8, sm: 16, md: 24, lg: 32 }" class="row">
      <a-col class="gutter-row" :span="8">
        <div class="gutter-box">
          <h6 class="title-box">Novo</h6>
          <div
            class="task"
            v-for="pendingTask in pendingTasks"
            :key="pendingTask.id"
          >
            <p class="title-task">
              {{ pendingTask.title }}
              <DeleteOutlined @click="handleDelete(pendingTask.id)" />
            </p>
            <p class="description-task">
              {{ pendingTask.description }}
            </p>
          </div>
        </div>
      </a-col>
      <a-col class="gutter-row" :span="8">
        <div class="gutter-box">
          <h6 class="title-box">Em andamento</h6>
          <div
            class="task"
            v-for="inProgressTask in inProgressTasks"
            :key="inProgressTask.id"
          >
            <p class="title-task">
              {{ inProgressTask.title }}
              <DeleteOutlined @click="handleDelete(inProgressTask.id)" />
            </p>
            <p class="description-task">
              {{ inProgressTask.description }}
            </p>
          </div>
        </div>
      </a-col>
      <a-col class="gutter-row" :span="8">
        <div class="gutter-box">
          <h6 class="title-box">Finalizado</h6>
          <div
            class="task"
            v-for="completedTask in completedTasks"
            :key="completedTask.id"
          >
            <p class="title-task">
              {{ completedTask.title }}
              <DeleteOutlined @click="handleDelete(completedTask.id)" />
            </p>
            <p class="description-task">
              {{ completedTask.description }}
            </p>
          </div>
        </div>
      </a-col>
    </a-row>
  </div>

  <a-modal v-model:open="open" title="Criar Task" @ok="onFinish">
    <a-form
      :model="formState"
      name="normal_login"
      class="create-form"
      @finish="onFinish"
      @finishFailed="onFinishFailed"
    >
      <a-form-item
        label="Título"
        name="title"
        :rules="[{ required: true, message: 'Por favor, insira um título!' }]"
      >
        <a-input v-model:value="formState.title"> </a-input>
      </a-form-item>

      <a-form-item
        label="Descrição"
        name="description"
        :rules="[
          { required: true, message: 'Por favor, insira uma descrição!' },
        ]"
      >
        <a-input v-model:value="formState.description"> </a-input>
      </a-form-item>

      <a-form-item label="Status" name="status">
        <a-select
          v-model:value="formState.status"
          size="middle"
          style="width: 200px"
          :options="options"
        ></a-select>
      </a-form-item>

      <template #footer>
        <a-button key="back" @click="handleCancel">Fechar</a-button>
        <a-button
          :disabled="disabled"
          html-type="submit"
          key="submit"
          type="primary"
          >Submit</a-button
        >
      </template>
    </a-form>
  </a-modal>
</template>

<script setup lang="ts">
import { PlusOutlined } from "@ant-design/icons-vue";
import { getData } from "nuxt-storage/local-storage";
import { ref } from "vue";
definePageMeta({
  layout: "home",
});

interface FormState {
  description: string;
  title: string;
  status: string;
}

interface ITaskReponse {
  task_completed: any[];
  tasks_in_progress: any[];
  tasks_pending: any[];
}

const pendingTasks = ref<any[]>([]);
const inProgressTasks = ref<any[]>([]);
const completedTasks = ref<any[]>([]);
const filter = ref<string>("");

const open = ref<boolean>(false);
const options = [
  { value: "pending", label: "Novo" },
  { value: "in progress", label: "Em andamento" },
  { value: "completed", label: "Finalizado" },
];

const formState = reactive<FormState>({
  title: "",
  description: "",
  status: "",
});

const onFinish = async (values: any) => {

  const { data, status } = await useFetch(`http://localhost:8004/api/tasks`, {
    method: "post",
    body: formState,
    headers: {
      Authorization: `Bearer ${getData("token")}`,
      Accept: "application/json",
      "Content-Type": "application/json",
    },
  });

  if (status.value === "success") {
    open.value = false;
    await getTasks('');
  }
};

const onFinishFailed = (errorInfo: any) => {
};

await getTasks('');

const showModal = () => {
  open.value = true;
};

const handleChange = () => {
    getTasks(`?status=${filter.value}`);
}

const handleCancel = () => {
  open.value = false;
};

const handleDelete = async (taskId: string) => {
  const { data, status } = await useFetch(`http://localhost:8004/api/tasks/${taskId}`, {
    method: "delete",
    headers: {
      Authorization: `Bearer ${getData("token")}`,
      Accept: "application/json",
      "Content-Type": "application/json",
    },
  });

  if (status.value === 'success') {
    getTasks('');
  }
};

const disabled = computed(() => {
  return !(formState.title && formState.description && formState.status);
});

async function getTasks(params: string) {
  const { data } = await useFetch<ITaskReponse>(
    `http://localhost:8004/api/tasks${params}`,
    {
      method: "get",
      headers: {
        Authorization: `Bearer ${getData("token")}`,
        Accept: "application/json",
        "Content-Type": "application/json",
      },
    }
  );

  pendingTasks.value = data.value?.tasks_pending ?? [];
  inProgressTasks.value = data.value?.tasks_in_progress ?? [];
  completedTasks.value = data.value?.task_completed ?? [];
}
</script>

<style lang="scss" scoped>
.container {
  width: 100vw;
  height: 90vh;
  background-image: url("/assets/images/background.png");
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;

  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;

  .filters {
    margin-block: 20px;
  }

  .row {
    width: 100%;
    flex-flow: row;

    .gutter-row {
      margin-inline: 0.3rem;

      .gutter-box {
        padding: 10px;
        height: auto;
        width: 300px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: flex-start;
        border-radius: 10px;
        background-color: #e7ebf2;

        .title-box {
          font-size: 1rem;
          margin: 0;
        }

        .task {
          cursor: pointer;
          margin-top: 10px;
          border-radius: 10px;
          padding-inline: 10px;
          background-color: #c3c7ce;

          .title-task {
            font-size: 0.8rem;
            font-weight: 500;
            margin-left: 20px;

            display: flex;
            align-items: center;
            justify-content: space-between;
          }

          .description-task {
            max-width: 100%;
            min-width: 280px;
          }
        }
      }
    }
  }
}
</style>
