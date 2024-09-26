<script setup>
import { ref } from 'vue';
import Skeleton from './SkeletonView.vue'

const category = ref('')
const image = ref('')
const price = ref('')
const rating = ref({ rate: 0, count: 0 })
const title = ref('')
const setId = ref(1)
const description = ref('')
const isLoading = ref(true)

// function increment id 
const increment = () => {
   isLoading.value = true
   if (setId.value >= 20) {
      setId.value = 0
   }
   setId.value += 1
   getProduct()
}

// fetch product by id product 
const getProduct = () => {
   fetch(`https://fakestoreapi.com/products/${setId.value}`)
      .then(res => res.json())
      .then(json => {
         category.value = json.category
         image.value = json.image
         price.value = json.price
         rating.value = json.rating
         title.value = json.title
         description.value = json.description
         isLoading.value = false
      })
      .catch(err => {
         isLoading.value = false
         console.log(err)
      })
}

getProduct();
</script>

<template>
   <!-- section men's clothing  -->
   <section id="Page-Men" v-if="category === `men's clothing`">
      <div class="container">
         <div class="bg-color-men">
            <Skeleton v-if="isLoading" />
            <div v-else>
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
   <section id="Page-women" v-else-if="category === `women's clothing`">
      <div class="container">
         <div class="bg-color-women">
            <Skeleton v-if="isLoading" />
            <div v-else>
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
   <section id="Page-Unavailable" v-else>
      <div class="container">
         <div class="bg-color-unavailable">
            <Skeleton v-if="isLoading" />
            <div v-else>
               <div class="unavailable">
                  <div class="sad-face"></div>
                  <div class="mouth"></div>
                  <p>This product is unavailable to show.</p>
                  <button class="btn-next-unavailable" @click="increment">Next product</button>
               </div>
            </div>
         </div>
      </div>
   </section>
</template>