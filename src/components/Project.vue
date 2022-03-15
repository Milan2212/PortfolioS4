<template>
<div>
    <section class="main projet">
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
                <router-link to="/">À propos de moi</router-link>
                <router-link to="/">Projets</router-link>
                <a href="mailto:milanmorisot@gmail.com">Contact</a>
                </div>
            </div>
        </header>
        <img :src="projet.acf.image_projet" alt="">
        <h2>{{projet.acf.nom}}</h2>
        <h4><span v-for="tag in projet.acf.tags" :key="tag.id"> {{tag.post_title}} </span></h4>
        <p>{{projet.acf.description}}</p>
        <a :href="projet.acf.lien_projet">→ Lien du projet</a>
        
    </section>
    <footer>
        <div class="footer-table">
            <div class="name">
                <p>Jonas Morisot</p>
            </div>
            <div class="nav">
                <p><a href="index.html#home">ACCUEIL</a></p>
                <p><a href="index.html#apropos">À PROPOS DE MOI</a></p>
                <p><a href="index.html#projets">PROJETS</a></p>
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
  name: 'Project',
  data () {
    return {
        liste: [],
        projet: []
    }
  },
  created(){
    this.id = this.$route.params.id;
    axios.get(param.host+"projet")
    .then(response => {
      this.liste = response.data;
      for (let i = 0; i < this.liste.length; i++) {
          if (this.liste[i].id == this.id){
              this.projet = this.liste[i]
          }     
      }
    })
  }
}
</script>

<style scoped>
</style>