<template>
    <div>
		<navbar/>
		<day
		v-if="hour >= '06'"
		/>
		<nigth
		v-if="hour >= '18' || hour <= '06'"
		/>
        <loading
        v-if="loading == true"
        />
        <div v-else class="container">
            <div class="columns is-centered">
                <div class="column is-12">
                    <div class="tabs is-toggle is-centered">
                        <ul>
                            <li @click="getreports('Enviado',null)"
                            :class="{'is-active' : menu.enviado }">
                            <a>
                                <span>Enviado</span>
                            </a>
                            </li>
                            <li @click="getreports('En proceso',null)"
                            :class="{'is-active' : menu.en_proceso }">
                            <a >
                                <span>En proceso</span>
                            </a>
                            </li>
                            <li @click="getreports('Finalizado',null)"
                            :class="{'is-active' : menu.finalizado }">
                            <a>
                                <span>Finalizado</span>
                            </a>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
            <div v-for="report in reports" class="columns is-centered">
                <div class="column is-12">
                    <div class="box">
                        <article class="media">
                            <div class="media-left">
                                <gmap
                                :lat="report.alert.latitude"
                                :lng="report.alert.longitude"
                                :full="false"
                                :image="report.alert.image"
                                />
                            </div>
                            <div class="media-content">
                                <div class="content">
                                    <p>
                                    <strong>{{report.user.name}}</strong>
                                    <small
                                    v-if="report.user.is_premium == true" 
                                    id="premium">
                                    <i class="fas fa-star"></i>
                                    </small>
                                    <small><strong>{{report.user.number}}</strong></small> <br> 
                                    <small>{{report.alert.type}}</small>
                                    </p>
                                    <br>
                                    <p v-if="report.alert.coments !== null">{{report.alert.coments}}</p> <br>
                                    <strong>Supervisión: </strong> <span v-if="report.alert.supervition == true">Sí</span><span v-else>No</span> <br>
                                    <button v-if="report.status == 'Enviado'" 
                                    @click="onproccess(report.id, consulta)" :disabled="sendalert"
                                    class="button is-success is-fullwidth">Atender</button>
                                    <button v-if="report.status == 'En proceso'" 
                                    @click="gobyid(report.id)"
                                    class="button is-warning is-fullwidth">Responder</button>
                                    <button 
                                    @click="gobyid(report.id)"
                                    v-if="report.status == 'Finalizado'" class="button is-white is-fullwidth">Editar</button>
                                </div>
                            </div>
                        </article>
                    </div>
                </div>
            </div>
            <div class="columns is-centered">
                <div id="footpage" class="column is-half">
                    <nav class="pagination" role="navigation" aria-label="pagination">
                        <ul class="pagination-list">
                            <li v-if="reports.length >= 6 " @click="getreports(consulta, page)">
                            <a class="pagination-link is-current" aria-label="Page 1" aria-current="page">{{this.page+1}}</a>
                            </li>
                            <li v-else @click="getreports(consulta, null)">
                            <a class="pagination-link is-current" aria-label="Page 1" aria-current="page">No hay más contenido, regresar</a>
                            </li>
                        </ul>
                    </nav>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import day from '../components/day'
import nigth from '../components/night'
import navbar from '../components/navbar'
import loading from '../components/loading'
import moment from 'moment-timezone'
import gmap from '../components/map'
moment.tz.setDefault('America/Mexico_City')
    export default {
        components: {
            day,nigth,navbar, loading, gmap
        },
        data(){
            return{ 
				time: '',
                hour: '',
                sendalert: false,
                menu: {enviado: true, en_proceso : false, finalizado: false},
                loading:true,
                
                consulta: '',
                reports : {},
                page: 1,
                interval: false

            }
        },
        async created(){
            await this.date()
            await this.gettoken()
            await this.getreports('Enviado', null)
        }, 
        mounted(){
            this.interval = setInterval( async() => {
            await this.date()
            await this.getreports(this.consulta)
            }, 60000)
        },
        methods:{
			async date(){
			let hour = moment().format('A')
			let time = moment().format('HH')
			this.time = hour
			this.hour= time
			},
			async gettoken(){
				const petratoken = await localStorage.getItem('petratoken')
				if(petratoken == null){
					this.$router.push('/')
				}
            },
            async getreports(status, page){
                this.consulta = status
                if(page == null){
                    this.page = 1
                } else {
                    this.page ++
                }
                await this.tabs(this.consulta)
                this.reports = {}
                this.loading = true
                window.scrollTo(0,0)
				const petratoken = await localStorage.getItem('petratoken')
                this.$axios.post('superuser/getallalerts/',{
                    page: this.page,
                    status: status
                }, {headers: {Authorization: `Bearer ${petratoken}`}
                }).then(response => {
                    this.reports = response.data.data.data
                    this.loading = false
                    console.log(this.reports)
                })
            },
            tabs(dato){
                if(dato == 'Enviado'){
                    this.menu.enviado = true
                    this.menu.en_proceso = false
                    this.menu.finalizado = false
                }else if(dato == 'En proceso'){
                    this.menu.enviado = false
                    this.menu.en_proceso = true
                    this.menu.finalizado = false
                } else if(dato == 'Finalizado'){
                    this.menu.enviado = false
                    this.menu.en_proceso = false
                    this.menu.finalizado = true
                }
            },
            async onproccess(id, dato){
				const petratoken = await localStorage.getItem('petratoken')
                this.sendalert = true
                this.$axios.get('superuser/onproccess/'+id, 
                {headers: {Authorization: `Bearer ${petratoken}`}
                })
                .then(response => {
                    alert('Status enviado')
                    this.getreports(dato)
                    this.sendalert = false
                }).catch(error => {
                    error.response.data.message
                    this.sendalert = false
                })
            },
            async gobyid(id){
                await clearInterval(this.interval)
                this.$router.push('onereport/'+id)
            }
            
        }

        
    }
</script>

<style scoped>
#footpage{
    display: flex;
    justify-content: center;
}
#premium{
    color: #00FF00
}

</style>