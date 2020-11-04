<template>
    <div class="v-main-wrapper">
      <v-header/>
       <keep-alive>
           <router-view></router-view>
       </keep-alive>
   <!-- <v-catalog /> -->
   <!-- <v-cart -->
   <!--   v-if="CART.length"-->
   <!--     :cart_data="CART"-->
   <!-- />-->
    </div>
</template>




<script> 
    import vHeader from './layouts/v-header'
   // import vCatalog from './v-catalog'
    //import vCart from './v-cart'
   // import {mapGetters} from 'vuex'
    
    export default {
        name: 'v-main-wrapper',
        components: {
       //        vCatalog,
       //     vCart,
            vHeader
        },
        props:{},
        data() {
            return{
                
            }
        },
        computed: {
           // ...mapGetters([
           //     'CART'
           // ])
        },
        methods: {},
        watch: {},
        mounted() {} 
    }

</script>


<style>
    .v-main-wrapper {
        margin: 0 auto;
    }

</style>
