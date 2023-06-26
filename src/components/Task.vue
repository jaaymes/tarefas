<template>
  <div @click="$emit('taskStateChange', task)" class="task" :class="stateClass">
    <p @click.stop="$emit('taskDeleted', task)" class="close">x</p>
    <p>{{ task.name }}</p>
  </div>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
      required: true,
    },
  },
  computed: {
    stateClass() {
      return {
        pending: this.task.pending,
        done: !this.task.pending,
      };
    },
  },
};
</script>

<style scope>
.task {
  position: relative;
  box-sizing: border-box;
  width: 350px;
  height: 150px;
  padding: 10px;
  border-radius: 8px;
  font-size: 2rem;
  font-weight: 300;
  cursor: pointer;
  user-select: none;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5px;
}
.done {
  background-color: rgb(2, 196, 28);
  color: #fff;
  border-left: 12px solid rgb(31, 252, 130);

  text-decoration: line-through;
}
.pending {
  background-color: rgb(255, 0, 0);
  color: #fff;
  border-left: 12px solid rgb(252, 31, 31);
  box-shadow: 4px 4px rgba(0, 0, 0, 0.2);
}
.pending .close {
  background-color: #b73229;
}
.done .close {
  background-color: #318600;
}
.close {
  position: absolute;
  right: 10px;
  top: 0px;
  font-size: 0.9rem;
  font-weight: 600;
  height: 20px;
  width: 20px;
  border-radius: 10px;
  display: flex;
  justify-content: center;
}
</style>