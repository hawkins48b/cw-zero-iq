<template>
  <q-table
    ref="profilesTable"
    :rows="profiles"
    :columns="columns"
    :filter="filter"
    row-key="id"
    title="Saved Profiles"
    flat
    :rows-per-page-options="[0]"
    :class="{'bg-grey-3':!$q.dark.isActive}"
    no-data-label="No profile found."
  >
    <template #top-left>
      <exportBtn />
    </template>
    <template #top-right>
      <q-input
        v-model="filter"
        filled
        debounce="300"
        placeholder="Search"
      >
        <template #append>
          <q-icon name="search" />
        </template>
      </q-input>
    </template>

    <template #header="props">
      <q-tr :props="props">
        <q-th auto-width />
        <q-th
          v-for="col in props.cols"
          :key="col.name"
          :props="props"
        >
          {{ col.label }}
        </q-th>
      </q-tr>
    </template>

    <template #body="props">
      <q-tr :props="props">
        <q-td auto-width>
          <ProfileEditButton
            :profile-id="props.row.id"
            :actions="['edit','duplicate','remove']"
          />
        </q-td>
        <q-td
          v-for="col in props.cols"
          :key="col.name"
          :props="props"
        >
          {{ col.value }}
        </q-td>
      </q-tr>
    </template>
  </q-table>
</template>

<script setup>
// imports
import ExportBtn from 'components/profiles/list/ExportBtn.vue'
import ProfileEditButton from 'components/profiles/ProfileEditButton.vue'
import { useProfilesStore } from 'stores/profiles'
import { storeToRefs } from 'pinia'
import { onMounted, ref/*  computed */ } from 'vue'

// list profiles
const profilesStore = useProfilesStore()
const { profiles } = storeToRefs(profilesStore)

const filter = ref('')

const columns = [
  {
    name: 'weapon',
    field: (row) => row.weapon.name,
    label: 'Weapon',
    align: 'left',
    sortable: true
  },
  {
    name: 'optic',
    field: (row) => row.optic.model,
    label: 'Optic',
    align: 'left',
    sortable: true
  },
  {
    name: 'opticZero',
    field: (row) => row.optic.zero,
    label: 'Zero distance',
    format: (val, row) => `${val} ${row.optic.zeroUnit}`,
    align: 'left',
    sortable: true
  },
  {
    name: 'ammo',
    field: (row) => row.bullet.brand,
    label: 'Ammunition',
    align: 'left',
    sortable: true
  },
  {
    name: 'caliber',
    field: (row) => row.bullet.diameter,
    label: 'Caliber',
    format: (val, row) => `${val} ${row.bullet.diameterUnit}`,
    align: 'left',
    sortable: true
  },
  {
    name: 'weight',
    field: (row) => row.bullet.weight,
    label: 'Weight',
    format: (val, row) => `${val} ${row.bullet.weightUnit}`,
    align: 'left',
    sortable: true
  },
  {
    name: 'velocity',
    field: (row) => row.measures.velocity,
    label: 'Velocity',
    format: (val, row) => `${val} ${row.measures.velocityUnit}`,
    align: 'left',
    sortable: true
  }
]

// sort table after component is mounted
const profilesTable = ref(null)
onMounted(() => {
  profilesTable.value.sort('weapon')
})
</script>
