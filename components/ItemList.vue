<template>
  <div class="ItemListHolder">
    <div class="FilterHolder">
      <div class="Select">
        <select id="" name="">
          <option value="">
            По умолчанию
          </option>
          <option value="">
            По цене min
          </option>
          <option value="">
            По цене max
          </option>
          <option value="">
            По наименованию
          </option>
        </select>
        <div class="Arrow" />
      </div>
    </div>
    <div class="ItemsGrid">
      <Item
        v-for="(Item, index) in Items"
        :key="index"
        :name="Item.Name"
        :desc="Item.Desc"
        :url="Item.Url"
        :price="Item.Price"
      />
    </div>
  </div>
</template>

<script>
import Item from './Item.vue'
import { emitter } from '@/assets/js/event-bus.js'

export default {
  name: 'ItemList',
  components: { Item },
  data () {
    return {
      Items: []
      /* {
        Name: 'test1',
        Desc: 'test decs',
        Url: 'https://i.ibb.co/Pjy2VLJ/Item-Photo.png',
        Price: '10000'
      }, {
        Name: 'test3',
        Desc: '',
        Url: 'https://i.ibb.co/Pjy2VLJ/Item-Photo.png',
        Price: '10000'
      }, {
        Name: 'test2',
        Desc: 'test decs',
        Url: 'https://i.ibb.co/Pjy2VLJ/Item-Photo.png',
        Price: '10000'
      }
      ] */
    }
  },
  created () {
    function saveStorage (ItemsArr) {
      const parsed = JSON.stringify(ItemsArr)
      localStorage.setItem('Items', parsed)
    }
    emitter.on('ItemAdded', (mess) => {
      this.Items.push(mess)
      saveStorage(this.Items)
    })
    emitter.on('DeleteItem', (Index) => {
      this.Items.splice(Index, 1)
      saveStorage(this.Items)
    })
  },
  mounted () {
    if (localStorage.getItem('Items')) {
      try {
        this.Items = JSON.parse(localStorage.getItem('Items'))
      } catch (e) {
        localStorage.removeItem('Items')
      }
    }
  }
}
</script>

<style lang="scss" scoped>

.ItemListHolder{
  width: 100%;
  display: grid;
  row-gap: 16px;
}

.FilterHolder{
  display: flex;
  justify-content: end;
}

.Select{
  height: 35px;
  position: relative;
  align-items: center;
  display: flex;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  font-size: 12px;
  color: $mianDisableColor;
}

.Arrow{
  background: url('@/static/DropdownArrow.svg') no-repeat;
  background-size: cover;
  width: 8px;
  height: 6px;
  position: relative;
  right: 21px;
}

.Select select{
  font-size: inherit;
  width: 100%;
  height: 100%;
  padding: $mainClickablePadding;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-color: $mainWhite;
}

.ItemsGrid{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  column-gap: 16px;
  row-gap: 16px;
}

</style>
