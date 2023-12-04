<template>

     <div class="box a">
     
    <img v-bind:src="car.img" :title="car.name" :alt="car.name" />
    <h1 class="name"> {{car.name}} </h1>
    <div class="Specifications">
      <ul>
        <li>Horsepower: {{car.horsepower}}</li>
        <li>{{car.price}}</li>
        <li>{{car.category}}</li>
        <li>{{is_Electric(car.electric) }}</li>
      </ul>
    </div>
    <section class="Buttons">
      <button v-on:click="increase">Add to basket</button>
      <button v-on:click="decrease">Remove from basket</button>
      <p class="amount" v-if="amountOrdered">Amount: {{amountOrdered}} </p>
    </section>
    </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      car: Object
    },
    data() {
      return {
        amountOrdered: 0,
      };
    },
    methods: {
      
      is_Electric(electric) {
        if (electric) return "Electric";
        else return "Combustion";
    },

   
      increase() {
        this.amountOrdered++;
        this.$emit("orderedCar", {
          name: this.car.name,
          amount: this.amountOrdered,
        });
      },
      decrease() {
        if (this.amountOrdered > 0) {
          this.amountOrdered--;
          this.$emit("orderedCar", {
            name: this.car.name,
            amount: this.amountOrdered,
          });
        }
      },
    },
  };
  </script>
  
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  body {
    font-family: Arial, Helvetica, sans-serif; 
    font-size: 1em;
 }

 .obs {
    font-weight: bold;
    
 }

#sectiontwo {
    background-color: black;
    color: white;
    margin: 2em 2em; /* top/bottom, R/L */
    border: 2px dashed #557ef8;
    border-radius: 2em;
}

.wrapper {
    display: grid;
    grid-gap: 1em;
    grid-template-columns: 33% 33% 33%;
}

@media screen and (max-width: 800px) {
    h1 {
        font-size: 6vw;
    }
}
</style>