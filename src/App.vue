<template>
  <div id="app">

    {{value}}

    <fancy-select v-model="value" :reduce="value => value.valueName.name"
                  :options="nestedOptions" :disabled="disabled" placeholder="выбрать значение"
                  :getOptionLabel="value => value.valueName">

      <template #option="{ valueName }">
        <h3 style="margin: 5px 0">{{valueName.name}}
          <span style="font-size: 12px; color: #696969">({{ valueName.jobTitle }})</span>
        </h3>
      </template>

    </fancy-select>


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
              {label: 'option 1', valueName: {name: "Ivan", jobTitle: 'CMO'}},
              {label: 'option 2', valueName: {name: "John", jobTitle: 'CEO'}},
              {label: 'option 3', valueName: {name: "Peter",jobTitle: 'CTO'}},
              {label: 'option 4', valueName: {name: "Alex", jobTitle: 'CEO'}},
          ]
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
