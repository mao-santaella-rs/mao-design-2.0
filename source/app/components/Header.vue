<template lang="pug">

    header.header(v-if='datos', v-on:mousemove='logoRotation')
        .header__logo-cont
            .header__logo__bubble
                span(v-if='datos.greetings') {{datos.greetings[greetingRndm(datos.greetings)]}}
            img.header__logo-img(:src='datos.logo', alt='')
        .header__content
            span.nombre {{datos.name}}
            span {{datos.under_line_l1}}
            span {{datos.under_line_l2}}

</template>

<script>
export default {
	name: "vheader",
    props:['datos'],
    data(){
        return{

        }
    },
    methods:{
        greetingRndm: array => {
            var rndmNum = Math.floor((Math.random() * array.length))            
            return rndmNum
        },

        logoRotation: cords => {
            var $header = $('.header');
            var $header_logo_cont = $('.header__logo-cont');

            var x = cords.clientX - $header.position().left;
            var y = cords.clientY - $header.position().top;
            var dx = $header.innerWidth()/2 - x;
            var dy = $header.innerHeight()/2 - y;
            var dxp = dx/($header.innerWidth()/2);
            var dyp = dy/($header.innerHeight()/2);

            var newTransformRotateXY = "rotateY(" + 20*Math.tan(dxp) + "deg) rotateX(" + -20*Math.tan(dyp) + "deg)";

            $header_logo_cont.css({
                'transform': newTransformRotateXY
            });
        }


    },

    mounted: function(){
    }
}
</script>

<style lang="sass">

// TOOLS
@import "../../_includes/sass/1-tools/tools"

@import "../../_includes/sass/3-modules/header"


</style>
