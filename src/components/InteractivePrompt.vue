<template>
  <div class="prompt">
    <span>A game</span>
    <template v-for="(frag, i) in fragmentsSubset">
      <template v-for="(lex, j) in frag">
        <span v-if="typeof lex === 'string'" v-text="lex" :key="`${lex}${i}m${j}`"/>
        <Selector v-else-if="Array.isArray(lex)" :choices="lex" :num="3" :key="`c${lex[0]}${i}m${j}`"/>
      </template>
    </template>
    <button @click="setList">Random</button>
  </div>
</template>

<script>

import Selector from '@/components/Selector.vue'
const {rando, randoSequence} = require('@nastyox/rando.js');

export default {
  name: 'InteractivePrompt',
  components: {Selector},
  props: {
    'num': Number
  },
  data () {
    return {
      fragmentsSubset: [],
      temp: [],
      wordlists: {
        game: [
          "Undertale",
          "Chrono Trigger",
          "Sokoban",
          "F-Zero"
        ],
        concept: [
          "Abandonment",
          "Abatement",
          "Hedging",
          "Convergence",
          "Uncertainty",
          "Discrimination",
          "Youth",
          "Truth",
          "Robots"
        ],
        genre: [
          "Fantasy",
          "Sci-fi",
          "Post-apocolyptic",
          "High fantasy"
        ]
      }
    }
  },
  computed: {
    fragments() {
      return [
        [" with gameplay like", this.wordlists.game],
        [" with an engine like", this.wordlists.game],
        [" with an theme like", this.wordlists.game, "'s"],
        [" that's about", this.wordlists.concept],
        [" that touches on", this.wordlists.concept],
        [" set in a", this.wordlists.genre, " theme"],
      ]
    }
  },
  methods: {
    setList(){
      // let fragments = [...this.fragments]
      // this.fragmentsSubset = [fragments.shift(1), ...randoSequence(fragments).map(o => o.value).slice(-this.num)]
      this.fragmentsSubset = randoSequence(this.fragments).map(o => o.value).slice(-this.num)
    }
  },
  mounted() {
    this.setList()
  }
}
</script>

<style scoped lang="scss">
  .prompt {
    > * {vertical-align: top}
    > span {
      margin-top: 2px;
      display: inline-block;
      white-space: pre-wrap;
    }
  }
</style>
