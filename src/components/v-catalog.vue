<template>
    <div class="v-catalog">
        <router-link :to="{name: 'cart', params: {cart_data: CART}}">
            <div class="v-catalog_link_to_cart">Cart: {{CART.length}}</div>
        </router-link>
        <h1>Catalog</h1>
        <v-select :selected="selected" :options="categories" @select="sortByCategories" />
        <div class="v-catalog_list">
            <v-catalog-item v-for="product in filtredProducts" :key="product.article"
             v-bind:product_data="product"
              @addToCart="addToCart"
              @productClick="productClick"
                />
        </div>
    </div>

</template>


<script>
    import vCatalogItem from './v-catalog-item'
    import {
        mapActions,
        mapGetters
    } from 'vuex'
    import vSelect from './v-select'
    export default {
        name: "v-catalog",
        components: {
            vCatalogItem,
            vSelect
        },
        props: {},
        data() {
            return {
                categories: [{
                        name: 'All',
                        value: "al"
                    },
                    {
                        name: 'Accessories',
                        value: "acces"
                    },
                    {
                        name: 'Laptop',
                        value: 'lap'
                    },
                ],
                selected: 'ALL',
                sortedProducts: []
            }
        },
        computed: {
            ...mapGetters([
                'PRODUCTS',
                'CART',
                'SEARCH_VALUE'
            ]),
            filtredProducts() {
                if (this.sortedProducts.length) {
                    return this.sortedProducts
                } else {
                    return this.PRODUCTS
                }
            },
        },
        methods: {
            ...mapActions([
                'GET_PRODUCTS_FROM_API',
                'ADD_TO_CART'
            ]),
                productClick(article){
                this.$router.push( {name: 'product', query:{ 'product': article }})
                
            },
            sortByCategories(category) {
                this.sortedProducts = [];
                let vm = this;
                this.PRODUCTS.map(function(item) {
                    if (item.category === category.name) {
                        vm.sortedProducts.push(item);
                    }
                })
                this.selected = category.name
            },
            addToCart(data) {
                this.ADD_TO_CART(data)
            },
            sortProductsBySearchValue(value) {
                this.sortedProducts = [...this.PRODUCTS]
                if (value) {
                    this.sortedProducts = this.sortedProducts.filter(function(item) {
                        return item.name.toLowerCase().includes(value.toLowerCase())
                    })
                } else {
                    this.sortedProducts = this.PRODUCTS;
                }
            },
        },
        watch: {
            SEARCH_VALUE() {
                this.sortProductsBySearchValue(this.SEARCH_VALUE);
            }
        },

        mounted() {
            this.GET_PRODUCTS_FROM_API()
                .then((response) => {
                    if (response.data) {
                        console.log('Data arrived!')
                    }
                })
        }
    }

</script>


<style lang="scss">
    .v-catalog {
        &_list {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
        }

        &_link_to_cart {
            position: absolute;
            top: 80px;
            right: 10px;
            padding: 16px;
            border: solid 1px #aeaeae;
        }
    }

</style>
