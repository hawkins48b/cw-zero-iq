<template>
  <q-form @submit="submitProfile">
    <div class="row q-col-gutter-md">
      <div class="col-xs-12 col-sm-6">
        <WeaponCard
          ref="rifle"
          :profile="profile"
        />
        <OpticCard
          :profile="profile"
          class="q-mt-md"
        />
        <MeasuresCard
          :profile="profile"
          class="q-mt-md"
        />
      </div>
      <div class="col-xs-12 col-sm-6">
        <BulletCard :profile="profile" />
      </div>
    </div>
    <div class="row q-mt-md">
      <q-btn
        color="primary"
        :icon="submitIcon"
        type="submit"
        class="col-xs-12 col-sm-6"
      >
        {{ submitText }}
      </q-btn>
    </div>
  </q-form>
</template>

<script setup>
// import
import WeaponCard from 'components/profiles/form/WeaponCard.vue'
import OpticCard from 'components/profiles/form/OpticCard.vue'
import BulletCard from 'components/profiles/form/BulletCard.vue'
import MeasuresCard from 'components/profiles/form/MeasuresCard.vue'

// props
const {
  profile,
  submitText,
  submitIcon
} = defineProps({
  profile: {
    type: Object,
    required: true
  },
  submitText: {
    type: String,
    required: false,
    default: 'Add'
  },
  submitIcon: {
    type: String,
    required: false,
    default: 'add'
  }
})

const emit = defineEmits(['submit'])

// form submit
const submitProfile = () => {
  emit('submit', profile)
}
</script>
