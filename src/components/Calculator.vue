<template>
  <div>
    <input v-model="expression" v-on:keyup.enter="calc" />
    <p>
    Results: {{result}}
    <table align="center">
      <tr v-for="item in easyResults" v-bind:key="item">{{item}}</tr>
    </table>
    </p>
  </div>
</template>

<script>
import * as mexp from 'math-expression-evaluator'

const replaceMap = {
  'p': '0.01',
  'h': '100',
  'k': '1 000',
  'w': '10 000',
  'm': '1000 000',
  'y': '100 000 000',
  'b': '1 000 000 000'
}

function expand(expression) {

  const units = Object.keys(replaceMap).join('')
  expression = expression.replace(new RegExp(`([0-9]+(\\.[0-9]*)?[\\s${units}]*)`, 'g'), '($1)')
  for (const key in replaceMap) {
    const element = replaceMap[key];
    expression = expression.replace(new RegExp(key, 'g'), `* ${element}`)
  }
  return expression
}

function toEasyResults(result) {
  var results = []
  for (const key in replaceMap) {
    const element = replaceMap[key]
    const num = mexp.eval(`${result} / ${element}`).toPrecision(3)
    const unit = key == 'p' ? '%' : key
    if (num >= 1 && num < 1000) {
      results.push(`${num} ${unit}`)
    }
  }
  return results
}

export default {
  name: 'Calculator',
  data: function() {
    return {
      expression: '',
      result: '',
      easyResults: []
    }
  },
  props: {
    msg: String
  },
  methods: {
    calc: function() {
      var result = mexp.eval(expand(this.$data.expression))
      this.$data.easyResults = toEasyResults(result)
      if (result > 1000) {
        result = Math.round(result)
      } else if (result > 1 && !Number.isInteger(result)) {
        result = result.toFixed(2)
      }
      this.$data.result = result.toLocaleString()
    }
  }
}
</script>
