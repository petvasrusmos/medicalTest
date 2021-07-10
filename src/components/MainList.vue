<template>
  <div class="main-wrapper">
    <div>
      <custom-button
        v-if="items.length === 0"
        @click="getData"
        :isLoading="isLoading"
      >
        get data
      </custom-button>
      <div class="menu" v-else>
        <div class="menu__buttons">
          <custom-button
            @click="addItem"
          >
            add item
          </custom-button>
          <custom-button @click="alphabetOrder=!alphabetOrder">
            Filter by alphabet
          </custom-button>
        </div>
        <div class="selector">
          <span> Medical use: </span>
          <select v-model="filters.medical_use">
            <option selected value> Show all</option>
            <option v-for="(option, index) in options" :key="option + index"> {{option}}</option>
          </select>
        </div>
      </div>
      <popup-menu
        @newItem="updateList"
        v-if="addingItem"
        @closePopup="addingItem = false"
      />
    </div>
    <div class="list-wrapper">
      <div class="list">
        <div
          v-for="(item, index) in finalList"
          :key="`${item.brand}${item.id}`"
        >
          <custom-item
            @updateList="updateList"
            @deleteFromArray="deleteItem"
            :index="index"
            :item="item"
          />
        </div>
      </div>
      <div
        v-if="isError"
        class="error"
      >
        {{errorMessage}}
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
import CustomButton from './CustomButton.vue'
import CustomItem from './CustomItem.vue'
import PopupMenu from './PopupMenu.vue'

export default {
  components: {
    CustomButton,
    CustomItem,
    PopupMenu
  },
  data () {
    return {
      text: 'hello',
      items: [],
      isLoading: false,
      isError: false,
      errorMessage: '',
      addingItem: false,
      selected: '',
      filters: { medical_use: '' },
      options: [''],
      alphabetOrder: false
    }
  },
  methods: {
    getData () {
      this.errorMessage = false
      this.isLoading = true
      let uniqueOptions = new Set()
      axios.get('https://random-data-api.com/api/cannabis/random_cannabis?size=30')
        .then(res => {
          this.items = res.data
          this.items.forEach(item => {
            uniqueOptions.add(item.medical_use)
          })
          this.options = [...uniqueOptions]
          Vue.set(this.options)
        })
        .catch(error => {
          console.log(error)
          this.errorMessage = error
          this.isError = true
        })
        .finally(() => {
          this.isLoading = false
        }
        )
    },
    updateList (data) {
      let foundItemIndex = this.items.findIndex(el => el.id === data.id)
      if (foundItemIndex > 0) Vue.set(this.items, foundItemIndex, data)
      else {
        this.items.unshift(data)
        this.addingItem = false
      }
      console.log(foundItemIndex)
      return foundItemIndex
    },
    addItem () {
      this.addingItem = true
    },
    deleteItem (data) {
      let foundItemIndex = this.items.findIndex(el => el.id === data)
      this.items.splice(foundItemIndex, 1)
    }
  },
  computed: {
    filtredList () {
      console.log(this.options.indexOf(this.filters.medical_use))
      if (!this.filters.medical_use) return this.items
      return this.items.filter(item => item.medical_use === this.filters.medical_use)
    },
    finalList () {
      if (!this.alphabetOrder) return this.filtredList
      const filtredListCopy = [...this.filtredList]
      return filtredListCopy.sort((a, b) => a.brand < b.brand ? -1 : 1)
    }
  }
}
</script>

<style scoped>

.error {
  color: rgb(250, 3, 3)
}

.list {
  display: flex;
  flex-flow: column;
  box-sizing: border-box;
  width: 100%;
}

.list-wrapper {
  width: 100%;
  display: flex;
  align-items: center;
  flex-flow: column;
}
.main-wrapper {
  width: 100%;
}

.menu {
  display: flex;
  justify-content: space-between;
}
.menu__buttons {
  display: flex;
  width: 100%;
  justify-content: flex-start;
}

</style>
