<template>
  <div class="row">
    <slot />
  </div>
</template>

<style lang="scss">
.row {
  display: flex;
  flex-wrap: wrap;
  margin: 0 -1rem;
}
</style>