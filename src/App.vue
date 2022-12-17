<script setup>
import InfoComponentVue from "./components/InfoComponent.vue";
import InventoryComponentVue from "./components/InventoryComponent.vue";
</script>

<template>
  <div class="app">
    <div class="app__info">
      <InfoComponentVue />
    </div>
    <div class="app__inventory">
      <InventoryComponentVue />
    </div>
    <div class="app__notice">
      <div class="borders notice">
        <div class="notice__filler" />
        <div class="notice__close">
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M18 7.05L16.95 6L12 10.95L7.05 6L6 7.05L10.95 12L6 16.95L7.05 18L12 13.05L16.95 18L18 16.95L13.05 12L18 7.05Z"
              fill="white"
            />
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.app {
  padding: 2rem;
  box-sizing: border-box;
  display: grid;
  grid-template-areas: "info inventory" "notice notice";
  grid-template-columns: 15rem 1fr;
  grid-template-rows: 1fr 5rem;
  min-height: 100vh;
  gap: 1.5rem;
  &__info {
    grid-area: info;
  }
  &__inventory {
    grid-area: inventory;
  }
  &__notice {
    grid-area: notice;
  }
}
.notice {
  height: 100%;
  width: 100%;
  padding: 8px;
  display: flex;
  box-sizing: border-box;
  &__filler {
    background: var(--blank-filler);
    flex: 1;
    margin: 10px;
    box-sizing: border-box;
    border-radius: 12px;
  }
}
</style>
