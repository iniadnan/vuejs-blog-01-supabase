<script setup lang="ts">
interface Props {
  type: string
  name: string
  id: string
  title: string
  placeholder?: string | undefined
  value?: string | undefined
}

const props = defineProps<Props>()

const emit = defineEmits<{
  change: [id: number]
  update: [value: string]
}>()
</script>

<template>
  <div>
    <label :for="props.id" class="text-base text-gray-700 inline-block pb-2">{{
      props.title
    }}</label>
    <input
      :type="props.type"
      :name="props.name"
      :id="props.id"
      class="bg-gray-100 w-full focus:outline-none py-2 px-4 rounded"
      :placeholder="props.placeholder"
      :value="props.value"
      @input="$emit('update:modelValue', ($event.target as HTMLInputElement).value)"
    />
  </div>
</template>
