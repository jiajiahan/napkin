<template>
  <div>
    <input v-model="expression" v-on:keyup.enter="calc" />
    <p>
    Result: {{result}}
    </p>
  </div>
</template>

<script>
import * as mexp from 'math-expression-evaluator'

function expand(expression) {
  const replaceMap = {
    'p': '0.01',
    'h': '100',
    'k': '1 000',
    'w': '10 000',
    'm': '1000 000',
    'y': '100 000 000',
    'b': '1 000 000 000'
  }
  const units = Object.keys(replaceMap).join('')
  expression = expression.replace(new RegExp(`([0-9]+(\\.[0-9]*)?[\\s${units}]*)`, 'g'), '($1)')
  for (const key in replaceMap) {
    const element = replaceMap[key];
    expression = expression.replace(new RegExp(key, 'g'), `* ${element}`)
  }
  return expression
}

export default {
  name: 'Calculator',
  data: function() {
    return {
      expression: '',
      result: ''
    }
  },
  props: {
    msg: String
  },
  methods: {
    calc: function() {
      var result = mexp.eval(expand(this.$data.expression))
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
