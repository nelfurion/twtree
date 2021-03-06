<template>
  <div class="example-wrapper">
    <div class="title">size</div>
    <div class="panel">
      <TWTree 
      :tree="commonTree" 
      :defaultAttrs="{
        style: {
            fontSize: '18px',
            indent: '40px'
        }
      }"
      ref="stree" 
      class="tree" />
    </div>

    <div class="title">icon</div>
    <div class="panel">
      <TWTree 
        :tree="customIconTree" ref="citree" class="tree">
          <template v-slot:icon="{node}">
            <img class="node-icon" src="folder.svg" v-if="node.hasChild && node.directoryState === 'collapsed'"/>
            <img class="node-icon" src="folder-open.svg" v-else-if="node.hasChild && node.directoryState === 'expanded'"/>
            <img class="node-icon" src="video.svg" v-else-if="!node.hasChild && node.type === 'video'"/>
            <img class="node-icon" src="audio.svg" v-else-if="!node.hasChild && node.type === 'audio'"/>
            <img class="node-icon" src="text.svg" v-else />
          </template>
          <template v-slot:switcher="{node}">
            <svg class="switcher-icon collapsed" viewBox="-3 -3 38 38" fill="none" stroke="currentcolor" stroke-linecap="round" stroke-linejoin="round" stroke-width="3px" v-if="node.directoryState === 'collapsed'">
              <path d="M12 30 L24 16 12 2" />
            </svg>
            <svg class="switcher-icon expanded" viewBox="-3 -3 38 38" fill="none" stroke="currentcolor" stroke-linecap="round" stroke-linejoin="round" stroke-width="3px" v-else-if="node.directoryState === 'expanded'">
              <path d="M30 12 L16 24 2 12" />
            </svg>
          </template>
      </TWTree>
    </div>

    <div class="title">drag and drop</div>
    <div class="panel">
      <TWTree :tree="commonTree" ref="dndtree" class="tree">
        <template v-slot:drag-image="{dnd}">
          <svg class="drag-image-icon droppable" viewBox="0 0 32 32" fill="none" stroke="currentcolor" stroke-linecap="round" stroke-linejoin="round" v-if="dnd.overNode.__.isDroppable">
            <path d="M2 20 L12 28 30 4" />
          </svg>
          <svg class="drag-image-icon notDroppable" viewBox="0 0 32 32" fill="none" stroke="currentcolor" stroke-linecap="round" stroke-linejoin="round" v-else>
            <path d="M2 30 L30 2 M30 30 L2 2" />
          </svg> 
          <span class="drag-image-title">{{dnd.dragNode.title}}</span>
        </template>
        <template v-slot:drag-arrow>
          <svg viewBox="0 0 32 32" :style="{overflow: 'visible'}" width="1.7em" height="2em" fill="none" stroke="currentcolor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2">
              <path d="M22 6 L30 16 22 26 M30 16 L2 16" />
          </svg>
        </template>
      </TWTree>
    </div>

    <div class="title">checkbox</div>
    <div class="panel">
      <TWTree :tree="commonTree" ref="ctree" class="tree" :defaultAttrs="{checkbox:{show:true}}">
        <template v-slot:checkbox="{node}">
          <svg class="checkbox-icon checked" viewBox="0 0 24 24" v-if="node.checkbox.state === 'checked'">
            <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"></path>
            <polyline points="22 4 12 14.01 9 11.01"></polyline>
          </svg>
          <svg class="checkbox-icon unchecked" viewBox="0 0 24 24" v-else-if="node.checkbox.state === 'unchecked'">
            <circle cx="12" cy="12" r="10"></circle>
          </svg>
          <svg class="checkbox-icon undetermined" viewBox="0 0 24 24" v-else-if="node.checkbox.state === 'undetermined'">
            <circle cx="12" cy="12" r="10"></circle>
            <line x1="8" y1="12" x2="16" y2="12"></line>
          </svg>
        </template>
      </TWTree>
    </div>

    <div class="title">background color</div>
    <div class="panel">
      <TWTree 
      :tree="commonTree" 
      :defaultAttrs="{
        style: {
          hoverBgColor: 'lightgray',
          selectedBgColor: '#cc9966',
          dragOverBgColor: 'lightgreen'
        }
      }"
      ref="bctree" 
      class="tree" />
    </div>
    <div class="panel">
      <TWTree 
      :tree="commonTree" 
      @expand="stripes"
      @collapse="stripes"
      ref="stripedTree" 
      class="tree" />
    </div>

    <div class="title">disable animations</div>
    <div class="panel">
      <TWTree 
      :tree="commonTree" 
      animationDuration="0"
      ref="datree" 
      class="tree" />
    </div>

    <div class="title">extra content</div>
    <div class="panel">
      <TWTree :tree="extraTree" class="tree" :defaultAttrs="{style:{extraFloatRight:true}}">
        <template v-slot:extra>
          <svg viewBox="0 0 32 32" class="extra-content" fill="none" stroke="currentcolor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2">
              <path d="M4 16 C1 12 2 6 7 4 12 2 15 6 16 8 17 6 21 2 26 4 31 6 31 12 28 16 25 20 16 28 16 28 16 28 7 20 4 16 Z" />
          </svg>
        </template>
      </TWTree>
    </div>

    <div class="title">limit title width</div>
    <div class="panel">
      <TWTree :tree="widthTree" class="tree" :defaultAttrs="{style:{titleMaxWidth: '3em', titleOverflow: 'ellipsis'}}">
      </TWTree>
    </div>
    <div class="panel">
      <TWTree :tree="widthTree" class="tree" :defaultAttrs="{style:{titleMaxWidth: '20%', titleOverflow: 'ellipsis'}}">
      </TWTree>
    </div>
    <div class="panel">
      <TWTree :tree="widthTree" class="tree" :defaultAttrs="{style:{titleMaxWidth: '-20%', titleOverflow: 'ellipsis'}}">
      </TWTree>
    </div>

  </div>
