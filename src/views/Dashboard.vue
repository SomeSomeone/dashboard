<template>
  <div class="home">
    <h2>
      Dashboard
      <button v-on:click="addElement">
        Add element <span class="mdi mdi-plus"></span>
      </button>
    </h2>
    <grid-layout
      :layout.sync="layout"
      :col-num="12"
      :row-height="30"
      :is-draggable="true"
      :is-resizable="true"
      :is-mirrored="false"
      :margin="[10, 10]"
      :use-css-transforms="true"
      class="dashboard"
    >
      <grid-item
        v-for="item in layout"
        :x="item.x"
        :y="item.y"
        :w="item.w"
        :h="item.h"
        :i="item.i"
        :key="item.i"
        drag-allow-from=".button-move"
        drag-ignore-from="a"
      >
        <Widget
          :name="item.options.name"
          v-on:delete="() => removeElement(item.i)"
        ></Widget>
      </grid-item>
    </grid-layout>
  </div>
</template>

<script>
import VueGridLayout from "vue-grid-layout";
import Widget from "@/components/Widget";
export default {
  name: "Home",
  components: {
    GridLayout: VueGridLayout.GridLayout,
    GridItem: VueGridLayout.GridItem,
    Widget: Widget
  },
  data: () => ({
    layout: [
      { x: 0, y: 0, w: 4, h: 5, i: 0, options: { name: "Title 1" } },
      { x: 4, y: 0, w: 4, h: 5, i: 1, options: { name: "Title 2" } },
      { x: 8, y: 0, w: 4, h: 5, i: 2, options: { name: "Title 3" } },
      { x: 0, y: 1, w: 4, h: 5, i: 3, options: { name: "Title 4" } },
      { x: 4, y: 1, w: 4, h: 5, i: 4, options: { name: "Title 5" } }
    ]
  }),
  methods: {
    addElement() {
      if (this.layout.length < 5) {
        for (let i = 0; i < 5; i++) {
          if (this.layout.findIndex(item => item.i === i) === -1) {
            this.layout.push({
              x: 0,
              y: 0,
              w: 4,
              h: 5,
              i: i,
              options: { name: `Title ${i + 1}` }
            });
            break;
          }
        }
      }
    },
    removeElement(id) {
      console.log(id);
      let index = this.layout.findIndex(item => item.i === id);
      if (index > -1) {
        this.layout.splice(index, 1);
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.dashboard {
  overflow: hidden;
}
.dashboard::v-deep .vue-grid-item.vue-grid-placeholder {
  background: #2c3e50;
}
</style>
