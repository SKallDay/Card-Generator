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
      } else {
        alert('please enter number belows 5 for cards and columns ')
      }
      const totalLetters = cards * cols
      const randomLetters = this.generateString(totalLetters, cols)
      this.randomLetters = randomLetters
    },
    generateString(numOfLetters, cols) {
      const characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("");
      const randomLettersArray = [...characters]
        .sort(() => (Math.random() > 0.5 ? 1 : -1))
        .slice(0, numOfLetters);
      return this.splitIntoRows(randomLettersArray, cols)
    },
    splitIntoRows(arr, len) {
      var chunks = []
      for (let i = 0; i < arr.length; i += len) {
        const chunk = arr.slice(i, i + len);
        chunks.push(chunk)
      }
      return chunks;
    },
    handleInput(e) {
      const inputName = e.target.name
      const value = parseInt(e.target.value)

      inputName === 'cards' ? this.cards = value : this.cols = value
    }
  }
};
</script>

<template>
  <div class="container">
    <h1 class="header">Card Generator</h1>
    <form class="form" v-on:submit="submit">
      <div class="form-container">
        <div class="input-container">
          <label>Generate</label>
          <input class="textInput" type="text" :v-model=this.cards name="cards" pattern="[+]?([0-4]*\.[0-9]+|[0-5])"
            required @input="handleInput" />
          <p mb10>random cards, &nbsp</p>
        </div>
        <div class="input-container">
          <p>each with </p>
          <input class="textInput" type="text" :v-model=this.cols name="cols" max="5"
            pattern="[+]?([0-4]*\.[0-9]+|[0-5])" required @input="handleInput" />
          <label>rows/columns.</label>
        </div>
      </div>
      <button class="button" :disabled=isDisabled>
        Generate
      </button>
    </form>
  </div>

  <div v-if="randomLetters.length" class="grid-container">
    <div class="grid-row" v-for="(row, index1) in randomLetters">
      <div v-for="(letter, index) in row">
        <Card :msg="letter" :row=index :col=index1 />
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
}

.grid-container {
  border: 12px solid black;
  display: flex;
  flex-direction: column;
  width: fit-content;
  margin: 0 auto;
  margin-top: 35px;
  border-radius: 4px;
}

.grid-row {
  display: flex;
  flex-direction: row;
  flex-grow: 3;
}

.item {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #FFFFFF;
  font-family: 'Roboto Slab';
  font-style: normal;
  font-weight: 900;
  font-size: 36px;
  line-height: 47px;
  border: 12px solid black;
}

.header {
  font-family: 'Roboto Slab';
  font-style: normal;
  font-weight: 700;
  font-size: 48px;
  line-height: 63px;
}

.form {
  height: 62px;
  background: #FFFFFF;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 4px;
  display: flex;
  align-items: center;
  padding: 0px 10px;
}

.form-container {
  display: flex;
  align-items: center;
}

.input-container {
  display: flex;
  align-items: center;
}

.button {
  padding: 7px 13px;
  height: 38px;
  background: #0D6EFD;
  border-radius: 4px;
  font-family: 'Roboto Slab';
  color: #FFFFFF;
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
  border-radius: 4px;
  margin: 0px 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
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

  .input-container {
    display: flex;
  }

  .textInput {
    margin-bottom: 10px;
  }

  .button {
    width: 100%;
  }

  .grid-row {
    display: flex;
    flex-direction: row;
    flex-grow: 3;
  }

  .card {
    width: 66px;
    height: 65px;
  }
}
</style>
