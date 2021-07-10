<template>
  <div class="popup__wrapper">
    <div class="popup">
      <header class="popup__header">{{newItem.brand}}</header>
      <div class="popup__input">
        <span class="popup__text"> Brand:</span>
        <input v-model="newItem.brand"/>
      </div>
      <div class="popup__input">
        <span class="popup__text"> Buzzword:</span>
        <input v-model="newItem.buzzword"/>
      </div>
      <div class="popup__input">
        <span class="popup__text"> Medical use:</span>
        <input v-model="newItem.medical_use"/>
      </div>
      <div class="popup__buttons">
        <button v-if="!addingItem" @click="accept()">Save Changes</button>
        <button v-if="!addingItem" @click="deleteItem()"> Delete Item </button>
        <button v-else @click="addItem()">Add item</button>
        <span class="error" v-if="isError">{{errorMessage}}</span>
        <button @click="close()">Cancel</button>
      </div>
    </div>
  </div>
</template>
<script>
// Somewhere i broke the DRY rules in the hope of getting more readable
let validate = function (obj) {
  if (obj.brand && obj.buzzword && obj.medical_use) {
    return true
  }
}
export default {
  props: {
    item: {
      type: Object
    }
  },
  data () {
    return {
      newItem: {},
      addingItem: false,
      errorMessage: 'Please fill in all fields',
      isError: false
    }
  },
  mounted () {
    if (this.item) this.newItem = { ...this.item }
    else this.addingItem = true
  },
  methods: {
    close () {
      this.$emit('closePopup')
    },
    accept () {
      if (validate(this.newItem)) {
        this.$emit('saveChanges', this.newItem)
      } else this.isError = true
    },
    addItem () {
      this.isError = false
      if (validate(this.newItem)) {
        this.newItem.id = Math.floor(Math.random(0, 1000) * 1000)
        console.log(this.newItem.id)
        this.$emit('newItem', this.newItem)
      } else this.isError = true
    },
    deleteItem () {
      this.$emit('deleteItem', this.newItem.id)
    }
  }
}
</script>
<style scoped>
.popup__wrapper {
  display: flex;
  flex-flow: column wrap;
  background: rgba(0,0,0,.8);
  position: fixed;
  height: 100vh;
  justify-content: center;
  align-items: center;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  /* opacity: 0.76; */
  z-index: 1;
}

.popup__input {
  display: flex;
  flex-flow: row;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}
.popup__header {
  font-size: 20px;
  font-weight: 800;
  margin-bottom: 1rem;
}
.popup {
  z-index: 2;
  display: flex;
  align-items: center;
  padding: 10px;
  width: 40%;
  border: 1px solid black;
  display: flex;
  position: absolute;
  flex-flow: column;
  background: white;
}

.popup__buttons {
  display: flex;
  margin-top: 1rem;
  justify-content: space-between;
  width: 100%;
}
input {
  margin: 10px;
  font-size: 12px;
  color: rgb(33, 36, 36);
}
.error {
  color: red;
}
</style>
