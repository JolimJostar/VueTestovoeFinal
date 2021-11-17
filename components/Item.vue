<template>
  <div
    class="Item"
    @mouseover="hover = true"
    @mouseleave="hover = false"
  >
    <transition name="fade" mode="out-in">
      <button v-show="hover" class="DeleteButton" @click="consoleLog">
        <img src="@/static/Delete.svg" alt="">
      </button>
    </transition>
    <div class="ItemImage" :style="{backgroundImage:`url(${url})`}" />
    <div class="ItemContentHolder">
      <div class="ItemContentHolderText">
        <h3 class="ItemH1">
          {{ name }}
        </h3>
        <p class="ItemP">
          {{ desc }}
        </p>
      </div>
      <h2 class="ItemH3">
        {{ price }} руб. {{ id }}
      </h2>
    </div>
  </div>
</template>

<script>
import { emitter } from '@/assets/js/event-bus.js'

export default {
  name: 'Item',
  props: {
    name: String,
    desc: String,
    price: String,
    url: String,
    id: String
  },
  data () {
    return {
      hover: false
    }
  },
  methods: {
    LogKey () {
      console.log(this.id)
    },
    consoleLog () {
      emitter.emit('DeleteItem', this.$vnode.key)
    }
  }
}
</script>

<style lang="scss" scoped>

.Item{
  display: flex;
  flex-direction: column;
  position: relative;
  background-color: $mainWhite;
  border-radius: $mainBorderRadius;
  box-shadow: $mainShaddow;
  cursor: $mainCursorPointer;
}

.DeleteButton{
  right: -8px;
  top: -8px;
  position: absolute;
  padding: 8px 9.5px;
  background: #FF8484;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

.ItemImage{
  background-image: url(@/static/ItemPhoto.png);
  height: 200px;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  border-radius: 4px 4px 0px 0px;
  flex-shrink: 0;
}

.ItemContentHolder{
  height: 100%;
  padding: 16px;
  padding-bottom: 24px;
  display: flex;
  flex-direction: column;
  row-gap: 32px;
  justify-content: space-between;
  word-break: break-all;
}

.ItemContentHolderText{
  display: flex;
  flex-direction: column;
  row-gap: 16px;
}

</style>
