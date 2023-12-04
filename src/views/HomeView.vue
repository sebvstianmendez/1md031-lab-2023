//Enhanced HTML -------------------------------------------------------------------------------------------------------------------------------------

<template>
    <div class="sectionone" id="sectionone">
        <header id="header"><h1 class="center-text">MZ Luxury Cars</h1></header>
        <img src="Taycan.jpeg" id="mainbg" alt="Porsche 911" title="Porsche 911">
    </div>
        
<main>
  
        <section class="sectiontwo" id="sectiontwo">
                <h2>Welcome to MZ Luxury Cars</h2><br>
                <p>Please choose your automobile</p>
      
        </section>
            <div class="wrapper">
            <Burger
                v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedCar="addToOrder($event)"/>
                
            </div>

        <section class="receipt">
          <div>
      <h3>Amount:</h3>
   
  </div>
    <p v-for="(amount, name) in orderedCars" :key="name">
      {{ name }}: {{ amount }}
    </p>

    </section>

        <section class="sectionthree" id="sectionthree">
            <h2>Delivery Information</h2>
            <p>Delivery instructions</p>
            <p>
                <label for="fullname">Full name</label><br>
                    <input type="text" id="fullname" v-model="fullname" required="required" minlength="1"
                        placeholder="Full name"/>
                </p>

                <p> <label for="email">E-mail</label><br>
                    <input type="email" id="email" v-model="email" required minlength="1"
                        placeholder="E-mail"/>
                </p>
                <!-- 
                <p>
                    <label for="Streetname">Street</label><br>
                    <input type="text" id="streetname" v-model="streetname"
                    required minlength="1"
                        placeholder="Street"/>
                </p>

                <p> 
                    <label for="Housenumber">Housenumber</label><br>
                    <input type="number" id="housenumber" v-model="hnr" required minlength="1"
                        placeholder="Streetnumber"/>
                </p>
                -->

                <p>
                    <label for="payment">Payment method</label>
                    <select id="payment" v-model="paymenthmethod">
                        <option checked >Swish</option>
                        <option>Kontokort</option>
                        <option>Faktura</option>
                        <option>PayPal</option>
                    </select>
                </p>

            <form>

                <h3>Gender</h3>
                <input type="radio" id="male" value = "man" v-model="gender">
                <label for="male">Male</label><br>

                <input type="radio" id="female" value = "female" v-model="gender">
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
                'url(' + require('/Users/sebastian.mendez/Desktop/Desktop/Github/1md031-lab-2023/public/img/polacks.jpg') + ')',
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

    <section class="sectionfour" id="sectionfour">
      <h1>About:</h1>
      <p>
       "MZ Luxury Cars 2023 Ltd."
      </p>
    </section>

</main>
<!-- Horisontal line -->
<hr> 
<footer>&copy; 2023 MZ Luxury Cars Limited</footer>
</template>

<!-- JavaScript -------------------------------------------------------------------------------------------------------------------------------------
-->
<script>
import menu from "/Users/sebastian.mendez/Desktop/Desktop/Github/1md031-lab-2023/src/assets/menu.json";
import Burger from '/Users/sebastian.mendez/Desktop/Desktop/Github/1md031-lab-2023/src/components/OneBurger.vue'
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
      burgers: menu, gender: "Undisclosed", orderedCars: {}, clientDetails: [],
      selected: "Card", queNumber: 0,
      location: { x: 0, y: 0 },
    };
  },

  computed: {
    totalAmount: function () {
      let sum = 0;
      for (let amount of Object.values(this.orderedCars)) {
        sum += amount;
      }
      return sum;
    },
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
      delete this.orderedCars[event.name] }
    else{
      this.orderedCars[event.name] = event.amount;} 
    }, //chatgpt

    calculateAmount: function () {
      let sum = 0;
      for (let amount of Object.values(this.orderedCars)) {
        //chatgpt
        sum += amount;
      }

      console.log(this.calculateAmount)
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
        orderItems: [this.fullname, this.orderedCars],
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
        orderItems: this.orderedCars,
        givenInfo: this.formInformation(),
      });
    },
    // <p v-for="(amount, name) in orderedCars" :key="name">{{name}}:{{amount}}
  },
  
};




</script>



<!-- CSS -----------------------------------------------------------------------------------------------------------------------------------------------
-->
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
    border: 2px dashed #557ef8;
    border-radius: 2em;
}

#sectiontwo {
    background-image: url("https://img.freepik.com/free-vector/blue-abstract-background_1393-339.jpg");
    background-repeat: no-repeat; 
    background-size: cover;      
    color: white;
    margin: 1em 1em; /* top/bottom, R/L */
    border-radius: 2em;
    text-align: center;
    overflow: hidden;
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
    opacity: 0.9;
    width: 100%;
    height: auto;
}

#header {
  text-align: center;
    position: absolute;
    margin: 0 auto;
    z-index: 1;
    left: 0; right: 0; top: 50%; transform: translateY(-130%);
    color: white;
}


#headline h1{

  text-align: center;


}

.wrapper {
  display: grid;
  grid-gap: 1em;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  justify-content: space-evenly;
  max-height: 900px;

  
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
background: black;
    color: #ADD8E6;
    border-radius: 10px;
    width:20px;
    height:20px;

 
}

button:hover {
cursor:grab;
}

</style>

