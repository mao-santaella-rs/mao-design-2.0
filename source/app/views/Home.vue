<template lang="pug">

	#home
		vheader(v-if='datos', :datos='datos.header')

		vportfolio(v-if='datos.portfolio')

		vabout(v-if='datos.about', :datos='datos.about')

		.mail-section.orange(v-if='datos.about')
			a.mail-link(href='#') {{datos.about.email}}

		footer.red.footer(v-if='datos.header')
			span {{datos.header.name}} {{new Date().getFullYear()}}

</template>

<script>
// import axios     from 'axios'
import db from '../firebase/init'
import vheader from '../components/Header.vue'
import vportfolio from '../components/Portfolio.vue'
import vabout from '../components/About.vue'

export default {
	name: 'home',

	components: {
		vheader,
		vportfolio,
		vabout
	},

	data(){
		return{
			datos:{}
		}
	},

	methods:{
		getRealTimeData() {
			let app = this
			let dbObject = {}
			db.collection('web-data').onSnapshot(snapshot => {
				snapshot.forEach(doc => {
					dbObject[doc.id] = doc.data()
				})
				app.datos = dbObject
				console.log(dbObject);
				
			})
		}
	},
	
	computed:{
	},

	created(){
		this.getRealTimeData()
	},

	mounted(){
	}

}
</script>

<style lang="sass">
// TOOLS
@import "../../_includes/sass/1-tools/tools"

@import "../../_includes/sass/3-modules/mail"
@import "../../_includes/sass/3-modules/footer"
</style>