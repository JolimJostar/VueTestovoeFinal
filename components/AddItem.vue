<template>
  <div class="AddItemHolder">
    <h1 class="AddItemH1">
      Добавление товара
    </h1>
    <form action="" class="Form" @submit="checkForm">
      <div class="FormHolder">
        <div class="FormInputHolder">
          <div class="FormImputTextHolder">
            <p class="FormInputText">
              Наименование товара
            </p>
            <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect width="4" height="4" rx="2" fill="#FF8484" />
            </svg>
          </div>
          <input v-model="ItemName" type="text" class="FormInputField" placeholder="Введите наименование товара" required>
        </div>
        <div class="FormInputHolder">
          <div class="FormImputTextHolder">
            <p class="FormInputText">
              Описание товара
            </p>
          </div>
          <textarea v-model="ItemDesc" class="FormInputField TextAreaInput" placeholder="Введите описание товара" />
        </div>
        <div class="FormInputHolder">
          <div class="FormImputTextHolder">
            <p class="FormInputText">
              Ссылка на изображение товара
            </p>
            <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect width="4" height="4" rx="2" fill="#FF8484" />
            </svg>
          </div>
          <input v-model="ItemImgUrl" type="url" class="FormInputField" placeholder="Введите ссылку" required>
        </div>
        <div class="FormInputHolder">
          <div class="FormImputTextHolder">
            <p class="FormInputText">
              Цена товара
            </p>
            <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect width="4" height="4" rx="2" fill="#FF8484" />
            </svg>
          </div>
          <input v-model="ItemPrice" type="number" class="FormInputField" placeholder="Введите цену" required>
        </div>
      </div>
      <button type="submit" class="FormButtonSubmit">
        Добавить товар
      </button>
    </form>
  </div>
</template>

<script>
import { emitter } from '@/assets/js/event-bus.js'

export default {
  name: 'AddItem',
  data () {
    return {
      ItemName: null,
      ItemDesc: null,
      ItemImgUrl: null,
      ItemPrice: null
    }
  },
  methods: {
    isURL (str) {
      const pattern = new RegExp('^(https?:\\/\\/)?' + // protocol
    '((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|' + // domain name
    '((\\d{1,3}\\.){3}\\d{1,3}))' + // OR ip (v4) address
    '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*' + // port and path
    '(\\?[;&a-z\\d%_.~+=-]*)?' + // query string
    '(\\#[-a-z\\d_]*)?$', 'i') // fragment locator
      return !!pattern.test(str)
    },

    checkForm (e) {
      const mess = {
        Name: this.ItemName,
        Desc: this.ItemDesc,
        Url: this.ItemImgUrl,
        Price: this.ItemPrice
      }
      emitter.emit('ItemAdded', mess)
      this.ItemName = ''
      this.ItemDesc = ''
      this.ItemImgUrl = ''
      this.ItemPrice = ''
      e.preventDefault()
    }
  }
}
</script>

<style lang="scss" scoped>

.AddItemHolder{
  align-self: flex-start;
  position: sticky;
  top: 35px;
  width: 332px;
  flex-shrink: 0;
}

.AddItemH1{
  color: $mainBlack;
  font-size: 28px;
  font-family: $mainFont;
  margin-bottom: 16px;
}

.Form{
  padding: 24px;
  display: grid;
  row-gap: 24px;
  background: $mainWhite;
  box-shadow: $mainShaddow;
  border-radius: $mainBorderRadius;
}

.FormHolder{
  display: grid;
  row-gap: 16px;
}

.FormImputTextHolder{
  display: flex;
  flex-direction: row;
  margin-bottom: 4px;
}

.FormInputText{
  font-size: 10px;
  color: $mainBlue;
  letter-spacing: $mainLetterSpacing;
}

.FormInputField{
  width: 100%;
  font-size: 12px;
  padding: $mainClickablePadding;
  background: $mainWhite;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: $mainBorderRadius;
  -moz-appearance:textfield !important;
}

.FormInputField::-webkit-inner-spin-button, .no-spin::-webkit-outer-spin-button {
    -webkit-appearance: none !important;
    margin: 0 !important;
}

.TextAreaInput{
  height: 108px;
}

.FormButtonSubmit{
  background: #EEEEEE;
  border-radius: 10px;
  padding: $mainClickablePadding;
  color: $mianDisableColor;
  font-size: 12px;
  text-align: center;
  font-weight: 600;
  letter-spacing: $mainLetterSpacing;
  cursor: $mainCursorPointer
}

</style>
