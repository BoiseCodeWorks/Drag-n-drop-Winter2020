<template>
  <div class="Rooms col-6"
       dropzone="zone"
       @dragover.prevent
       @drop.prevent="moveItem"
  >
    <div class="row justify-content-center" draggable="true">
      <div class="col-3 text-light bg-info rounded-pill p-1">
        {{ roomData.name }}
      </div>
    </div>
    <transition-group name="item" tag="div" class="row space-evenly mt-3">
      <Items
        v-for="item in state.items"
        :item-data="item"
        :key="item.id"
        :room-id="roomData.id"
        draggable="true"
      />
    </transition-group>
  </div>
</template>

<script>
import { computed, reactive } from 'vue'
import Items from '../components/Item'
import { itemService } from '../services/ItemService'
import { AppState } from '../AppState'
export default {
  name: 'Rooms',
  props: ['roomData'],
  components: {
    Items
  },
  setup(props) {
    const state = reactive({
      items: computed(() => AppState.rooms[props.roomData.id].items),
      itemToMove: computed(() => AppState.tempItem)
    })
    function moveItem() {
      // const item = JSON.parse(event.dataTransfer.getData('item'))
      // const oldRoom = JSON.parse(event.dataTransfer.getData('room'))
      // const newRoom = props.roomData.id
      // console.log(item, oldRoom, newRoom)
      // itemService.moveItem2(oldRoom, newRoom, item)

      const oldRoom = AppState.rooms.find(r => r.items.includes(state.itemToMove))
      itemService.moveItem(oldRoom.id, props.roomData.id)
    }
    return { state, moveItem, itemToMove: computed(() => AppState.tempItem) }
  }
}
</script>

<style scoped>
.Rooms {
  height: 30vh;
  background-color: white;
  border: 1px;
  border-style: solid;
  border-color: black;
}

.item-enter-active,
.item-leave-active{
  transition: all .5s ease;
  transition: opacity 2s ease;
}

.item-enter-from{
  transform: translate(0px,-20vh);
  opacity: 0;
}

.item-leave-to{
  transform: translate(0px, 20vh);
  opacity: 0;
}
</style>
