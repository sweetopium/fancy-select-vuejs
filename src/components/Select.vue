<style>
  @import '../assets/style/scss/style.scss';
</style>
<template>
  <div class="f-select">
    <div class="fs__select-placeholder">
      <div class="fs__open-indicator" @keyup="onSearchKeyDown">
      <button class="fs__dropdown-input" ref="selectorInput" tabindex="0"
              @mousedown.prevent.stop="handleClick" @focus="onFocus" @blur="onBlur"
            :style="{'fs__disabled-input': disabled}">

        <slot name="selectedValue" v-if="value" >
          <span v-if="value">{{value}}</span>
        </slot>

        <slot name="emptyPlaceholder" v-if="!value">
          <span>{{placeholder}}</span>
        </slot>

        <open-arrow :class="{'fs__reverse-arrow': isOpen}" class="fs__indicator-pointer"/>
      </button>

      <ul ref="dropdownMenu" class="fs__dropdown-menu" v-if="isOpen">
        <li v-for="(option, index) in options" :key="index"
            @mousedown.prevent.stop="selectValue(option)"
            @mouseover.prevent.stop="currentItem = index"
            class="fs__dropdown-menu-option"
            :class='{"fs__active-item": index === currentItem}'>
          <slot name="option" v-bind="normalizeOption(option)">
            {{ getOptionLabel(option) }}
          </slot>
        </li>
      </ul>

      </div>
    </div>


  </div>
</template>
<script>
  import OpenArrow from './OpenArrow'
  export default {
      components: { OpenArrow },
      props: {
          value: {},
          options: {
              type: Array,
              default(){
                  return []
              }
          },
          disabled: {
              type: Boolean,
              default: false
          },
          placeholder: {
              type: String,
              default: 'выбрать значение'
          },
          label: {
              type: String,
              default: 'label'
          },
          reduce: {
              type: Function,
              default: option => option
          },
          getOptionLabel: {
              type: Function,
              default: option => option
          },
          tabIndex: {
              type: Number,
              default: null
          },
      },
      data() {
          return {
            isOpen: false,
            content: this.value,
            currentItem: 0
          }
      },
      methods: {
          onFocus () {
              this.isOpen = true
          },
          onBlur () {
              this.isOpen = false
          },
          handleClick(){
              this.isOpen = !this.isOpen
          },
          selectValue(option){
              if (option !== null) {
                  if (typeof option === 'string') {
                      this.content = option;
                  } else {
                      this.content = this.reduce(option);
                  }
              }
              this.$emit('input', this.content);
              this.$refs.selectorInput.blur();
              this.isOpen = false
          },
          onSearchKeyDown(){
              if(event.key === 'ArrowDown' && this.currentItem+1 < this.options.length) {
                  this.currentItem++
              } else if (event.key === 'ArrowUp' && this.currentItem > 0) {
                  this.currentItem--
              } else if (event.key === 'Enter') {
                  this.selectValue(this.options[this.currentItem])
              } else if (event.key === 'Escape') {
                  this.$refs.selectorInput.blur();
              }
          },
          normalizeOption(option){
              return (typeof option === 'object') ? option : {[this.label]: option};
          }
      }
  }
</script>
