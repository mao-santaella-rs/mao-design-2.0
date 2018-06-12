<template lang="pug">
    
.edit-portfolio

    link(rel='stylesheet', href='https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css', integrity='sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4', crossorigin='anonymous')
    
    .container
        .row.justify-content-center.mt-5.mb-5
            .col-md-10
                .card.w-100
                    h2.card-header AGREGAR DATOS
                    .card-body
                        form
                            .form-group.row
                                label.col-sm-3.col-form-label(for='inputCategory') Category
                                .col-sm-9
                                    select#inputCategory.form-control(v-model="cat")
                                        option(v-for="(item,index) in categories",:key="'categorie-'+index") {{item}}
                            .form-group.row
                                label.col-sm-3.col-form-label(for='inputName') Name
                                .col-sm-9
                                    input#inputName.form-control(type='text',v-model="name")
                            .form-group.row
                                label.col-sm-3.col-form-label(for='inputId') Id
                                .col-sm-9
                                    input#inputId.form-control(type='text',v-model="id")
                            .form-group.row
                                label.col-sm-3.col-form-label(for='inputThumbsrc') Thumb_src
                                .col-sm-9
                                    input#inputThumbsrc.form-control(type='text',v-model="thumb_src")
                            .form-group.row(v-if="cat != 'print'")
                                label.col-sm-3.col-form-label(for='inputImgsrc') Img_src
                                .col-sm-9
                                    input#inputImgsrc.form-control(type='text',v-model="img_src")
                            .form-group.row
                                label.col-sm-3.col-form-label(for='inputDescription') Description
                                .col-sm-9
                                    input#inputDescription.form-control(type='text',v-model="description")
                            .form-group.row(v-if="cat != 'branding'")
                                label.col-sm-3.col-form-label(for='inputLink') Link
                                .col-sm-9
                                    input#inputLink.form-control(type='text',v-model="link")
                            .form-group
                                a.btn.btn-primary(@click.prevent="updateData()", role='button' href) guardar
                            .form-group
                                span {{mensaje}}
        .row(v-for="(categorie,index) in datos",:key="'categorie-'+index")
            .col-3.mb-3(v-for="(item,index) in categorie",:key="'dato-'+index")
                .card
                    .card-img-top
                        img(v-if="item.img_src",:src="item.img_src", alt='Card image cap')
                        img.img-thumb(:src="item.thumb_src")
                    .card-body
                        p.card-title <b>Name:</b><br>{{item.name}}
                        p.card-text <b>Description:</b><br>{{item.description}}
                        p.card-text <b>Id:</b> {{item.id}}
                        p.card-text(v-if="item.link") <b>Link:</b><br>
                            small {{item.link}}   
</template>

<script>

import db from '../firebase/init'

export default {
    
    name: 'edit',
    
    data(){
        return{
            datos:{},
            categories: ["web","branding","print"],
            cat: "",
            name: "",
            description: "",
            id: "",
            img_src: "",
            thumb_src: "",
            link: "",
            mensaje: ""
        }
    },

    methods:{
		getDataBase() {
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
            })       
        },
        clearInputs(){
            let app = this
            app.name = "",
            app.description = "",
            app.id = "",
            app.thumb_src = "",
            app.img_src = "",
            app.link = ""
        },
        updateData(){
            
            const app = this
            const collectionRoute = 'web-data/portfolio/' + app.cat
            const savingMessage = "Guardando..."
            const succesMessage = "Se guardo correctamente!"
            const errorMessage = "Error al guardar: "
            const inputEmpty = "Faltan campos por llenar"
            const categorieEmpty = "Escoja categoria"

            if (app.cat == 'web') {
                if (app.name && app.description && app.id && app.img_src && app.thumb_src && app.link) {
                    app.mensaje = savingMessage
                    db.collection(collectionRoute).add({
                        name: app.name,
                        description: app.description,
                        id: app.id,
                        thumb_src: app.thumb_src,
                        img_src: app.img_src,
                        link: app.link
                    }).then( () => {
                        app.mensaje = succesMessage
                        app.clearInputs()
                    }).catch( error => {
                        app.sendError(errorMessage + error)
                    })
                } else {
                    app.mensaje = inputEmpty
                }
            }else if (app.cat == 'branding') {
                if (app.name && app.description && app.id && app.img_src && app.thumb_src) {
                    app.mensaje = savingMessage
                    db.collection(collectionRoute).add({
                        name: app.name,
                        description: app.description,
                        id: app.id,
                        thumb_src: app.thumb_src,
                        img_src: app.img_src
                    }).then( () => {
                        app.mensaje = succesMessage
                        app.clearInputs()
                    }).catch( error =>{
                        app.sendError(errorMessage + error)
                    })
                } else {
                    app.mensaje = inputEmpty
                }
            }else if (app.cat == 'print') {
                if (app.name && app.description && app.id && app.thumb_src && app.link) {
                    app.mensaje = savingMessage
                    db.collection(collectionRoute).add({
                        name: app.name,
                        description: app.description,
                        id: app.id,
                        thumb_src: app.thumb_src,
                        link: app.link
                    }).then( () => {
                        app.mensaje = succesMessage
                        app.clearInputs()
                    }).catch( error =>{
                        app.sendError(errorMessage + error)
                    })

                } else {
                    app.mensaje = inputEmpty
                }
            }else{
                app.mensaje = categorieEmpty
            }
        }
	},
	
	computed:{
	},

	created(){
		this.getDataBase()
	},

	mounted(){
        // this.updateData()
	}
}
</script>

<style lang="sass" scoped>

@import "../../_includes/sass/1-tools/tools"

.card-img-top
    position: relative
    min-height: 80px
    border-bottom: 1px solid $lighter-gray

.card
    p
        font-size: 0.9rem
        margin-bottom: 10px
        &:last-child
            margin-bottom: 0
    br
        line-height: 1.2em

.card-body
    padding: 15px !important

.img-thumb
    +position(absolute,0 0 null null)
    width: 80px
    border-bottom: 1px solid $lighter-gray
    border-left: 1px solid $lighter-gray

input[type="text"]
    margin: 0

</style>
