<script setup lang="ts">
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
</script>

<template>
  <ACard
      title="Тестовое задание для RocketSales от Артёма Несина"
  >
    <template #extra>
      <AInputSearch
          v-model:value=search
          placeholder="Найти..."
          @input="loadData()"
      />
    </template>

    <ATable
        :columns="columns"
        :data-source="data"
        :expand-column-width="100"
    >
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
      <template #expandColumnTitle>
        <span>Контакты</span>
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
