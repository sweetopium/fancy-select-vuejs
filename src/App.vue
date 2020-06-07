<template>
  <div id="app">

    <div style="border: 1px solid #959ba5; padding: 0 20px 20px 20px; margin-bottom: 20px">
      <h3>Вложенный массив (+ стили для слотов)</h3>
      <p>Выбранное значение: <strong>{{value}}</strong></p>
      <fancy-select v-model="value" :reduce="value => value.valueName.name"
                  :options="nestedOptions" :disabled="disabled" placeholder="выбрать значение"
                  :getOptionLabel="value => value.valueName">
      <template #selectedValue>
        <span style="color: red">{{value}}</span>
      </template>
      <template #emptyPlaceholder>
        <span style="color: rebeccapurple">выбрать опцию...</span>
      </template>
      <template #option="{ valueName }">
        <h3 style="margin: 5px 0">{{valueName.name}}
          <span style="font-size: 12px; color: #696969">({{ valueName.jobTitle }})</span>
        </h3>
      </template>
    </fancy-select>
    </div>

    <div style="border: 1px solid #959ba5; padding: 0 20px 20px 20px; margin-bottom: 20px">
      <h3>Массив без форматирования лейбла</h3>
      <p>Выбранное значение: <strong>{{noLabelValue}}</strong></p>
      <fancy-select v-model="noLabelValue" :reduce="value => value.valueName" :options="anotherOptions"></fancy-select>
    </div>


    <div style="border: 1px solid #959ba5; padding: 0 20px 20px 20px; margin-bottom: 20px">
      <h3>Массив</h3>
      <p>Выбранное значение: <strong>{{arrayValue}}</strong></p>
      <fancy-select v-model="arrayValue" :options="arrayOptions"></fancy-select>
    </div>


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
          noLabelValue: null,
          arrayValue: null,
          disabled: true,
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
