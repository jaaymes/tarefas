<template>
  <div class="new-task">
    <input
      v-model="name"
      @keydown.enter="add"
      type="text"
      placeholder="Nova tarefa?"
      class="form-element"
    />
    <button @click="add" class="form-element">+</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
    };
  },
  methods: {
    add() {
      this.$emit("taskAdded", { name: this.name });
      this.name = "";
    },
  },
};
</script>

<style scoped>
.new-task {
  margin: 35px;
}
.form-element {
  outline: none;
  font-size: 2rem;
  border: 1px solid #ddd;
  padding: 5px 10px 8px;
  color: #fff;
}
input.form-element {
  width: 500px;
  background: #fff2;
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
}
button.form-element {
  border-left: none;
  background: rgba(10, 238, 21, 0.473);
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  cursor: pointer;
}
</style>