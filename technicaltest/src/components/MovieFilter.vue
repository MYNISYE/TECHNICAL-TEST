<template>
  <div style="display: flex; gap: 8px;">
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      type="text"
      placeholder="Buscar película por título..." />
    <button
    class="btn btn-primary" 
    @click="$emit('search')">
      Buscar
    </button>
  </div>
</template>

<script setup>
import {defineProps} from 'vue'
defineProps({
  modelValue: {
    type: String,
    required: true
  }
})
</script>
