<template>
  <div class="root">
    <div class="content">
      <AddItem />
      <ItemList />
    </div>
  </div>
</template>

<script>
import ItemList from '../components/ItemList.vue'
import AddItem from '@/components/AddItem.vue'

export default {
  name: 'PageIndex',
  components: {
    AddItem,
    ItemList
  }
}
</script>

<style scoped>

.root{
  display: flex;
  justify-content: center;
  background: rgba(255, 254, 251, 0.8)
}

.content{
  display: flex;
  column-gap: 16px;
  width: 1440px;
  padding: 35px;
}

</style>
