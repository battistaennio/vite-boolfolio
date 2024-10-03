<script>
import Loader from '@/components/Loader.vue';
import { store } from '@/store/store';
import axios from 'axios';

export default {
  name: 'ProjectDetail',

  components: {
    Loader
  },

  data(){
    return{
        project: [],
        slug: this.$route.params.slug,
        loading: false
    }
  },

  methods: {

    convertDate(date) {
        const [year, month, day] = date.split("-");    
        return `${day}/${month}/${year}`;
    },

    getTechs(project){
        if(project.technologies.length){
           return project.technologies.map(tech => tech.name).join(', ')
            
        }
        return 'tecnologie non disponibili'

    },

    getApi(url){
        this.loading = false;

        axios.get(url)
        .then(response => {
            this.project = response.data.project
            this.project.start_date = this.convertDate(this.project.start_date)
            this.project.technologies = this.getTechs(this.project)
            this.loading = true;
        })
        .catch(error => {
            console.log('Errore: ' + error.message);
        })
    },


  },

  mounted(){
    this.getApi(store.apiUrl + `project/${this.slug}`)
  }
}
</script>

<template>

  <div class="container">
    <h1>Dettagli Progetto</h1>

    <div class="wrapper">
        <div  v-if="loading">
            <h2>Progetto: {{ project.name }}</h2>
            <p><strong>Descrizione: </strong>{{ project.description ? project.description : 'descrizione non disponibile' }}</p>
            <p><strong>Data di inizio: </strong>{{ project.start_date }}</p>
            <p><strong>Link repository: </strong><a :href="project.repo_link">{{ project.repo_link }}</a></p>
            <p><strong>Progetto di tipo: </strong>{{ project.type ? project.type.name : 'tipo non disponibile' }}</p>
            <p><strong>Tecnologia utilizzata: </strong>{{ project.technologies }}</p>

            <router-link class="link" :to="{name: 'projects'}"><i class="fa-solid fa-arrow-left-long"></i>
                Torna alla lista dei progetti</router-link>
        </div>

        <Loader v-else/>

    </div>



    

  </div>
</template>

<style lang="scss" scoped>

    .wrapper{
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 0 70px;

        h2{
            margin-bottom: 25px;
            text-align: center;
        }

        p{
            margin-bottom: 10px;
        }
    }

</style>