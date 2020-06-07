<template>
  <div class="f-select">

    <slot name="header" />
    <div ref="toggle" class="fs__dropdown-wrapper">
        <div class="fs__selected-options" ref="selectedOptions"></div>
      </div>

    <div class="fs__select-placeholder">

      <div class="fs__open-indicator" @keyup="onSearchKeyDown">

      <open-arrow :class="{'fs__reverse-arrow': isOpen}" class="fs__indicator-pointer"/>

      <input type="text" class="fs__dropdown-input" :placeholder="placeholder"
             @focus="onFocus" tabindex="1" @blur="onBlur" @input="handleInput"
             ref="selectorInput" v-model="content" :disabled="disabled"
             :style="{'fs__disabled-input': disabled}" >

      <ul ref="dropdownMenu" class="fs__dropdown-menu" v-if="isOpen">
        <li v-for="(option, index) in options" :key="index"
            @mousedown.prevent.stop="selectValue(option)"
            @mouseover.prevent.stop="currentItem = index"
            class="fs__dropdown-menu-option"
            :class='{"fs__active-item": index === currentItem}'
        >

          <slot name="option">
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
          handleInput(){
              this.$emit('input', this.content)
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
          }
      }
  }
</script>
<style>
  .fs__select-placeholder {
    position: relative;
  }
  .fs__dropdown-input {
    display: flex;
    padding: 10px 13px;
    background: none;
    border: 1px solid rgba(60,60,60,.26);
    border-radius: 4px;
    white-space: normal;
    width: calc(100% - 26px);
    outline: none;
    font-size: 14px;
    cursor: pointer;
  }
  .fs__dropdown-menu {
    display: block;
    box-sizing: border-box;
    position: absolute;
    top: calc(100% - 2px);
    left: 0;
    z-index: 1000;
    padding: 5px 0;
    margin: 0;
    width: calc(100% + 2px);
    max-height: 350px;
    min-width: 160px;
    overflow-y: auto;
    box-shadow: 0 3px 6px 0 rgba(0,0,0,.15);
    border: 1px solid rgba(60,60,60,.26);
    border-top-style: none;
    border-radius: 0 0 4px 4px;
    text-align: left;
    list-style: none;
    background: #fff;
  }
  .fs__dropdown-menu-option {
    position: relative;
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    color: #212121;
    line-height: 1.5;
  }
  .fs__dropdown-menu-option:hover {
    background-color: #f5f5f5;
    cursor: pointer;
  }
  .fs__open-indicator {
    position: relative;
  }
  .fs__indicator-pointer {
    position: absolute;
    top: 35%;
    right: 1%;
    fill: rgba(60,60,60,.4);
    transform: scale(1);
    transition: transform .15s cubic-bezier(1,-.115,.975,.855);
    transition-timing-function: cubic-bezier(1,-.115,.975,.855);
  }
  .fs__reverse-arrow {
    transform: scale(-1);
  }
  .fs__disabled-input {
    cursor: default;
  }
  .fs__active-item {
    background-color: #f5f5f5;
  }
</style>
