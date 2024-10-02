<script>
import { store } from '@/store/store';
import axios from 'axios';

export default {
    name: 'Projects',
    data () {
        return {
            loading: true,

            projects: [],
            technologies: [],
            types: [],

            paginator: [],
        }
    },

    methods : {

        getApi(url, type = 'projects'){
            this.loading = true;

            axios.get(url)
            .then(response => {

                switch(type){
                    case 'techs':
                        this.technologies = response.data.techs;
                    break

                    case 'types':
                        this.types = response.data.types;
                    break

                    case 'projects':
                        this.projects = response.data.projects;
                        this.paginator = response.data.projects.links;
                        this.loading = false;
                    break
                }

            })
            .catch(error => {

                console.log('Errore: ' + error.message);
            })
        },
    },

    mounted(){
        this.getApi(store.apiUrl + 'technologies',  'techs')
        this.getApi(store.apiUrl + 'types',  'types')
        this.getApi(store.apiUrl + 'projects', 'projects')
    }
}
</script>

<template>

    <div class="container">
        <h1>Progetti</h1>

            <div class="wrapper">
                <div id="is-loaded" v-if="loading == false">

                    <div id="prj-container">
                        <div id="list-container">
                            <h3>Lista dei progetti:</h3>

                            <div id="list">
                                <ul>
                                    <li v-for="(prj, i) in projects.data" :key="i">
                                        <strong>{{ prj.id }}</strong>
                                        <router-link class="link" :to="{ name: 'projectdetail', params: { slug: prj.slug} }">{{ prj.name }}</router-link>
                                    </li>
                                </ul>

                                <div id="paginator">
                                    <button
                                        v-for="link in paginator"
                                        v-html="link.label"
                                        :disabled="link.active || !link.url"
                                        :class="link.active ? 'active' : ' '"
                                        @click="getApi(link.url, 'projects')"
                                    >
                                    </button>
                                </div>
                            </div>
                        </div>

                    </div>

                    <div id="techs-types-container">

                        <h4>Tecnologie:</h4>
                        <div class="box">
                            <span v-for="tech in technologies">{{tech.name}}</span>
                        </div>

                        <h4>Tipi:</h4>
                        <div class="box">
                            <span v-for="type in types">{{type.name}}</span>
                        </div>

                    </div>

                </div>

                <div v-else id="loader-container">
                    <span class="loader"></span>
                </div>

            </div>

            




    </div>
</template>

<style lang="scss" scoped>

    #is-loaded{
        display: flex;
        justify-content: space-between;
        height: 100%;
    }

    #prj-container{

        width: 70%;

        #list-container{
            height: 100%;
            padding: 10px 20px;

            #list{
                height: 90%;
        
                ul{
                    height: 80%;
                    overflow-y: auto;


                    li{
                        list-style: none;
                        margin: 10px 0;
                        padding-bottom: 3px;
                        border-bottom: 1px solid #151515;

                        &:last-child{
                            border-bottom: none;
                        }

                        strong{
                            margin-right: 10px;
                        }
                    }
                }

                #paginator{
                    height: 20%;
                    display: flex;
                    justify-content: center;
                    align-items: flex-end;

                    button{
                        margin: 0 3px;
                        padding: 2px 7px;
                        border: 1px solid black;
                        border-radius: 5px;
                        background-color: black;
                        color: white;
                        font-size: 15px;
                        
                        &:hover{
                            cursor: pointer;
                            color: rgb(226, 194, 255);
                        }
                    }

                    .active{
                        color: rgb(158, 55, 255);
                        font-weight: 700;
                    }
                }

            }
        }
    }

    #techs-types-container{
        min-height: 200px;
        width: 30%;

        h4{
            text-align: center;
            margin-top: 20px;
        }

        .box{
            margin: 20px 0 60px 0;
            width: 100%;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;

            span{
                background-color: #000;
                color: white;
                padding: 3px 5px;
                border-radius: 5px;
                margin: 5px 10px;
            }
        }
    }


    #loader-container{

        margin: 0 auto;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;

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
    }
</style>