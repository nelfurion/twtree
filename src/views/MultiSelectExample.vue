<template>
  <div class="example-wrapper">
    <div class="panel">
      <TWTree 
        :tree="tree" 
        ref="tree" 
        :multiSelect="true" 
        class="tree" 
        :defaultAttrs="{style:{marginTop: '2px', marginBottom: '2px'}}" 
        @select="refreshSelected" 
        @deselect="refreshSelected" />
    </div>
    <span class="title">selected:</span>
    <span class="selected-item" v-for="item of selected" :key="item.title">{{item.title}}</span>
  </div>
</template>

<script>
import TWTree from '../components/TWTree.vue'

export default {
  name: 'multi-select-example',
  components: {
    TWTree
  },
  data() {
    return {
      selected: '',
      tree: [
        {
          id: 1,
          title: 'ROOT',
          hasChild: true,
          children: [
            {
              id: 2,
              title: 'child 1',
            },
            {
              id: 3,
              title: 'child 2',
              hasChild: true,
              children: [
                {
                  id: 4,
                  title: 'child 2-1',
                  selected: true
                },
                {
                  id: 5,
                  title: 'child 2-2'
                },
                {
                  id: 6,
                  title: 'child 2-3',
                  selected: true
                }
              ],
            },
            {
              id: 7,
              title: 'child 3'
            },
            {
              id: 8,
              title: 'child 4'
            }
          ]
        }
      ]
    }
  },
  methods: {
    refreshSelected() {
      this.selected = this.$refs.tree.getSelected()
    }
  },
  mounted() {
    this.refreshSelected()
  }
}
</script>

<style scoped>
.example-wrapper {
  width: 800px;
  margin-left: auto;
  margin-right: auto;
}
.panel {
  width: 100%;
  min-height: 500px;
  box-sizing: border-box;
  padding: 10px;
  margin: 20px 0 20px 0;
  border: 1px solid gray;
  box-shadow: 0 0 5px #ccc;
  border-radius: 5px;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
}
.panel .tree {
  width: 50%;
}
.title {
  font-size: 1.5em;
  font-weight: bold;
}
.selected-item {
  display: inline-block;
  border-radius: 0.3em;
  margin-right: 0.5em;
  margin-left: 0.5em;
  padding-left: 0.5em;
  padding-right: 0.5em;
  color: white;
  background-color: lightseagreen;
}
</style>
