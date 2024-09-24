<script setup>
import { ref } from 'vue';

const category = ref('')
const image = ref('')
const price = ref('')
const rating = ref({ rate: 0, count: 0 })
const title = ref('')
const setId = ref(1)
const description = ref('')

// function increment id 
const increment = () => {
   if (setId.value >= 20) {
      setId.value = -1 + 1
   }
   setId.value += 1
   fetchData()
}

// fetch product by id product 
const fetchData = () => {
   fetch(`https://fakestoreapi.com/products/${setId.value}`)
      .then(res => res.json())
      .then(json => {
         category.value = json.category
         image.value = json.image
         price.value = json.price
         rating.value = json.rating
         title.value = json.title
         description.value = json.description
      })
}
fetchData();

const categories = ["men's clothing", "women's clothing"];
</script>

<template>
   <!-- section men's clothing  -->
   <section id="Page-Men" v-if="category === categories[0]">
      <div class="container">
         <div class="bg-color-men">
            <div>
               <div class="image-product">
                  <img class="photo-product" :src='image' :alt="title">
               </div>
               <div class="product">
                  <h2 class="title-mens">{{ title }}</h2>
                  <div class="sub-title">
                     <p>{{ category }}</p>
                     <div class="rating">
                        <p style="margin-right: 2px;">{{ rating.rate }}/5</p>
                        <div class="stars-mens" v-for="i in Math.round(rating.rate)" :key="i"></div>
                        <div class="stars-outline-mens" v-for="i in 5 - Math.round(rating.rate)" :key="i"></div>
                     </div>
                  </div>
                  <p class="description">{{ description }}</p>
                  <p class="price-mens">${{ price }}</p>
                  <div class="buttons-group">
                     <button class="btn-buy-mens">Buy now</button>
                     <button class="btn-next-mens" @click="increment">Next product</button>
                  </div>
               </div>
            </div>
         </div>
      </div>
   </section>

   <!-- section women's clothing  -->
   <section id="Page-women" v-if="category === categories[1]">
      <div class="container">
         <div class="bg-color-women">
            <div>
               <div class="image-product">
                  <img class="photo-product" :src='image' :alt="title">
               </div>
               <div class="product">
                  <h2 class="title-womens">{{ title }}</h2>
                  <div class="sub-title">
                     <p>{{ category }}</p>
                     <div class="rating">
                        <p style="margin-right: 2px;">{{ rating.rate }}/5</p>
                        <div class="stars-womens" v-for="i in Math.round(rating.rate)" :key="i"></div>
                        <div class="stars-outline-womens" v-for="i in 5 - Math.round(rating.rate)" :key="i"></div>
                     </div>
                  </div>
                  <p class="description">{{ description }}</p>
                  <h3 class="price-womens">${{ price }}</h3>
                  <div class="buttons-group">
                     <button class="btn-buy-womens">Buy now</button>
                     <button class="btn-next-womens" @click="increment">Next product</button>
                  </div>
               </div>
            </div>
         </div>
      </div>
   </section>

   <!-- section unavailable  -->
   <section id="Page-Unavailable" v-if="!categories.includes(category)">
      <div class="container">
         <div class="bg-color-unavailable">
            <div>
               <div class="unavailable">
                  <div class="sad-face"></div>
                  <div class="mouth"></div>
                  <p>This product is unavailable to show</p>
                  <button class="btn-next-unavailable" @click="increment">Next product</button>
               </div>
            </div>
         </div>
      </div>
   </section>
</template>