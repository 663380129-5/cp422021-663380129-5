<script setup>
import InfoCard from '@/components/cards/InfoCard.vue';
import TableInfoCard from "@/components/cards/TableInfoCard.vue";
import { useTableStore } from '@/store/table';

const tableStore = useTableStore();

const reserveTable = (table) => {
  const date = new Date()
  table.status = "reserve"
  table.checkin = `${date.getHours()} : ${date.getMinutes()}`
};

</script>
<template>
  <VCard>
    <VCardItem>
      <VCardTitle>โต๊ะในร้าน</VCardTitle>
    </VCardItem>
    <VCardText>
      <VRow>
        <VCol cols="3">
          <InfoCard
            title="โต๊ะทั้งหมด"
            :stats="10"
            unit="ตัว"
            icon="mdi-table"
            color="primary"
          />
        </VCol>
        <VCol cols="3">
          <InfoCard
            title="โต๊ะว่าง"
            :stats="tableStore.tables.filter(table => table.status === 'Ready').length"
            unit="ตัว"
            icon="mdi-table-plus"
            color="success"
          />
        </VCol>
        <VCol cols="3">
          <InfoCard
            title="ใช้งานอยู่"
            :stats="tableStore.tables.filter(table => table.status === 'Reserve').length"
            unit="ตัว"
            icon="mdi-table-account"
            color="warning"
          />
        </VCol>
        <VCol cols="3">
          <VCard class="align-center justify-center d-flex fill-height">
            <VBtn
              class="fill-height"
              variant="text"
              block
              text
            >
              <VIcon>mdi-plus</VIcon>
              เพิมโต๊ะใหม่
            </VBtn>
          </VCard>
        </VCol>
      </VRow>
    </VCardText>
  </VCard>
  <VCard class="mt-8">
    <VCardText>
      <v-row>
      <v-col v-for="table in tableStore.tables" cols="3" class ="d-flex align-center justify-center">
        <v-btn @click="reserveTable(table)" v-if="table.status === 'ready'" size="x-large" block prepend-icon="mdi-table" height="200">
          {{ table.name }} - {{ table.status }}
        </v-btn>
        <TableInfoCard :table="table" v-else-if="table.status === 'reserve'"></TableInfoCard>
      </v-col>
    </v-row>
    </VCardText>
  </VCard>
</template>
