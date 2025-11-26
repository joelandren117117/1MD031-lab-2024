<template>
  <div class="burger">
          <h3>{{ burger.name }}</h3>
          <img
            :src="burger.url"
            width="250"
          />
          <ul>
            <li>{{ burger.kCal }} kCal</li>

      <li>
        <span v-if="burger.lactose">
          Contains <span class="allergen">lactose</span>
        </span>
        <span v-else>
          Lactose free
        </span>
      </li>

      <li>
        <span v-if="burger.gluten">
          Contains <span class="allergen">gluten</span>
        </span>
        <span v-else>
          Gluten free
        </span>
      </li>
          </ul>
          <p>Amount ordered: {{ amountOrdered }}</p>
    <button v-on:click="removeBurger">-</button>
    <button v-on:click="addBurger">+</button>
        </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
  return {
    amountOrdered: 0,
   }
  },
  methods: {
    addBurger() {
      this.amountOrdered += 1;
      this.$emit("orderedBurger", {
        name: this.burger.name,
        amount: this.amountOrdered,
      });
    },
    removeBurger() {
      if (this.amountOrdered > 0) {
        this.amountOrdered -= 1;
        this.$emit("orderedBurger", {
          name: this.burger.name,
          amount: this.amountOrdered,
        });
      }
    },
  }
}
</script>

<style scoped>
.allergen {
  font-weight: bold;
  color: red;
  text-transform: uppercase;
}
.burger {
  padding: 0.5em;
  margin: 0.25em;
}
.burger ul {
  font-size: 0.85rem;   
  line-height: 1.3;     
  padding-left: 1.1rem; 
}
.burger img {
    max-width: 100%;   
    height: auto;
    display: block;
    margin: 0 auto;    
}
</style>