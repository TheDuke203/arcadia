<template>
  <div class="edit-user">
    <Form :ref="formRef" v-if="isFormReady">
      <BBCodeEditor :initialValue="editedUser.description" :label="t('general.description')" @valueChange="(val: string) => (editedUser.description = val)" />
      <FloatLabel class="input avatar-url">
        <InputText name="avatar" v-model="editedUser.avatar" />
        <label for="avatar">{{ t('user.avatar_url') }}</label>
      </FloatLabel>
      <FloatLabel class="input email">
        <InputText name="email" v-model="editedUser.email" />
        <label for="email">{{ t('user.email') }}</label>
      </FloatLabel>

      <div class="wrapper-center">
        <Button :label="t('general.validate')" size="small" :loading @click="sendEdits()" />
      </div>
    </Form>
  </div>
</template>

<script setup lang="ts">
import { showToast } from '@/main'
import { type EditedUser, editUser } from '@/services/api/userService'
import { FloatLabel, InputText } from 'primevue'
import { Form } from '@primevue/forms'
import BBCodeEditor from '../community/BBCodeEditor.vue'
import Button from 'primevue/button'
import { ref } from 'vue'
import { useI18n } from 'vue-i18n'
import { onMounted } from 'vue'
import type { VNodeRef } from 'vue'
import { nextTick } from 'vue'

const { t } = useI18n()
const formRef = ref<VNodeRef | null>(null)

const props = defineProps<{
  initialUser: EditedUser
}>()

const editedUser = ref<EditedUser>({
  avatar: '',
  description: '',
  email: '',
})
const loading = ref(false)
const isFormReady = ref(false)

const emit = defineEmits<{
  done: [EditedUser]
}>()

const sendEdits = () => {
  loading.value = true
  editUser(editedUser.value).then(() => {
    loading.value = false
    showToast('Success', t('user.profile_edited_success'), 'success', 4000)
    emit('done', editedUser.value)
  })
}
onMounted(async () => {
  Object.assign(editedUser.value, props.initialUser)
  isFormReady.value = true
  await nextTick()
  formRef.value?.setValues(props.initialUser)
})
</script>

<style scoped>
.edit-user {
  width: 60vw;
}
.input {
  margin-bottom: 25px;
}
.avatar-url {
  width: 60%;
  .p-inputtext {
    width: 100%;
  }
}
</style>
