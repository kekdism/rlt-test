<script setup>
import { reactive, ref, onMounted, onUpdated } from "vue";
import DescriptionFillerComponent from "./DescriptionFillerComponent.vue";
import ItemIconVue from "./ItemIconComponent.vue";

const inventory = ref([]);

onMounted(() => {
  if (localStorage.getItem("inventory")) {
    inventory.value = JSON.parse(localStorage.getItem("inventory"));
  } else {
    inventory.value = new Array(25).fill(null).map((value, index) => {
      return { id: index.toString(), content: value };
    });

    inventory.value[3].content = {
      id: 3,
      quantity: 10,
    };
    inventory.value[1].content = {
      id: 2,
      quantity: 3,
    };
    inventory.value[2].content = {
      id: 2,
      quantity: 1,
    };
    inventory.value[0].content = {
      id: 1,
      quantity: 6,
    };
  }
});

onUpdated(() => {
  localStorage.setItem("inventory", JSON.stringify(inventory.value));
});

const selected = ref(null);
const isEditing = ref(false);
const deleteCount = ref(0);

const dragStart = (evt, id) => {
  const startSlot = inventory.value.find((slot) => slot.id === id);
  evt.dataTransfer.dropEffect = "move";
  evt.dataTransfer.effectAllowed = "move";
  evt.dataTransfer.setData("prevSlot", startSlot.id);
};

const onDrop = (evt, id) => {
  const prevSlotId = evt.dataTransfer.getData("prevSlot");
  const prevSlot = inventory.value.find((slot) => slot.id === prevSlotId);
  const newSlot = inventory.value.find((slot) => slot.id === id);
  if (newSlot === prevSlot) {
    return;
  }
  if (
    newSlot?.content !== null &&
    newSlot?.content.id === prevSlot?.content.id
  ) {
    newSlot.content.quantity += prevSlot?.content.quantity;
    prevSlot.content = null;
  } else if (
    newSlot?.content !== null &&
    newSlot?.content.id !== prevSlot?.content.id
  ) {
    const temp = prevSlot?.content;
    prevSlot.content = newSlot?.content;
    newSlot.content = temp;
  } else {
    newSlot.content = prevSlot.content;
    prevSlot.content = null;
  }
};

const closeInfo = () => {
  stopEditing();
  selected.value = null;
};

const items = [
  {
    id: 1,
    icon: ItemIconVue,
    description: DescriptionFillerComponent,
  },
  {
    id: 2,
    icon: ItemIconVue,
    description: DescriptionFillerComponent,
  },
  {
    id: 3,
    icon: ItemIconVue,
    description: DescriptionFillerComponent,
  },
];
const getItemIcon = (id) => {
  return items.find((item) => item.id === id)?.icon;
};

const showSlotInfo = (slot) => {
  const item = getItem(slot.content.id);
  selected.value = { item, slot };
};

const getItem = (id) => {
  return items.find((item) => item.id === id);
};

const stopEditing = () => {
  deleteCount.value = 0;
  isEditing.value = false;
};
const confirmDeletion = (slot) => {
  if (deleteCount.value < 0) {
    stopEditing();
    return;
  }
  const newQuantity = slot.content.quantity - deleteCount.value;
  if (newQuantity < 1) {
    slot.content = null;
    closeInfo();
    return;
  }
  slot.content.quantity = newQuantity;
  stopEditing();
};

const itemColors = (id) => {
  switch (id) {
    case 1:
      return "red";
    case 2:
      return "blue";
    case 3:
      return "green";
  }
};
</script>
<template>
  <div class="borders inventory">
    <div
      v-for="slot in inventory"
      :key="slot.id"
      class="inventory__slot"
      @drop="onDrop($event, slot.id)"
      @dragenter.prevent
      @dragover.prevent
    >
      <component
        v-if="slot?.content"
        :is="getItemIcon(slot?.content?.id)"
        :color="itemColors(slot?.content?.id)"
        draggable="true"
        @dragstart="dragStart($event, slot.id)"
        @click="showSlotInfo(slot)"
      >
      </component>
      <div v-if="slot?.content?.quantity > 1" class="inventory__quantity">
        {{ slot?.content?.quantity }}
      </div>
    </div>
    <div v-if="selected" class="inventory__info">
      <button class="inventory__close" @click="closeInfo">
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
      </button>
      <div class="inventory__container">
        <component
          v-if="selected.item.icon"
          :is="selected.item.icon"
          :color="itemColors(selected.item.id)"
          class="inventory__big-image"
        />
        <component
          v-if="selected.item.description"
          :is="selected.item.description"
          class="inventory__description"
        />
        <button
          v-if="!isEditing"
          class="inventory__delete"
          @click="isEditing = true"
        >
          Удалить предмет
        </button>
        <div class="inventory__edit" v-if="isEditing">
          <input class="inventory__input" type="number" v-model="deleteCount" />
          <button class="inventory__cancel" @click="stopEditing">Отмена</button>
          <button
            class="inventory__confirm"
            @click="confirmDeletion(selected.slot)"
          >
            Подтвердить
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<style lang="scss">
.inventory {
  width: 100%;
  height: 100%;
  overflow: hidden;
  display: grid;
  position: relative;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
  &__slot {
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    border-width: 1px;
    border-style: solid;
    border-color: #4d4d4d;
    border-top: 0;
    border-left: 0;
    &:nth-child(5n) {
      border-right: 0;
    }
    &:nth-child(n + 21) {
      border-bottom: 0;
    }
    &:hover {
      background-color: rgba($color: white, $alpha: 0.02);
    }
  }
  &__quantity {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    bottom: 0;
    right: 0;
    width: 1rem;
    height: 1rem;
    border: 1px solid #4d4d4d;
    border-bottom: 0;
    border-right: 0;
    border-top-left-radius: 4px;
    color: rgba(255, 255, 255, 0.35);
    font-weight: 500;
    font-size: 10px;
    line-height: 12px;
  }
  &__info {
    position: absolute;
    width: 50%;
    height: 100%;
    backdrop-filter: blur(0.5rem);
    right: 0;
    padding: 0.5rem;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
  }
  &__close {
    background: none;
    padding: 0;
    border: none;
    display: block;
    margin: 0 0 0 auto;
    text-align: center;
    cursor: pointer;
  }
  &__container {
    flex: 1;
    display: flex;
    flex-direction: column;
  }
  &__big-image {
    padding: 2rem;
    border-bottom: 1px solid #4d4d4d;
  }
  &__description {
    margin: 1rem;
    border-bottom: 1px solid #4d4d4d;
  }
  &__delete {
    font-size: 14px;
    line-height: 17px;
    padding: 11px;
    background-color: #fa7272;
    border-radius: 8px;
    border: none;
    color: white;
  }
  &__edit {
    display: flex;
    flex-direction: column;
  }
  &__cancel {
    font-size: 14px;
    line-height: 17px;
    padding: 11px;
    background-color: white;
    border-radius: 8px;
    border: none;
    color: #fa7272;
  }
  &__confirm {
    font-size: 14px;
    line-height: 17px;
    padding: 11px;
    background-color: #fa7272;
    border-radius: 8px;
    border: none;
    color: white;
  }
}
</style>
