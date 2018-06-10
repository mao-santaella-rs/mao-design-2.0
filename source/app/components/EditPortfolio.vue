<template lang="pug">
    
    .edit-portfolio
        
        .container
            .row.justify-content-center.mt-5.mb-5
                .col-md-8
                    .card.w-100
                        h2.card-header AGREGAR DATOS
                        .card-body
                            form
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
                                .form-group.row
                                    label.col-sm-3.col-form-label(for='inputImgsrc') Img_src
                                    .col-sm-9
                                        input#inputImgsrc.form-control(type='text',v-model="img_src")
                                .form-group.row
                                    label.col-sm-3.col-form-label(for='inputDescription') Description
                                    .col-sm-9
                                        input#inputDescription.form-control(type='text',v-model="description")
                                .form-group.row
                                    label.col-sm-3.col-form-label(for='inputLink') Link
                                    .col-sm-9
                                        input#inputLink.form-control(type='text',v-model="link")
                                .form-group
                                    a.btn.btn-primary(@click.prevent="updateData()", role='button') guardar
                                .form-group
                                    span {{mensaje}}
            .row.mb-5
                .col-4(v-for="(item,index) in datos",:key="'dato-'+index")
                    .card
                        img.card-img-top(:src="item.img_src", alt='Card image cap')
                        .card-body
                            p.card-title Name: {{item.name}}
                            p.card-text Description: {{item.description}}
                            p.card-text Id: {{item.id}}
                            p.card-text Link: {{item.link}}
                            img.img-thumb(:src="item.thumb_src")



        link(rel='stylesheet', href='https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css', integrity='sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4', crossorigin='anonymous')

    
</template>

<script>

import db from '../firebase/init'

export default {
    
    name: 'edit',
    
    data(){
        return{
            datos:{},
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
			let dbObject = {}
			db.collection('web-data/portfolio/web').onSnapshot(snapshot => {
                snapshot.forEach(doc => {
					dbObject[doc.id] = doc.data()
				})
                app.datos = dbObject
                console.log(dbObject);
                
            })
        },
        updateData(){
            let app = this

            if (app.name && app.description && app.id && app.img_src && app.thumb_src && app.link) {
                
                db.collection('web-data/portfolio/web').add({
                    name: app.name,
                    description: app.description,
                    id: app.id,
                    img_src: app.img_src,
                    thumb_src: app.thumb_src,
                    link: app.link
                }).then( () => {
                    app.mensaje = "se guardo correctamente"
                }).catch(()=>{
                    app.mensaje = "error al guardar"
                })
                app.mensaje = "todos los campos llenos!"
            } else {
                app.mensaje = "falta un campo por llenar"
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

<style lang="sass">
.img-thumb
    width: 100px

input[type="text"]
    margin: 0
</style>
