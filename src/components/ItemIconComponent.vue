<script setup>
const props = defineProps({ color: String });
</script>

<template>
  <div class="icon">
    <div class="icon__bottom" :style="`--item-color: ${props.color}`">
      <div class="icon__top"></div>
    </div>
  </div>
</template>

<style lang="scss">
.icon {
  display: flex;
  align-items: center;
  justify-content: center;
  &__bottom {
    height: 3rem;
    width: 3rem;
    position: relative;
    background-color: var(--item-color);
  }
  &__top {
    height: 3rem;
    width: 3rem;
    position: relative;
    top: -6px;
    left: 6px;
    background-color: rgba($color: var(--item-color), $alpha: 0.35);
    backdrop-filter: blur(6px);
  }
  &:active {
    padding: 1rem;
    border: 1px solid yellow;
    border-radius: 12px;
  }
}
</style>
