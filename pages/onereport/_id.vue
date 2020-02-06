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
        v-if="reports == null"
        />
        <div v-else class="container">
            <div class="columns is centered">
                <div class="column is-12">
                    <gmap 
                    :lat="reports.alert.latitude"
                    :lng="reports.alert.longitude"
                    :full="true"
                    :image="reports.alert.image"
                    />
                </div>
            </div>
            <div 
            v-if="reports.alert.image !== null" 
            class="columns is-centered is-multiline is-gapless">
                <div class="column is-12">
                    <div id="imagen" style="width: 100%; height: 500px;"
                    v-bind:style="{ 'background-image': 'url(' + reports.alert.image + ')' }"
                    > 
                    </div>
                </div>
                <div class="column is-12">
                    <a :href="reports.alert.image" target="_blank">
                    <button 
                    class="button is-success is-fullwidth">Ver imagen en tamaño completo
                    </button>
                    </a>
                </div>
            </div>
            <div v-if="reports.alert.coments !== null " class="columns is-centered">
                <div class="column is-12">
                    <div class="box">
                        <form>
                            <div v-if="reports.alert.calle !==null" class="field">
                                <label class="label">Calle:</label>
                                <div class="control">
                                    <p>{{reports.alert.calle}}</p>
                                </div>
                            </div>
                            <div v-if="reports.alert.colonia !== null" class="field">
                                <label class="label">Colonia:</label>
                                <div class="control">
                                    <p>{{reports.alert.colonia}}</p>
                                </div>
                            </div>
                            <div v-if="reports.alert.numerocasa !== null" class="field">
                                <label class="label">Numero de casa:</label>
                                <div class="control">
                                    <p>{{reports.alert.numerocasa}}</p>
                                </div>
                            </div>
                            <div v-if="reports.alert.coments !== null" class="field">
                                <label class="label">Detalles:</label>
                                <div class="control">
                                    <p>{{reports.alert.coments}}</p>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
            <div class="columns is-centered is-multiline is-gapless">
                <div class="column is-12">
                    <div class="box">
                        <form @submit.prevent="responder(reports.id)">
                            <div class="field">
                                <label class="label">Nombre del Supervisor</label>
                                <div class="control">
                                    <input class="input" 
                                    v-model="supervisor_name"
                                    type="text" 
                                    placeholder="El supervisor que inspeccionó este reporte">
                                </div>
                            </div>
                            <div class="field">
                                <label class="label">Reporte</label>
                                <div class="control">
                                    <textarea v-model="report"
                                    class="textarea" placeholder="Escribe lo que quieras reportar">
                                    </textarea>
                                </div>
                            </div>
                            <div class="field">
                                <div class="file is-info has-name">
                                    <label class="file-label">
                                    <input 
                                    accept="image/png, image/jpeg" @change="onFileChange"
                                    class="file-input" type="file" name="resume">
                                    <span class="file-cta">
                                        <span class="file-icon">
                                        <i class="fas fa-upload"></i>
                                        </span>
                                        <span class="file-label">
                                        Info file…
                                        </span>
                                    </span>
                                    <span class="file-name">
                                        Añade tu imagen
                                    </span>
                                    </label>
                                <button 
                                :disabled="respondiendo"
                                class="button is-primary is-fullwidth">
                                <span v-if="this.reports.status !== 'Finalizado'">Reportar!</span>
                                <span v-else>Actualizar!</span>
                                </button>
                                </div>
                                </div>
                                <div class="field">
                                    <div class="control">
                                        <div
                                        id="image"
                                        style="width: 100%; height: 50px"
                                        v-bind:style="{ 'background-image': 'url(' + image + ')' }"
                                        >
                                        </div>
                                    </div>
                                </div>
                        </form>
                    </div>
                    <div class="column is-12">
                        <label>Envia una notificación</label>
                        <div class="select is-fullwidth">
                        <select v-model="notif">
                            <option value=""></option>
                            <option value="¡Estamos retrasados! Vamos en camino">
                                ¡Estamos retrasados! Vamos en camino
                            </option>
                            <option value="¡Acabamos de llegar pronto te llamaremos!">
                                ¡Acabamos de llegar pronto te llamaremos!
                            </option>
                            <option value="Modificamos uno de tus reportes añadiendo mas detalles">
                                Modificamos uno de tus reportes añadiendo mas detalles
                            </option>
                        </select>
                        </div>
                        <div class="column is-12">
                            <button 
                            :disabled="respondiendo"
                            @click="notificar(reports.id)"
                            class="button is-primary is-fullwidth">
                                Notificar
                            </button>
                        </div>
                    </div>
                    <div 
                    v-if="reports.status == 'Finalizado'"
                    class="column is-12">
                    <div class="box">
                        <form>
                            <div class="field">
                                <div class="control">
                                    <h2 class="title is-5">Tu reporte Actual</h2>
                                </div>
                            </div>
                            <div class="field">
                                <label class="label">Nombre del supervisor:</label>
                                <div class="control">
                                    <p>{{reports.supervisor_name}}</p>
                                </div>
                            </div>
                            <div class="field">
                                <label class="label">Comentarios:</label>
                                <div class="control">
                                    <p>{{reports.report}}</p>
                                </div>
                            </div>
                            <div class="field"  v-if="reports.image !== null">
                                <label class="label">Imagen: </label>
                                <div
                                id="imagen" style="width: 100%; height: 500px;"
                                v-bind:style="{ 'background-image': 'url(' + reports.image + ')' }"
                                class="control">
                                </div>
                            </div>
                        </form>
                    </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import day from '../../components/day'
