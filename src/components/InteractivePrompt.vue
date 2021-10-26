<template>
  <div class="prompt">
    <span class="textfrag">A game</span>
    <span class="textfrag">like</span>
    <div class="radiovert">
      <label><input type="radio" :value="0" v-model="temp[0]"/> Mario</label>
      <label><input type="radio" :value="1" v-model="temp[0]"/> Luigi</label>
    </div>
    <span class="textfrag">that's about</span>
    <span class="textfrag">with an engine like</span>
    <span class="textfrag">set in a</span>
    <span class="textfrag">theme</span>
  </div>
</template>

<script>

export default {
  name: 'InteractivePrompt',
  data () {
    return {
      temp: [],
      wordlists: {
        game: [
          "Mario"
        ]
      }
    }
  }
}
</script>
s
<style scoped lang="scss">

.radiovert {
  display: inline-block;
  label {
    display: block;
  }
}
</style>
