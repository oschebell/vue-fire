<template>
  <div class="index container">
    <div class="card" v-for="smoothie in smoothies" :key="smoothie.id">
      <div class="card-content">
        <i class="material-icons delete" @click="deleteSmoothie(smoothie.id)">delete</i>
        <h2 class="indigo-text">{{ smoothie.title }}</h2>
        <ul class="ingredients">
          <li v-for="(ing, index) in smoothie.ingredients" :key="index">
            <span class="chip">{{ ing }}</span>
          </li>
        </ul>
          <a :href="smoothie.image" download="smoothie.image"><img :src="smoothie.image"  style="height:48px"  alt="" ></a>
       
      </div>
    </div>
  </div>
</template>

<script>
import db from '@/firebase/init' //import firebase database

export default {
  name: 'Index',
  data(){
    return{
      smoothies: []
    }
  },
  methods: {
    deleteSmoothie(id){
      this.smoothies = this.smoothies.filter(smoothie => {
        return smoothie.id != id
      })
    }
  }, 
  created(){
    
    //fetch data from firestore
    db.collection('Smoothies').get()
    .then(snapshot => {
      snapshot.forEach(doc => {
        this.smoothies.push(doc.data())
      })
    })
    .catch(err => {
      console.log(err)
    })
  }
}
</script>

<style>
.index{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 30px;
  margin-top: 60px;
}
.index h2{
  font-size: 1.8em;
  text-align: center;
  margin-top: 0;
}
.index .ingredients{
  margin: 30px auto;
}
.index .ingredients li{
  display: inline-block;
}
.index .delete{
  position: absolute;
  top: 4px;
  right: 4px;
  cursor: pointer;
  color: #aaa;
  font-size: 1.4em;
}
</style>
