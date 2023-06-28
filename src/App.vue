<template>
  <main>
    <h1 class="title">Optimal City Test</h1>

    <section class="wrapper">
      <form class="form" @submit.prevent="onSubmitHandler">
        <input
          class="form__input _shadow"
          type="text"
          name="editing-item"
          v-model="editableValue"
          placeholder="select item to edit"
          :disabled="!editableIndex"
        />
        <button class="form__btn _shadow" type="submit" :disabled="!editableValue">Save</button>
      </form>
    </section>

    <section class="wrapper">
      <ul class="items">
        <li class="items__item item" v-for="(item, index) in appData" :key="item.name">
          <label
            class="item__wrapper _shadow"
            :class="index === editableIndex ? '_active' : undefined"
          >
            <input
              class="item__radio"
              type="radio"
              name="editable-item"
              :value="index"
              v-model="editableIndex"
              hidden
            />

            <p class="item__name">{{ item.name }}</p>
            <p class="item__value">{{ item.value }}</p>
          </label>
        </li>
      </ul>
    </section>
  </main>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { DataItem, initialData } from "./data";

type AppDataType = {
  appData: DataItem[];
  editableIndex: number | null;
  editableValue: string;
};

export default defineComponent({
  data(): AppDataType {
    return {
      appData: initialData,
      editableIndex: null,
      editableValue: "",
    };
  },
  watch: {
    editableIndex() {
      if (!this.editableIndex) return;

      this.editableValue = this.appData[this.editableIndex].value;
    },
  },
  methods: {
    onSubmitHandler() {
      if (!this.editableIndex || !this.editableValue) return;

      this.appData[this.editableIndex].value = this.editableValue;
    },
  },
});
</script>

<style scoped>
main {
  min-width: 100px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  color: #424242;
  background-color: #fafafa;
  padding: 2rem;
}
.title {
  font-size: 20px;
}
.items {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}
.item {
  width: 120px;
}
.item__wrapper {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  background-color: #eeeeee;
  border: 1px solid #bdbdbd;
  border-radius: 0.75rem;
  padding: 0.5rem;
  transition: all 0.2s ease-out;
  cursor: pointer;
}
.item__wrapper._active {
  color: #fafafa;
  background-color: #42a5f5;
}
.item__wrapper p {
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
.form {
  display: flex;
  gap: 1rem;
}
.form__input {
  border-width: 1px;
  border-style: solid;
  border-color: #bdbdbd;
  border-radius: 0.75rem;
  padding: 0.5rem 0.75rem;
}
.form__input:focus {
  border-color: #42a5f5;
}
.form__btn {
  color: #fafafa;
  background-color: #42a5f5;
  border-radius: 0.75rem;
  padding: 0.75rem 1rem;
}
</style>
