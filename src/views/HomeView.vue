<template>
    <div class="sectionone">
        <header id="headline">MZ Luxury Cars</header>
        <img src="Taycan.jpeg" id="mainbg" alt="Porsche 911" title="Porsche 911">
    </div>
        
<main>

        <section class="sectiontwo">
            <h2>
                <h2 class="center-text">Welcome to MZ Luxury Cars</h2>
            </h2>
            <p>
                <p class="center-text">Please choose your automobile</p>
            </p>
        </section>
            <div class="wrapper">

                <Burger v-for="burger in burgers" :key="burger.name" :burger="burger" />
                <div class="wrapper">
                <Burger
                v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>
                </div>
            </div>

    <section class="receipt">
      <h1>Selected order: </h1>
      <p v-for="(amount, name) in orderedBurgers" :key="name">
        {{name}}: {{amount}}</p>


      <h3>Amount: {{calculateAmount()}}</h3>
    </section>

        <section class="sectionthree" id="sectionthree">
            <h2>Delivery Information</h2>
            <p>Delivery instructions</p>
            
                <p>
                    <label for="fullname">Full name</label><br>
                    <input type="text" id="fullname" v-model="fullname" required minlength="1"
                        placeholder="Full name"/>
                </p>

                <p> <label for="email">E-mail</label><br />
                    <input type="email" id="email" v-model="email" required minlength="1"
                        placeholder="E-mail"/>
                </p>

                <p>
                    <label for="streetname">Street</label><br />
                    <input type="text" id="streetname" v-model="streetname" required minlength="1"
                        placeholder="Street"/>
                </p>

                <p>
                    <label for="housenumber">Housenumber</label><br />
                    <input type="number" id="housenumber" v-model="housenumber" required minlength="1"
                        placeholder="Housenumber"/>
                </p>


                <p>
                    <label for="payment">Payment method</label>
                    <select id="payment" v-model="selected">
                        <option selected="Swish">Swish</option>
                        <option>Kontokort</option>
                        <option>Faktura</option>
                        <option>PayPal</option>
                    </select>
                </p>

            <form>

                <h3>Gender</h3>
                <input type="radio" id="male" value = "man" v-model="gender">
                <label for="male">Male</label><br>

                <input type="radio" id="female" value = "woman" v-model="gender">
                <label for="female">Female</label><br>

                <input type="radio" id="undefined" value = "undefined" v-model="gender" checked>
                <label for="undefined">Prefer not to answer</label><br>
            </form>

            <div>

        <h3>Please select your location:</h3>
        <section class="scroll">
          <div
            id="polacksbacken"
            v-on:click="setLocation"
            v-bind:style="{
              background:
                'url(' + require('../../public/img/polacks.jpg') + ')',
            }"
          >
            <div
              v-bind:style="{
                left: this.location.x + 'px',
                top: this.location.y + 'px',
              }"
            >
              T
            </div>
          </div>
        </section>
    </div>
      
       

    <button v-on:click="submit">
        <img src="check.png" width="21" height="21">
        Send Info
    </button>
</section>
    <section class="sectionfour">
      <h1>About:</h1>
      <p>
       "This page needs serious help"
      </p>
    </section>

</main>
<footer>&copy; 2023 MZ Luxury Cars Limited</footer>
</template>

<script>
import menu from "/Users/sebastian.mendez/Desktop/Desktop/Github/1md031-lab-2023/src/assets/menu.json";
import Burger from '/Users/sebastian.mendez/Desktop/Desktop/Github/1md031-lab-2023/src/views/DispatcherView.vue'
import io from 'socket.io-client'
const socket = io();



//Object Constructor Function

/* function MenuItem(name, price, category, electric, url) {
    this.name = name;
    this.price = price;
    this.category = category;
    this.electric = electric;
    this.url = url;
}
*/

/* Create an array of cars using the MenuItem constructor
const burgers = [
    new MenuItem('Ferrari 458 Italia', 828, 'Supercar', 'false', 'ferre.png'),
    new MenuItem('Porsche GT4RS', 639, 'Daily-driver', 'false', 'poss.png'),
    new MenuItem('Koenigsegg Stradale', 9999, 'Eco-friendly', 'true', 'koenigsegg.png'),
   
];
console.log(burgers); */


export default {
  name: "HomeView",
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: menu, gender: "undisclosed", orderedBurgers: {}, clientDetails: [],
      selected: "Card", queNumber: 0,
      location: { x: 0, y: 0 },
    };
  },

methods: {
    getOrderNumber: function () {
      return (this.queNumber += 1);
    },
    formInformation: function () {
      this.clientDetails = [
        this.fullname,
        this.email,
        this.gender,
        this.selected,
    ];
      console.log(this.clientDetails);
      /* Log information about each car in the array
burgers.forEach(burger => {
    console.log(burger);
    
}); */
      return this.clientDetails;
    },
addToOrder: function (event) {
    if (event.amount===0){
      delete this.orderedBurgers[event.name] }
    else{
      this.orderedBurgers[event.name] = event.amount;} 
    }, //chatgpt

    calculateAmount: function () {
      let sum = 0;
      for (let amount of Object.values(this.orderedBurgers)) {
        //chatgpt
        sum += amount;
      }
      return sum;
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
      }; //chatgpt
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y,
        },
        orderItems: [this.fullname, this.orderedBurgers],
      });
      (this.location.x = event.clientX - 10 - offset.x),
        (this.location.y = event.clientY - 10 - offset.y);
    },

    setLocation: function () {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y,
      };
    },

    submit: function () {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: { x: this.location.x, y: this.location.y },
        orderItems: this.orderedBurgers,
        givenInfo: this.formInformation(),
      });
    },
    // <p v-for="(amount, name) in orderedBurgers" :key="name">{{name}}:{{amount}}
  },
};
</script>



<style>

  body {
    font-family: Arial, Helvetica, sans-serif; 
    font-size: 1em;
 }

header {

    text-align: center;
    color: #000000;
    font-size: 5em;
    top: 5%;

}

.sectionthree,
.sectionone,
.receipt {
  background-color: #ADD8E6;
}



 #sectionone {
    margin: 1em 1em; /* top/bottom, R/L */
    height: 30em;
    overflow: hidden;
}

#sectiontwo {
    background-color: black;
    color: white;
    margin: 2em 2em; /* top/bottom, R/L */
    border: 2px dashed #557ef8;
    border-radius: 2em;
}

#sectionthree {
    margin: 2em 2em; /* top/bottom, R/L */
    padding: 2em;
    border: 2px dashed #557ef8;
    border-radius: 2em;
}
.buttonone {
    margin: 1em;   /* All sides */
}

button:hover { background-color: rgb(230, 230, 230);
    cursor: pointer;
}

#mainbg {
    opacity: 0.6;
    width: 100%;
    height: auto;
}

#headline {
    position: absolute;
    padding: 4em 26.5em;
    margin-top: 0em;
}

.wrapper {
  display: grid;
  grid-gap: 1em;
  grid-template-columns: repeat(3, 1fr);
  justify-content: space-evenly;
}

.wrapper img {
  width: 100%;
  height: auto;
  max-width: 100%;
  max-height: 100%;
}

@media screen and (max-width: 800px) {
    h1 {
        font-size: 6vw;
    }
}

.scroll {
    height: 500px; width: 80%; overflow: scroll;  
  
}

#polacksbacken {

  position: relative;
  
  width: 1920px;
  height: 1078px;
  margin: 0;
  padding: 0;
  cursor: crosshair;
}

#polacksbacken div {
  position: absolute;
text-align: center;

 
}

button:hover {
cursor:grab;
}

</style>