import nigth from '../../components/night'
import navbar from '../../components/navbar'
import loading from '../../components/loading'
import moment from 'moment-timezone'
import gmap from '../../components/map'
moment.tz.setDefault('America/Mexico_City')

    export default {
        components: {
            day,nigth,navbar, loading, gmap
        },
        data(){
            return {
                time:'',
                hour:'',
                reports: null,
                supervisor_name:'',
                report: '',
                image: '',
                file: {},
                notif: '',

                respondiendo: false
            }
        },
        async created(){
            await this.date()
            await this.gettoken()
        },
        async mounted(){
            await this.getreport()
			setInterval( async() => {
				 await this.date()
			}, 60000)
        },
        methods: {
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
            async getreport(){
                this.reports = null
				const petratoken = await localStorage.getItem('petratoken')
                this.$axios.get('superuser/getone/'+this.$route.params.id,  
                {headers: {Authorization: `Bearer ${petratoken}`}
                }).then(response => {
                    console.log(response.data.data)
                    this.reports = response.data.data
                }).catch(error => {
                    alert('Tuvimos un error, intentalo después')
                })
            },

            onFileChange(e) {
                let files = e.target.files || e.dataTransfer.files;
                if (!files.length)
                return;
                this.createImage(files[0]);
                this.file = files[0];
                },
            createImage(file) {
                let image = new Image();
                let reader = new FileReader();
                let vm = this;
                reader.onload = (e) => {
                vm.image = e.target.result;
                };
                reader.readAsDataURL(file);
            },
            async responder(id){
                this.respondiendo = true
				const petratoken = await localStorage.getItem('petratoken')
                if (this.image == ''){
                    this.image = null
                }
                this.$axios.put('superuser/respond',{
                    reportid: id,
                    supervisor_name: this.supervisor_name,
                    coments: this.report,
                    image: this.image
                }, {headers: {Authorization: `Bearer ${petratoken}`}
                }).then(response => {
                    alert('¡Respondiste a este reporte!')
                    this.respondiendo = false
                    this.$router.push('/home')
                }) .catch(error => {
                    alert(error.response.data.message)
                    this.respondiendo = false
                })
            },
            async notificar(id){
				const petratoken = await localStorage.getItem('petratoken')
                this.respondiendo = true
                this.$axios.post('superuser/notificar',{
                    reportid: id,
                    notification: this.notif
                },{headers: {Authorization: `Bearer ${petratoken}`}
                }).then(response => {
                    alert('Notificación enviada')
                    this.respondiendo = false
                    this.notif = ''
                }).catch(error =>{
                    alert(error.response.data.message)
                    this.respondiendo = false
                    this.notif = ''
                })
            }
        }
        
    }
</script>

<style scoped>
#imagen {
    background-repeat: no-repeat;
    background-position: center;
    background-size: auto 100%;
    background-color: black;
}
</style>