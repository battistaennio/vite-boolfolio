<script>
import { store } from '@/store/store';
import axios from 'axios';

export default {
    name: 'Projects',
    data () {
        return {
            projects: [],
            loading: true,

            paginator: [],
        }
    },

    methods : {

        getApi(url){
            this.loading = true;

            axios.get(url)
            .then(response => {

                this.projects = response.data.projects;

                this.paginator = response.data.projects.links;
                console.log(this.paginator);
                this.loading = false;
            })
            .catch(error => {

                console.log('Errore: ' + error.message);
            })
        },
    },

    mounted(){
        this.getApi(store.apiUrl + 'projects')
    }
}
</script>

<template>

    <div class="container">
        <h1>Progetti</h1>

        <div id="prj-container">
            <div id="list-container">
                <h3>Lista dei progetti:</h3>

                <div id="list" v-if="loading == false">
                    <ul>
                        <li v-for="(prj, i) in projects.data" :key="i">
                            <strong>{{ prj.id }}</strong>{{ prj.name }}
                        </li>
                    </ul>

                    <div id="paginator">
                        <button
                            v-for="link in paginator"
                            v-html="link.label"
                            :disabled="link.active || !link.url"
                            :class="link.active ? 'active' : ' '"
                            @click="getApi(link.url)"
                        >
                        </button>
                    </div>
                </div>

                <div v-else id="loader-container">
                    <span class="loader"></span>
                </div>
            </div>

        </div>
    </div>
</template>

<style lang="scss" scoped>

    h1{
        text-align: center;
        margin-bottom: 30px;
    }

    #prj-container{
        margin: 10px auto;
        width: 50%;
        height: 70vh;
        border: 2px solid black;
        border-radius: 10px;
        padding: 10px;


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

    #loader-container{
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;

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