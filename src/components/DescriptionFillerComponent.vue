<script setup></script>
<template>
  <div class="filler">
    <p class="filler__title"></p>
    <div class="filler__text">
      <p class="filler__phrase"></p>
      <p class="filler__phrase"></p>
      <p class="filler__phrase"></p>
      <p class="filler__phrase"></p>
      <p class="filler__phrase"></p>
    </div>
  </div>
</template>
<style lang="scss">
.filler {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;

  &__title {
    height: 1.5rem;
    background: var(--blank-filler);
    border-radius: 4px;
    margin: 0;
  }
  &__text {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    flex: 1;
  }
  &__phrase {
    width: 100%;
    height: 0.65rem;
    background: var(--blank-filler);
    border-radius: 4px;
  }
}
</style>