</template>

<script>
import TWTree from '../components/TWTree.vue'

export default {
  name: 'custom-appearance-example',
  components: {
    TWTree
  },
  data() {
    return {
      commonTree: [
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
                  title: 'child 2-1'
                },
                {
                  id: 5,
                  title: 'child 2-2'
                },
                {
                  id: 6,
                  title: 'child 2-3'
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
      ],

      customIconTree: [
        {
          id: 1,
          title: 'ROOT',
          hasChild: true,
          children: [
            {
              id: 3,
              title: 'videos',
              hasChild: true,
              children: [
                {
                  id: 4,
                  title: 'song of ice and fire',
                  type: 'video'
                },
                {
                  id: 5,
                  title: 'harry potter',
                  type: 'video'
                },
                {
                  id: 6,
                  title: 'the lord of the rings',
                  type: 'video'
                }
              ],
            },
            {
              id: 7,
              title: 'audios',
              hasChild: true,
              children: [
                  {
                      id: 8,
                      title: 'yesterday once more',
                      type: 'audio'
                  },
                  {
                      id: 9,
                      title: 'my heart will go on',
                      type: 'audio'
                  },
                  {
                      id: 10,
                      title: 'lovers',
                      type: 'audio'
                  }
              ]
            },
            {
              id: 11,
              title: 'text 1'
            },
            {
              id: 12,
              title: 'text 2'
            }
          ]
        }
      ],

      extraTree: [
        {
          id: 1,
          title: 'ROOT',
          hasChild: true,
          children: [
            {
              id: 2,
              title: 'child 1',
              style: {
                extraAlwaysVisible: true
              }
            },
            {
              id: 3,
              title: 'child 2',
              hasChild: true,
              children: [
                {
                  id: 4,
                  title: 'child 2-1'
                },
                {
                  id: 5,
                  title: 'child 2-2'
                },
                {
                  id: 6,
                  title: 'child 2-3'
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
      ],

      widthTree: [
        {
          id: 1,
          title: 'ROOT',
          hasChild: true,
          children: [
            {
              id: 2,
              title: 'hello, world! hello, world! hello, world! hello, world! hello, world!',
            },
            {
              id: 3,
              title: 'hello, world! hello, world! hello, world! hello, world! hello, world!',
              hasChild: true,
              children: [
                {
                  id: 4,
                  title: 'hello, world! hello, world! hello, world! hello, world! hello, world!'
                },
                {
                  id: 5,
                  title: 'hello, world! hello, world! hello, world! hello, world! hello, world!'
                },
                {
                  id: 6,
                  title: 'hello, world! hello, world! hello, world! hello, world! hello, world!'
                }
              ],
            },
            {
              id: 7,
              title: 'hello, world! hello, world! hello, world! hello, world! hello, world!'
            },
            {
              id: 8,
              title: 'hello, world! hello, world! hello, world! hello, world! hello, world!'
            }
          ]
        }
      ]



    }
  },
  methods: {
    stripes() {
      let stripedTree = this.$refs.stripedTree
      stripedTree.traverse(function(node) {
        if (node.__.isVisible) {
          let bgColor = node.__.dpos % 2 === 1
                        ? 'lightblue'
                        : 'transparent';
          stripedTree.setAttr(node, 'style', 'bgColor', bgColor)
        }
      })
    }
  },
  mounted() {
    this.stripes()
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
  position: relative;
  margin-bottom: 100px;
}
.panel .tree {
  width: 50%;
}
.title {
    width: auto;
    margin-left: auto;
    margin-right: auto;
    font-size: 24px;
}
.btn {
  width: 100px;
  margin-right: 20px;
}
.node-icon {
    width: 1em;
    height: 1em;
    vertical-align: middle;
}
.switcher-icon {
    width: 1em;
    height: 1em;
    vertical-align: middle;
}
.drag-image-icon {
  width: 1em;
  height: 1em;
  margin-right: 1em;
  stroke-width: 6px;
}
.drag-image-icon.droppable {
  color: green;
}
.drag-image-icon.notDroppable {
  color: red;
}
.drag-image-title {
  text-indent: 0;
  width: auto;
  height: 1.5em;
  line-height: 1.5em;
  padding: 0.3em 0.5em;
  border: 0;
  border-radius: 5px;
  background-color: #eb801c;
  box-shadow: 3px 3px 3px 0px rgba(0,0,0,0.2);
}
.checkbox-icon {
    width: 12px;
    height: 12px;
    vertical-align: middle;
    stroke: #eb801c;
    stroke-width: 2px;
    fill: none;
    stroke-linecap: round;
    stroke-linejoin: round;
    margin-right: 0.3em;
}
.extra-content {
  width: 0.8em;
  height: 0.8em;
  stroke-width: 2;
  fill: red;
  stroke: red;
  stroke-linecap: round;
  stroke-linejoin: round;
  margin-right: 10em;
}
</style>
