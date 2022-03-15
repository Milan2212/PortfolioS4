<template>
<div>
  <section class="main" id="home">
        <header>
            <div class="nav">
                <input type="checkbox" id="nav-check">
                <div class="nav-header">
                <div class="nav-title">
                    Jonas Morisot
                </div>
                </div>
                <div class="nav-btn">
                <label for="nav-check">
                    <span></span>
                    <span></span>
                    <span></span>
                </label>
                </div>
                
                <div class="nav-links">
                <router-link to="/">Accueil</router-link>
                <a href="#apropos">À propos de moi</a>
                <a href="#projets">Projets</a>
                <a href="mailto:milanmorisot@gmail.com">Contact</a>
                </div>
            </div>
        </header>
        <h2>{{texts[0].acf.titre}}</h2>
        <img src="../assets/img/front_image.png" alt="Front image">
    </section>
    <section class="pres" id="apropos">
        <img src="../assets/img/me.jpg" alt="">
        <div class="textprj">
            <p>{{texts[0].acf.texte1}}</p>
            <a href="">VOIR MES PROJETS →</a>
        </div>
        <div class="textcont">
            <p>{{texts[0].acf.texte2}}</p>
            <a href="">ME CONTACTER →</a>
        </div>
    </section>
    <section class="project" id="projets">
        <h2>De nombreux projets divers et variés</h2>
        <label for="type_projet">Type de projet :</label>
        <select name ="type_projet" @change="choixType(choixtype)" v-model="choixtype" >
            <option value="" selected >Tous</option>
            <option :value="type.acf.nom" v-for="type in typeProjet" :key="type.id">{{type.acf.nom}}</option>
        </select>
        <div class="cardlist">
            <div class="card" v-for="projet in filterList.slice(0,x)" :key="projet.id">
                <img :src="projet.acf.image_projet" :alt="projet.acf.nom">
                <h3>{{projet.acf.nom}}</h3>
                <h4><span v-for="tag in projet.acf.tags" :key="tag.id"> {{tag.post_title}} </span></h4>
                <p>{{projet.acf.description | liveSubstr(194)}}</p>
                <router-link :to="{ name: 'Project', params: {id : projet.id}}">→ Voir le projet</router-link>
            </div>
        </div>
        <h3 class="plusbutton" v-on:click="addProjet" v-bind:style="{ display : computedDisplay }" >Voir plus de projets</h3>
    </section>
    <footer>
      <div class="footer-table">
          <div class="name">
              <p>Jonas Morisot</p>
          </div>
          <div class="nav">
              <p><a href="#home">ACCUEIL</a></p>
              <p><a href="#apropos">À PROPOS DE MOI</a></p>
              <p><a href="#projets">PROJETS</a></p>
          </div>
          <div class="reseaux">
              <a href="https://twitter.com/Milan__J" target=”_blank”><img src="../assets/img/twitter.svg" alt=""></a>
              <a href="https://www.linkedin.com/in/jonas-morisot/" target=”_blank”><img src="../assets/img/linkedin.svg" alt=""></a>
              <a href="https://lol.fandom.com/wiki/Self:Milan_(Manager)" target=”_blank”><img src="../assets/img/leaguepedia.svg" alt=""></a>
              <a href="mailto:milanmorisot@gmail.com" target=”_blank”><img src="../assets/img/mail.svg" alt=""></a>
          </div>
      </div>
  </footer>
    
</div>
</template>

<script>

import param from '@/param/param'


export default {
  name: 'Main',
  data () {
    return {
      liste:[],
      texts: [],
      typeProjet:[],
      x:3,
      y:0,
      display: "block",
      filterList: [],
      choixtype: "",
    }
  },
  created(){
    axios.get(param.host+"projet")
    .then(response => {
      this.liste = response.data;
      this.y = this.liste.length;
      this.filterList = this.liste
    }),
    axios.get(param.host+"type_projet")
    .then(response => {
      this.typeProjet = response.data;
    }),
    axios.get(param.host+"homepage")
    .then(response => {
      this.texts = response.data;
    })
  },
  computed: {
      computedDisplay: function(){
          return this.display;
      },
  },
  methods: {
      addProjet: function(){
          this.x = this.x+3
          if (this.x >= this.y) {
            this.display="none";
          }
      },
        choixType: function(){
            var type = this.choixtype;
            this.filterList = []
            var x = 0
            if (type == "") {
                this.filterList = this.liste
            }
            else {
            for (let i = 0; i < this.liste.length; i++) {
                for (let b = 0; b < this.liste[i].acf.tags.length; b++) {
                    if (this.liste[i].acf.tags[b].post_title == type) {
                        this.filterList[x] = this.liste[i]
                        x = x+1
                    }
                }
                
            }
            }
      }
  },
  filters: {
      liveSubstr: function(string, nb){
          return string.substring(0,nb) + '...';
      },
  }
}
</script>

<style scoped>
</style>
