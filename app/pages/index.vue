<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-3xl font-serif font-bold text-purple-dark">Welcome to the Independer Playground</h1>

    <div class="flex gap-2 mt-4">
        <Icon name="eva:info-outline" class="size-6" />
        Change this file to get started.
    </div>

    <div class="flex flex-col gap-6 mt-6">
        <IndFormInput v-model="name" name="name" label="Naam" />
        <IndButton label="Klik hier" tone="filled-yellow" @click="() => console.log('Button clicked!')" />
    </div>
  </div>
</template>

<script setup>
const name = ref('');
</script>