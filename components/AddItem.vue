<template>
  <div class="AddItemHolder">
    <h1 class="AddItemH1">
      Добавление товара
    </h1>
    <form action="" class="Form" @submit.prevent="checkForm">
      <div class="FormHolder">
        <div class="FormInputHolder" :class="{Requered: NameIsntValid}">
          <div class="FormImputTextHolder">
            <p class="FormInputText">
              Наименование товара
            </p>
            <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect width="4" height="4" rx="2" fill="#FF8484" />
            </svg>
          </div>
          <input
            v-model="ItemName"
            v-validate="'required'"
            name="Name"
            type="text"
            class="FormInputField"
            placeholder="Введите наименование товара"
            required
          >
          <p />
        </div>
        <div class="FormInputHolder">
          <div class="FormImputTextHolder">
            <p class="FormInputText">
              Описание товара
            </p>
          </div>
          <textarea v-model="ItemDesc" class="FormInputField TextAreaInput" placeholder="Введите описание товара" />
        </div>
        <div class="FormInputHolder" :class="{Requered: UrlIsntValid}">
          <div class="FormImputTextHolder">
            <p class="FormInputText">
              Ссылка на изображение товара
            </p>
            <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect width="4" height="4" rx="2" fill="#FF8484" />
            </svg>
          </div>
          <input
            v-model="ItemImgUrl"
            v-validate="{url: {require_protocol: true }}"
            name="Url"
            type="url"
            class="FormInputField"
            placeholder="Введите ссылку"
            required
          >
          <p />
        </div>
        <div class="FormInputHolder" :class="{Requered: PriceIsntValid}">
          <div class="FormImputTextHolder">
            <p class="FormInputText">
              Цена товара
            </p>
            <svg width="4" height="4" viewBox="0 0 4 4" fill="none" xmlns="http://www.w3.org/2000/svg">
              <rect width="4" height="4" rx="2" fill="#FF8484" />
            </svg>
          </div>
          <input
            v-model="ItemPrice"
            v-validate="'required|numeric'"
            type="number"
            name="Price"
            class="FormInputField"
            placeholder="Введите цену"
            required
          >
          <p />
        </div>
      </div>
      <button type="submit" :class="{Valid: ButtonIsValid}" class="FormButtonSubmit">
        Добавить товар
      </button>
    </form>
  </div>
</template>

<script>
import Vue from 'vue'
import VeeValidate from 'vee-validate'
import { emitter } from '@/assets/js/event-bus.js'

Vue.use(VeeValidate)

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
  computed: {
    ButtonIsValid () {
      if ((this.ItemName && this.isURL(this.ItemImgUrl) && this.ItemPrice && !isNaN(this.ItemPrice))) {
        return true
      } else {
        return false
      }
    },
    NameIsntValid () {
      if (this.errors.first('Name')) {
        return true
      } else {
        return false
      }
    },
    UrlIsntValid () {
      if (this.errors.first('Url')) {
        return true
      } else {
        return false
      }
    },
    PriceIsntValid () {
      if (this.errors.first('Price')) {
        return true
      } else {
        return false
      }
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

    checkForm () {
      if (this.ItemName && this.isURL(this.ItemImgUrl) && !isNaN(this.ItemPrice)) {
        const ItemPayload = {
          Name: this.ItemName,
          Desc: this.ItemDesc,
          Url: this.ItemImgUrl,
          Price: this.ItemPrice,
          ItemId: undefined
        }
        emitter.emit('ItemAdded', ItemPayload)
        this.ItemName = ''
        this.ItemDesc = ''
        this.ItemImgUrl = ''
        this.ItemPrice = ''
      }
    }
  }
}
</script>

<style lang="scss" scoped>

textarea:focus{
  outline: 1px solid gray;
}

input:focus{
  outline: 1px solid gray;
}

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
}

.Valid{
  cursor: $mainCursorPointer;
  background: #7BAE73;
  color: #FFFFFF;
}

.Valid:active{
  outline: green 1px solid;
}

.Requered > input{
  border: 1px solid $mainWarningColor;
}

.Requered > p::after{
  content: 'Поле является обязательным';
  font-size: 8px;
  letter-spacing: $mainLetterSpacing;
  color: $mainWarningColor;
  position: absolute;
}

.warning{
  display: block;
  position: absolute;
  word-break: keep-all;
}

</style>
