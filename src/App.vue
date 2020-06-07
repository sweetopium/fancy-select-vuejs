<template>
  <div id="app">

    {{value}}
    <fancy-select v-model="value" :reduce="value => value.valueName.val"
                  :options="nestedOptions" :disabled="disabled" placeholder="выбрать значение"
                  :getOptionLabel="value => value.valueName"/>


  </div>
</template>

<script>
import Select from './components/Select.vue'

export default {
  name: 'App',
  components: {
    'fancy-select': Select
  },
  data () {
      return {
          value: null,
          disabled: false,
          arrayOptions: ['one', 'two', 'three', 'lalala'],
          options: [
              {label: 'option 1', value: 'some value'},
              {label: 'option 2', value: 'another value'},
              {label: 'option 3', value: 'not me'},
              {label: 'option 4', value: 'here im'},
          ],
          anotherOptions: [
              {label: 'option 1', valueName: 'some value'},
              {label: 'option 2', valueName: 'another value'},
              {label: 'option 3', valueName: 'not me'},
              {label: 'option 4', valueName: 'here im'},
          ],
          nestedOptions: [
              {label: 'option 1', valueName: {'id': 1, val: 'lalal1'}},
              {label: 'option 2', valueName: {'id': 2, val: 'lalal2'}},
              {label: 'option 3', valueName: {'id': 3, val: 'lalal3'}},
              {label: 'option 4', valueName: {'id': 4, val: 'lalal4'}},
          ],
      }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
