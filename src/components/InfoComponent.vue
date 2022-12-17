<script setup>
import DescriptionFiller from "./DescriptionFillerComponent.vue";
</script>
<template>
  <div class="borders info">
    <div class="info__image-container">
      <img class="info__image" src="../assets/nicebg.png" alt="avatar" />
    </div>
    <div class="info__description">
      <DescriptionFiller />
    </div>
  </div>
</template>
<style lang="scss">
.info {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: repeat(2, 1fr);
  gap: 20px;
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  padding: 1rem;

  &__image-container {
    height: 100%;
    width: 100%;
    border-radius: 12px;
    overflow: hidden;
  }
  &__image {
    height: 100%;
    width: 100%;
    object-fit: cover;
    object-position: center;
    filter: blur(6px);
  }
  &__description {
    display: flex;
    flex-direction: column;
    flex: 1;
  }
}
</style>
