<template>
  <div>
    <popup-menu
      @saveChanges="saveChanges"
      v-bind="$props"
      v-if="isOpen"
      @deleteItem="deleteItem"
      @closePopup="isOpen = false"
    />
    <div class="item" @click.prevent="edit()">
      <span class="item__text">{{index + 1 }}.</span>
      <span class="item__text">{{item.brand}}</span>
      <span class="item__text">{{item.buzzword}}</span>
      <span class="item__text">{{item.medical_use}}</span>
    </div>
  </div>
</template>
<script>
import PopupMenu from './PopupMenu.vue'
export default {
  components: {
    PopupMenu
  },
  props: {
    item: {
      type: Object
    },
    index: {
      type: Number
    }
  },
  data () {
    return {
      isOpen: false
    }
  },
  methods: {
    edit () {
      this.isOpen = true
    },
    saveChanges (data) {
      this.$emit('updateList', data)
      this.isOpen = false
    },
    deleteItem (data) {
      this.$emit('deleteFromArray', data)
      this.isOpen = false
    }
  }
  // watch: {
  //   isOpen: function () {
  //     if (this.isOpen) {
  //       document.documentElement.style.overflow = 'hidden'
  //       return
  //     }

  //     document.documentElement.style.overflow = 'auto'
  //   }
  // }
}
</script>
<style scoped>
  .item {
    display: flex;
    width: 100%;
    color: white;
    justify-content: space-between;
    align-items: center;
    padding: 7px 10px;
    width: 100%;
    background: rgba(83,139,0,255);
    /* border: 1px solid rgba(65, 155, 13, 0.863); */
    border-radius: 5px;
    box-sizing: border-box;
    margin: 10px;
    box-shadow: bisque;
    cursor: pointer;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .item__text {
    text-overflow: ellipsis;
  }
  .item:hover {
    -webkit-box-shadow: 4px 4px 39px 2px rgba(34, 60, 80, 0.2);
-moz-box-shadow: 4px 4px 39px 2px rgba(34, 60, 80, 0.2);
box-shadow: 4px 4px 39px 2px rgba(34, 60, 80, 0.2);
  }
</style>
