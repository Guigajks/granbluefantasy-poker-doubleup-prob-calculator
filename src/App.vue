<template>
  <div id="test">
    <template v-for="suit in suits">
      <img
        v-for="number in numbers"
        :key="`${number}${suit}`"
        :src="getImageSrc(`${number}${suit}`)"
        width="75"
        height="100"
        loading="lazy"
        :class="{
          selected: cards[`${number}${suit}`] 
        }"
        @click="selectAndSetAsCurrent(`${number}${suit}`)"
      />
    </template>

    <div>
      <div>
        <button type="button" @click="resetSelection">
          Reset
        </button>
      </div>
      <!-- <img width="250" :src="previousCard ? getImageSrc(previousCard) : ''" alt="Nenhuma imagem selecionada"> -->
      <img width="250" :src="currentCard ? getImageSrc(currentCard) : ''" alt="Nenhuma imagem selecionada">


    </div>


  </div>
</template>

<script>
export default {
  name: 'App',

  data() {
    return {
      suits: ['H', 'D', 'S', 'C'],
      numbers: [
        '2',
        '3',
        '4',
        '5',
        '6',
        '7',
        '8',
        '9',
        '10',
        'J',
        'Q',
        'K',
        'A',
      ],
      cards: {},
      currentCard: '',
      previousCard: '',
      cardsDir: './assets/cards',
    };
  },

  computed: {
    currentCardValue() {
      return this.getCardValue(this.currentCard);
    },

    notSelectedCards() {
      return Object.entries(this.cards).filter(([, value]) => !value);
    },

    notSelectedGreaterThanCurrent() {
      if (!this.currentCard) return [];
      return this.notSelectedCards.filter(([key]) => this.getCardValue(key) > this.currentCardValue);
    },

    notSelectedLowerThanCurrent() {
      if (!this.currentCard) return [];
      return this.notSelectedCards.filter(([key]) => this.getCardValue(key) < this.currentCardValue);
    },

    notSelectedEqualsToCurrent() {
      if (!this.currentCard) return [];
      return this.notSelectedCards.filter(([key]) => this.getCardValue(key) == this.currentCardValue);
    },

    greaterThanCurrent() {
      return (this.notSelectedGreaterThanCurrent.length / this.notSelectedCards.length) * 100;
    },

    lowerThanCurrent() {
      return (this.notSelectedLowerThanCurrent.length / this.notSelectedCards.length) * 100;
    },

    equalsToCurrent() {
      return (this.notSelectedEqualsToCurrent.length / this.notSelectedCards.length) * 100;
    },
  },

  async created() {
    for (const number of this.numbers) {
      for (const suit of this.suits) {
        this.cards[`${number}${suit}`] = false;
      }
    }
  },

  methods: {
    getCardValue(cardString) { 
      return this.numbers.indexOf(cardString.slice(0, -1)) + 2;
    },

    selectImage(imageKey) {
      this.cards[imageKey] = true; 
    },

    setImageAsCurrent(imageKey) {
      if (!this.cards[imageKey]) {
        this.previousCard = this.currentCard;
        this.currentCard = imageKey;
      }
    },

    selectAndSetAsCurrent(imageKey) {
      this.setImageAsCurrent(imageKey);
      this.selectImage(imageKey);
    },

    getImageSrc(imageKey) {
      return imageKey ? require(`@/assets/cards/${imageKey}.jpg`): ''; 
    },

    resetSelection() {
      Object.entries(this.cards).forEach(([ key ]) => {
        this.cards[key] = false;
      })

      this.currentCard = '';
      this.previousCard = '';
    }
  },
};
</script>

<style scoped>
img.selected {
   filter: grayscale(100%) invert(1);
}
</style>
