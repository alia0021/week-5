<template>
  <div>
    <slot name="title"></slot>
    <!-- slot is a placeholder for my todoform (component) -->
    <form @submit.prevent="addTodo">
      <!-- prevent prevents the submit to send it to another page so prevents stops the action from happening -->
      <input type="text" v-model="newTodo">
    </form>
    <slot name="desc"></slot>
  </div>
</template>
<script>
export default {
  data() {
    return {
      newTodo: ""
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.length) {
        this.$emit("newTodo", this.newTodo); // emit is a child to the parent prop. u use $emit to send it to the parent
        this.newTodo = "";
      }
    }
  }
};
</script>

<style>
input {
  mind-width: 200px;
  padding: 10px 20px;
  font-size: 1.5rem;
}
</style>