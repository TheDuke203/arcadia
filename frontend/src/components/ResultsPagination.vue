<template>
  <div class="pagination">
    <Button :disabled="page < 2" @click="emit('changePage', 'previous')" label="<" size="small" />
    {{ t('general.page') }} {{ page }}
    <Button @click="emit('changePage', 'next')" label=">" size="small" />
  </div>
</template>
<script setup lang="ts">
import { Button } from 'primevue'
import { useI18n } from 'vue-i18n'

const { t } = useI18n()

defineProps<{
  page: number
}>()

const emit = defineEmits<{
  changePage: ['previous' | 'next']
}>()
</script>
