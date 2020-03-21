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
    'k': '000',
    'w': '0 000',
    'm': '000 000',
    'y': '00 000 000',
    'b': '000 000 000'
  }
  for (const key in replaceMap) {
    const element = replaceMap[key];
    expression = expression.replace(new RegExp(key, 'g'), element)
  }
  return expression
}

export default {
  name: 'HelloWorld',
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
      this.$data.result = mexp.eval(expand(this.$data.expression)).toLocaleString()
    }
  }
}
</script>
