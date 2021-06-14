<template>
  <div id="test">
    <!-- <template v-for="suit in suits"> -->
    <!-- <img
        v-for="number in numbers"
        :key="`${number}${suit}`"
        :src="`${require(`@/assets/cards/${number}${suit}.jpg`)}`"
        width="75"
        height="100"
        loading="lazy"
      /> -->
    <!-- </template> -->
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
      cardsDir: './assets/cards',
    };
  },

  async created() {
    for (const number of this.numbers) {
      for (const suit of this.suits) {
        this.cards[`${number}${suit}`] = false;
      }
    }

    console.log(this.cards);
  },

  methods: {},
};
</script>

<style></style>
