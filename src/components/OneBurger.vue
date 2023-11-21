<template>

     <div class="box a">
     
    <img v-bind:src="burger.url" :title="burger.name" :alt="burger.name" />
    <h1 class="name"> {{burger.name}} </h1>
    <div class="Specifications">
      <ul>
        <li>{{burger.price}}</li>
        <li>{{burger.category}}</li>
        <li>{{burger.electric}}</li>
      </ul>
    </div>
    <section class="Buttons">
      <button v-on:click="increase">+</button>
      <button v-on:click="decrease">-</button>
      <p class="amount" v-if="amountOrdered">Amount: {{amountOrdered}} </p>
    </section>
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
  }
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