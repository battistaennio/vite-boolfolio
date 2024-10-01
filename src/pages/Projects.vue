<script>
import { store } from '@/store/store';
import axios from 'axios';

export default {

    data () {
        return {
            projects: [],
            loading: true,
        }
    },

    methods : {

        getApi(){
            axios.get(store.apiUrl + 'projects')
            .then(response => {

                this.projects = response.data.projects
                console.log(this.projects);
                this.loading = false
            })
            .catch(error => {

                console.log(error);
            })
        },
    },

    mounted(){
        this.getApi()
    }
}
</script>

<template>
  <h1>Progetti</h1>

  <div id="prj-container">
    <h3>Lista dei progetti:</h3>

    <div v-if="loading == false">
        <ul>
            <li v-for="(prj, i) in projects" :key="i">
                <strong>{{ prj.id }}</strong>{{ prj.name }}
            </li>
        </ul>
    </div>

    <div v-else id="loader-container">
        <span class="loader"></span>
    </div>

  </div>
</template>

<style lang="scss" scoped>
    #prj-container{
        margin: 10px auto;
        width: 50%;
        min-height: 250px;
        border: 2px solid black;
        border-radius: 10px;
        padding: 10px;

        ul{
            margin: 10px 20px;

            li{
                list-style: none;
                margin: 10px 0;
                border-bottom: 1px solid #151515;

                &:last-child{
                    border-bottom: none;
                }

                strong{
                    margin-right: 10px;
                }
            }
        }
    }

    #loader-container{
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 250px;

    }

    .loader {
        width: 48px;
        height: 48px;
        border-radius: 50%;
        display: inline-block;
        border-top: 3px solid #000;
        border-right: 3px solid transparent;
        box-sizing: border-box;
        animation: rotation 1s linear infinite;
    }

        @keyframes rotation {
            0% {
                transform: rotate(0deg);
            }
            100% {
                transform: rotate(360deg);
            }
        } 
</style>