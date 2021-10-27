<template>
  <div class="prompt">
    <span>A game</span>
    <template v-for="(lex, i) in fragmentsSubset">
      <!-- <template v-for="(lex, j) in frag"> -->
        <span v-if="typeof lex === 'string'" v-text="lex" :key="`${lex}${i}m${j}`"/>
        <Selector v-else-if="Array.isArray(lex)" :choices="lex" :num="3" :key="`c${lex[0]}${i}m${j}`"/>
      <!-- </template> -->
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
        genre: [
          "Puzzle platformer"
        ],
        game: [
          "Undertale",
          "Chrono Trigger",
          "Sokoban",
          "F-Zero",
          "Downwell",
          "Device 9",

        ],
        concept: [
          "Abandonment",
          "Hedging",
          "Convergence",
          "Uncertainty",
          "Discrimination",
          "Youth",
          "Truth",
          "Robots"
        ],
        setting: [
          "Fantasy",
          "Sci-fi",
          "Post-apocolyptic",
          "High fantasy",
        ]
      }
    }
  },
  computed: {
    fragments() {
      return [
        [" ", "WORD_WITH", this.wordlists.genre, "mechanics"],
        [" ", "WORD_WITH", " gameplay like", this.wordlists.game],
        [" ", "WORD_WITH", " an engine like", this.wordlists.game],
        [" ", "WORD_WITH", " a theme like", this.wordlists.game, "'s"],
        [" ", "WORD_THAT", " is about", this.wordlists.concept],
        [" ", "WORD_THAT", " touches on", this.wordlists.concept],
        [" set in a", this.wordlists.setting, " setting"],
      ]
    }
  },
  methods: {
    setList(){
      // let fragments = [...this.fragments]
      // this.fragmentsSubset = [fragments.shift(1), ...randoSequence(fragments).map(o => o.value).slice(-this.num)]
      this.fragmentsSubset = randoSequence(this.fragments).map(o => o.value).slice(-this.num).flat()

      let prev_conjunction = ""
      for (let i = 0; i < this.fragmentsSubset.length; i++) {
        let lex = this.fragmentsSubset[i]
        if (lex == "WORD_WITH") {
          if (prev_conjunction == "WORD_WITH") this.fragmentsSubset[i] = "and"
          else this.fragmentsSubset[i] = "with"
          prev_conjunction = "WORD_WITH"
        }
        else if (lex == "WORD_THAT") {
          if (prev_conjunction == "WORD_THAT") this.fragmentsSubset[i] = "and"
          else this.fragmentsSubset[i] = "that"
          prev_conjunction = "WORD_THAT"
        }
      }
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
