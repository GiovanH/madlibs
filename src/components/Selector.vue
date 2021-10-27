<template>
  <div class="radiovert" :data-value="this.subset[index]">
    <button @click="setList">Random</button>
    <label v-for="(choice, i) in subset" :key="i">
      <input type="radio" :value="i" v-model="index"/> {{choice}}
    </label>
  </div>
</template>

<script>

const {rando, randoSequence} = require('@nastyox/rando.js');

export default {
  name: 'Selector',
  props: {
    'choices': Array,
    'num': Number
  },
  data () {
    return {
      index: 0,
      subset: []
    }
  },
  methods: {
    setList(){
      this.subset = randoSequence(this.choices).map(o => o.value).slice(-this.num)
    }
  },
  mounted() {
    this.setList()
  }
}
</script>

<style scoped lang="scss">
.radiovert {
  text-align: left;
  button {
    position: absolute;
    margin-top: -25px;
  }
  display: inline-block;
  label {
    display: block;
  }
}
</style>
