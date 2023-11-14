<script setup lang="ts">
  import {onMounted, ref} from 'vue'
  import {Lead} from "./models/Lead.ts";
  import axios from './config/axios.config.ts'

  const columns = [
        {
        title: 'Название',
        dataIndex: 'name',
        key: 'name'
      },
      {
        title: 'Бюджет',
        dataIndex: 'price',
        key: 'price'
      },
      {
        title: 'Статус',
        dataIndex: ['status', 'name'],
        key: 'status'
      },
      {
        title: 'Ответственный',
        dataIndex: ['responsible_user', 'name'],
        key: 'responsible_user'
      },
      {
        title: 'Дата создания',
        dataIndex: 'created_at',
        key: 'created_at'
      },
    ];

  // Reactive data
  let tableData = ref([])
  let tableLoading = ref(false)
  let search = ref('')

  // Methods
  async function loadData(): Promise<void> {
    try {
      tableLoading.value = true;

      const { data } = await axios.get<Lead[]>(
          `leads`,
          {
            params: {
              query: search.value
            }
          }
      );

      if (Array.isArray(data)) {
        tableData.value = data;
      }
      else {
        tableData.value = []
      }

      tableLoading.value = false;
    } catch (e) {
      console.log(`При выполнении запроса произошла ошибка: ${e}`)
    }
  }

  // Hooks
  onMounted(loadData)
</script>

<template>
  <ACard
      title="Тестовое задание для RocketSales от Артёма Несина"
  >
    <template #extra>
      <AInputSearch
          v-model:value=search
          placeholder="Найти..."
          @input="loadData"
      />
    </template>

    <ATable
        :columns="columns"
        :row-key="record => record.id"
        :loading="tableLoading"
        :data-source="tableData"
        :expand-column-width="100"
    >

      <template #expandColumnTitle>
        <span>Контакты</span>
      </template>

      <template #expandedRowRender="{ record }">
        <ul>
          <li v-for="contact in record.contacts" :key="contact.id">
            <ASpace>
              {{ contact.name }}

              <a :href="'tel:' + contact.phone_number">
                <PhoneOutlined/>
              </a>

              <a :href="'mailto:' + contact.email">
                <MailOutlined/>
              </a>
            </ASpace>
          </li>
        </ul>
      </template>

      <template #bodyCell="{ column, record }">
        <template v-if="column.key === 'status'">
          <span>
            <ATag
                :color="record.status.color"
            >
              {{ record.status.name }}
            </ATag>
          </span>
        </template>
      </template>
    </ATable>
  </ACard>
</template>

<style scoped>

</style>
