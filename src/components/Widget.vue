<template>
  <article class="widget">
    <header class="widget-head">
      <button class="button button-move mdi mdi-cursor-move"></button>
      {{ name }}
      <button
        class="button button-delete mdi mdi-trash-can-outline"
        v-on:click="$emit('delete')"
      ></button>
    </header>
  </article>
</template>

<script>
export default {
  name: "Widget",
  props: {
    name: {
      type: String
    }
  }
};
</script>

<style lang="scss" scoped>
.widget {
  width: 100%;
  height: 100%;
  border: 2px solid #2c3e50;
  box-sizing: border-box;
  .widget-head {
    background: rgba(44, 62, 80, 12%);
    border-bottom: 2px solid #2c3e50;
    padding: 5px 10px;
  }
}
.button {
  position: absolute;
  width: 32px;
  height: 32px;
  font-size: 24px;
  border: 2px solid #2c3e50;
  padding: 0;
  margin: 0;
  color: #2c3e50;
  background: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  top: 0;
  cursor: pointer;
  &.button-move {
    left: 0;
  }
  &.button-delete {
    right: 0;
    background: #ff0067;
  }
  &:focus {
    outline: 0;
  }
}
</style>
