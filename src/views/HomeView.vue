<template>
<header>
    
    <img src="https://s3.envato.com/files/f777f88d-7d92-453d-bc20-39c1a94089e5/inline_image_preview.jpg" alt="Interior of a burger restaurant" />
    <h1>Welcome to Burgerstore Online</h1>
  </header>
  <main>
    <section id="burgers">
      <h2>Select burger</h2>
      <div class="burger-grid">
        <Burger
        v-for="(burger, index) in burgers"
        :key="index"
        :burger="burger"
        v-on:orderedBurger="addToOrder"
      />
      </div>
    </section>

    <section id="contact">
      <h2>Customer information</h2>
      <form>
        <fieldset>
          <legend>Delivery information:</legend>

          <p>
            <label for="full-name">Full name</label><br />
            <input
              type="text"
              id="full-name"
              v-model="fullName"
              required="required"
              placeholder="First- and Last name"
            />
          </p>

          <p>
            <label for="email">E-mail</label><br />
            <input
              type="email"
              id="email"
              v-model="email"
              required="required"
              placeholder="E-mail address"
            />
          </p>
          <p>
            <label for="payment">Payment method</label><br />
            <select id="payment" v-model="payment">
              <option value="Credit card">Credit card</option>
              <option value="Swish">Swish</option>
              <option value="Cash">Cash</option>
              <option value="PayPal">PayPal</option>
            </select>
          </p>
          <label>Set location</label>
          <section id="map-container">
  <div id="map" v-on:click="setLocation">
    <div
          id="target-dot"
          v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }"
        >
          T
        </div>
</div>
</section>

        </fieldset>

        <fieldset>
          <legend>Gender</legend>

          <p>
            <label for="gender-male">
              <input
                type="radio"
                id="gender-male"
                name="gender"
                value="male"
                v-model="gender"
              />
              Male
            </label><br />

            <label for="gender-female">
              <input
                type="radio"
                id="gender-female"
                name="gender"
                value="female"
                v-model="gender"
              />
              Female
            </label><br />

            <label for="gender-none">
              <input
                type="radio"
                id="gender-none"
                name="gender"
                value="no-answer"
                v-model="gender"
              />
              Do not wish to provide
            </label>
          </p>
        </fieldset>
      </form>
    </section>
<button type="button" v-on:click="placeOrder">
      <img
        src="https://png.pngtree.com/png-clipart/20200225/original/pngtree-green-check-mark-icon-flat-style-png-image_5253210.jpg"
      />
      Place my order!
    </button>
  </main>

  <footer>
    <hr />
    <p>&copy; Joels Burgers Inc.</p>
  </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menuData from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name, imageUrl, kCal, gluten, lactose) {
  this.name = name;
  this.imageUrl = imageUrl;
  this.kCal = kCal;
  this.gluten = gluten;     
  this.lactose = lactose;   
}

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menuData,
      fullName: "",
      email: "",
      payment: "Credit card",
      gender: "male",
      orderedBurgers: {},
      location: { x: 0, y: 0 }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    setLocation: function (event) {
     var offset = {
       x: event.currentTarget.getBoundingClientRect().left,
       y: event.currentTarget.getBoundingClientRect().top
      };

     const x = event.clientX - 10 - offset.x;
     const y = event.clientY - 10 - offset.y;

     this.location.x = x;
     this.location.y = y;
    },
    
    addToOrder(event) {
    this.orderedBurgers[event.name] = event.amount;
    },
    placeOrder() {
    console.log("ORDER INFO:");
    console.log("Name:", this.fullName);
    console.log("Email:", this.email);
    console.log("Payment:", this.payment);
    console.log("Gender:", this.gender);
    console.log("Ordered burgers:", this.orderedBurgers);
    console.log("Delivery location (map coords):", this.location);

    socket.emit("addOrder", {
      orderId: this.getOrderNumber(),
      details: {
      x: this.location.x,
      y: this.location.y,
      name: this.fullName,
      email: this.email,
      gender: this.gender,
      payment: this.payment
    },
      orderItems: this.orderedBurgers
    });
  }
  }
}
</script>

<style>
#map-container {
  width: 400px;      
  height: 300px;
  overflow: scroll;  
  border: 2px dotted #1821cd; 
  margin: 2rem auto;
  padding: 0;
}

#map {
  width: 1920px;
  height: 1078px;
  background: url("/img/polacks.jpg");
  background-repeat: no-repeat;
  position: relative;
}
#target-dot {
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: red;
  color: white;
  text-align: center;
  line-height: 20px;
  font-weight: bold;
  pointer-events: none;     
}
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
body {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 12pt;
}

p {
    color: rgb(0, 0, 0);
}

h1 {
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 60pt;
}
main, header, footer, nav ul {
    max-width: 40rem;
    margin: 0 auto 0 auto;
}
main {
    background-color: rgb(255, 255, 255);
}

nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
}

header {
    position: relative;   
    height: 220px;        
    overflow: hidden;     
}
header img {
    width: 100%;
    height: auto;
    display: block; 
}


header h1 {
    position: absolute; 
    top: 50%;
    left: 50%;  
    transform: translate(-50%, -50%);         
    margin: 0;
    padding: 0.5rem 1rem; 
    color: rgb(255, 255, 255);      
    text-align: center;
    font-size: clamp(2rem, 6vw, 4rem);
}
section {
    margin: 2rem 1rem;  
    padding: 1rem;
    border: 2px dashed black;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: rgb(5, 170, 57);
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

.burger-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr); 
    gap: 0.5rem;
}
#burgers {
    background-color: rgb(221, 220, 200);
    color: rgb(0, 0, 0);
    border-color: rgb(20, 30, 218);
}
#contact {
    background-color: rgb(220, 204, 204);
    color: rgb(0, 0, 0);
    border-color: rgb(13, 0, 255);
}
button {
    background-color: rgb(5, 170, 57); 
    color: rgb(255, 255, 255);
    padding: 0.5em 1em;
    border: none;
    border-radius: 4px;
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
}

button img {
    width: 1.75rem;
    height: 1.75rem;
    object-fit: contain;
}
button:hover {
    background-color: rgb(0, 94, 30); 
    cursor: pointer;                   
}

</style>