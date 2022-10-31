<template>
  <div :class="['col', className]">
    <slot />
  </div>
</template>

<script setup>
const props = defineProps({
  cols: { type: String },
  sm: { type: String },
  md: { type: String },
  lg: { type: String }
})

const className = {
  [`col-${props.cols}`]: !!props.cols,
  [`col-sm-${props.sm}`]: !!props.sm,
  [`col-md-${props.md}`]: !!props.md,
  [`col-lg-${props.lg}`]: !!props.lg
}
</script>

<style lang="scss">
.col {
  position: relative;
  width: 100%;
  min-height: 1px;
  padding: 0 1rem;
}

@mixin make-col-default() {
  flex-shrink: 0;
  width: 100%;
  max-width: 100%;
  padding-right: 1rem;
  padding-left: 1rem;
}

@mixin make-col($size: false, $columns: $grid-columns) {
  @if $size {
    flex: 0 0 auto;
    width: percentage(divide($size, $columns));

  } @else {
    flex: 1 1 0;
    max-width: 100%;
  }
}
</style>