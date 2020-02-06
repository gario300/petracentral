<template>
	<div>
		<navbar/>
		<day
		v-if="hour >= '06'"
		/>
		<nigth
		v-if="hour >= '18' || hour <= '06'"
		/>
		<div class="container">
			<div class="columns is-centered">
				<div class="column is-12">
					<div v-if="formtype == 1" class="card">
						<form @submit.prevent="login">
							<div class="field">
								<label class="label">Numero de operador</label>
								<div class="control">
									<input class="input" v-model="number" type="number" placeholder="Numero Valido">
								</div>
							</div>
							<div class="field">
								<label class="label">Contraseña</label>
								<div class="control">
									<input class="input" v-model="password" type="password" placeholder="Contraseña">
								</div>
							</div>
							<div class="field">
								<div class="control">
									<button class="button is-fullwidth is-success">
										Acceder
									</button>
								</div>
							</div>
						</form>
						<span> O tal vez prefieres</span><a @click="mostrar"> dar de alta a un operador</a>
					</div>
					<div class="card" v-else>
						<form @submit.prevent="makeop">
							<div class="field">
								<label class="label">Numero de operador</label>
								<div class="control">
									<input v-model="number" class="input" type="number" placeholder="Numero Valido">
								</div>
							</div>
							<div class="field">
								<label class="label">Contraseña</label>
								<div class="control">
									<input v-model="password" class="input" type="password" placeholder="Clave Maestra">
								</div>
							</div>
							<div class="field">
								<div class="control">
									<button :disabled="!sendform" class="button is-fullwidth is-success">
										Crear operador
									</button>
								</div>
							</div>
						</form>
						<a @click="mostrar">Regresar</a>
					</div>
				</div>
			</div>
				<div class="colums is-centered">
					<div class="column is-12" id="logo">
						<figure class="image is-96x96">
						<img src="https://res.cloudinary.com/scute/image/upload/v1566225323/logros/centrodeatencion_oxplzq.png">
						</figure>
					</div>
				</div>
				<div class="colums is-centered">
					<div class="column is-12" id="logo">
						<span>P-Central 1.0</span>
					</div>
				</div>
		</div>
	</div>
</template>

<script>
import day from '../components/day'
import nigth from '../components/night'
import navbar from '../components/navbar'
import moment from 'moment-timezone'
moment.tz.setDefault('America/Mexico_City')
	export default {
		components: {
			day,nigth,navbar
		},
		data(){
			return{
				time: '',
				hour: '',
				formtype:1,

				number: '',
				passord:'',

				sendform : true
			}
		},
		async created(){
			await this.date()
			await this.gettoken()
		},
		mounted(){
			setInterval( () => {
				this.date()
			}, 60000)
		},
		methods:{
			async date(){
			let hour = moment().format('A')
			let time = moment().format('HH')
			this.time = hour
			this.hour= time
			},
			mostrar(){
				if(this.formtype == 1){
					this.formtype ++
				} else{
					this.formtype --
				}
				this.number = '',
				this.password = ''
			},
			async gettoken(){
				const petratoken = await localStorage.getItem('petratoken')
				if(petratoken !== null){
					this.$router.push('home')
				}
			},
			async makeop() {
				this.sendform = false
				this.$axios.post('makeop', {
					superpassword: this.password,
					usernumber: this.number
				})
				.then(response => {
					alert('Registraste a un nuevo operador')
					this.number = '',
					this.password = '',
					this.sendform = true
				})
				.catch(error => {
					alert('Hubo un error, intentalo más tarde ')
					this.number = '',
					this.password = '',
					this.sendform = true
				})

			},
			async login(){
				this.sendform = false
				this.$axios.post('login', {
					number: this.number,
					password:this.password
				})
				.then(response => {
					localStorage.setItem('petratoken', response.data.data.token)
					this.$router.push('home')
					this.sendform = true
				})
				.catch(error => {
					this.number = '',
					this.password = '',
					this.sendform = true
					alert(error.response.data.message)
				})
			}
		}
		
	}
</script>

<style scoped>
.card{
	padding: 9px;
	margin-top: 9em;
	opacity: .88;
}

#logo{
	display: flex;
	justify-content: center;
}

</style>