<template lang="pug">
    .section.grey.portfolio
        .titulo
            h2 PORTFOLIO
        .portfolio__content-cont
            .portfolio__item-orange.title
                span web
            .portfolio__item-orange(v-for='(item,index) in datos.web', :key="'port-'+ index")
                a.portfolio__link(href='', :l-box='item.id')
                    img.portfolio__img(:src='item.thumb_src', :alt='item.nombre')
                .item-hover
                    span {{item.name}}<br>{{item.description}}
            .portfolio__item-blue.title
                span brand
            .portfolio__item-blue(v-for='(item,index) in datos.branding', :key="'brand-'+ index")
                a.portfolio__link(href='', :l-box='item.id')
                    img.portfolio__img(:src='item.thumb_src', :alt='item.nombre')
                .item-hover
                    span {{item.name}}<br>{{item.description}}
            .portfolio__item-red.title
                span print
            .portfolio__item-red(v-for='(item,index) in datos.print', :key="'print-'+ index")
                a.portfolio__link(:href='item.link', target='_blank')
                    img.portfolio__img(:src='item.thumb_src', :alt='item.nombre')
                .item-hover
                    span {{item.name}}<br>{{item.description}}
</template>

<script>
import db from '../firebase/init'
export default {
    name: "vportfolio",
    data(){
        return{
            datos:{}
        }
    },
    methods:{
        getPortfolio() {
            let app = this
            let appData = {}
			db.collection('web-data/portfolio/web').onSnapshot(snapshot => {
                let webObject = {}
                snapshot.forEach(doc => {
					webObject[doc.id] = doc.data()
				})
                appData.web = webObject
            })
			db.collection('web-data/portfolio/branding').onSnapshot(snapshot => {
                let brandingObject = {}
                snapshot.forEach(doc => {
					brandingObject[doc.id] = doc.data()
				})
                appData.branding = brandingObject
            })
			db.collection('web-data/portfolio/print').onSnapshot(snapshot => {
                let printObject = {}
                snapshot.forEach(doc => {
					printObject[doc.id] = doc.data()
				})
                appData.print = printObject
                app.datos = appData
                console.log(appData);
                
            })       
        }
    },

	created(){
		this.getPortfolio()
	},
}
</script>

<style lang="sass">
// TOOLS
@import "../../_includes/sass/1-tools/tools"

@import "../../_includes/sass/3-modules/portfolio"
</style>