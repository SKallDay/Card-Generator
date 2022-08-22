<script>
import { ref } from 'vue'
import Card from './Card.vue';
export default {
  name: "Container",
  data() {
    return {
      cards: 0,
      cols: 0,
      randomLetters: [],
    }
  },
  components: {
    Card,
  },
  computed: {
    isDisabled() {
      // check card or columns are populated to disable button
      if (!this.cards || !this.cols) {
        return true
      }
    }
  },
  methods: {
    submit(e) {
      e.preventDefault()
      const cards = parseInt(e.target.elements.cards.value)
      const cols = parseInt(e.target.elements.cols.value)
      if (cards <= 5 && cols <= 5) {
        this.cols = e.target.elements.cols.value
        this.cards = e.target.elements.cards.value
      }
      const totalLetters = cards * cols
      const randomLetters = this.generateRandomLetters(totalLetters, cols)
      this.randomLetters = randomLetters
    },

    generateRandomLetters(numOfLetters, cols) {
      // Possible characters A-Z, split into an array
      const characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("");
      // generate array, use sort to randomize order of characters
      // slice array to numbers of characters needed
      const randomLettersArray = [...characters]
        .sort(() => (Math.random() > 0.5 ? 1 : -1))
        .slice(0, numOfLetters);
      return this.splitArrayIntoRows(randomLettersArray, cols)
    },

    splitArrayIntoRows(arr, len) {
      // Split array into sub arrays used for letter grid
      var chunks = []
      // Iterate over array and slice into chunks
      for (let i = 0; i < arr.length; i += len) {
        const chunk = arr.slice(i, i + len);
        chunks.push(chunk)
      }
      // returns an array of arrays
      return chunks;
    },

    handleInput(e) {
      // update values for inputs
      const inputName = e.target.name
      const value = parseInt(e.target.value)

      inputName === 'cards' ? this.cards = value : this.cols = value
    }
  }
};
</script>

<template>
  <div class="container">
    <h1 class="header fw700 text-5xl mb3">Card Generator</h1>
    <form class="form p3 items-center flex border-rd-1" v-on:submit="submit">
      <div class="form-container flex items-center">
        <div class="flex items-center">
          <label>Generate</label>
          <input class="textInput flex items-center justify-center p2 mr2 ml2 border-rd-1" type="text"
            :v-model=this.cards name="cards" pattern="[+]?([0-4]*\.[0-9]+|[0-5])" required @input="handleInput" />
          <p mb10>random cards, &nbsp</p>
        </div>
        <div class="flex items-center">
          <p>each with </p>
          <input class="textInput flex items-center justify-center p2 mr2 ml2 border-rd-1" type="text"
            :v-model=this.cols name="cols" max="5" pattern="[+]?([0-4]*\.[0-9]+|[0-5])" required @input="handleInput" />
          <label>rows/columns.</label>
        </div>
      </div>
      <button class="button border-rd-1 text-white" :disabled=isDisabled>
        Generate
      </button>
    </form>
  </div>

  <div v-if="randomLetters.length" class="grid-container border-rd-1 flex flex-col mt8">
    <div class="grid-row flex flex-row" v-for="(row, index1) in randomLetters">
      <div v-for="(letter, index) in row">
        <Card :msg="letter" :row=index1 :col=index />
      </div>
    </div>
  </div>

</template>

<style>
#app {
  background: #E5E5E5;
  height: 100vh;
}

.container {
  width: 80%;
  margin: 0 auto;
  padding-top: 52px;
}

.grid-container {
  border: 12px solid black;
  width: fit-content;
  margin: 0 auto;
}

.grid-row {
  display: flex;
  flex-direction: row;
}

.header {
  font-family: 'Roboto Slab';
}

.form {
  background: #FFFFFF;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
}

.form-container {
  display: flex;
  align-items: center;
}

.button {
  padding: 7px 13px;
  height: 38px;
  background: #0D6EFD;
  font-family: 'Roboto Slab';
  margin-left: auto;
}

button:disabled {
  background: darkgrey;
}

.textInput {
  background: #FFFFFF;
  width: 56px;
  height: 38px;
  border: 1px solid #CED4DA;
}

@media (max-width: 768px) {
  .container {
    padding: 0px 16px;
    width: auto;
  }

  .header {
    padding-top: 52px;
  }

  .form {
    flex-direction: column;
    height: max-content;
    padding: 12px;
  }

  .form-container {
    flex-wrap: wrap;
    height: fit-content;
    padding-right: 30px;
  }

  .textInput {
    margin-bottom: 10px;
  }

  .button {
    width: 100%;
  }
}
</style>
