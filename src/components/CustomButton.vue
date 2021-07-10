<template>
  <div>
   <button
      class="button"
      v-on="$listeners"
    >
      <span
        class="button__text"
        :class="{loading : isLoading }"
      >
        <slot>
        </slot>
      </span>
    </button>
  </div>
</template>
<script>
export default {
  props: {
    isLoading: {
      type: Boolean
    }
  }
}
</script>
<style scoped>

.error {
  color: rgb(250, 3, 3)
}
.button-wrapper {
  display: relative;
}
.button {
  text-transform: uppercase;
  margin: 3px;
  margin-bottom: 2rem;
  position: relative;
  padding: 8px 15px;
  background: yellowgreen;
  border: none;
  outline: none;
  color: white;
  border-radius: 2px;
  cursor: pointer;
}

.loading {
  visibility: hidden;
}

.loading::after {
  content: '';
  position: absolute;
  visibility: visible;
  width: 16px;
  height: 16px;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  border: 4px solid transparent;
  border-top-color: rgb(252, 252, 252);
  border-radius: 50%;
  animation: button-loading-spinner 1s ease infinite;
}

@keyframes button-loading-spinner {
  from {
    transform: rotate(0turn);
  }

  to {
    transform: rotate(1turn);
  }
}
</style>
