<template>
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :items="3" :nav="false" :margin="24">

                        <div class="product-item" v-for="product in products" v-bind:key="product.id">
                            <div class="pi-pic" v-if="product.galleries.length == 1">
                                <img v-bind:src="product.galleries.find(el => el.is_default).photo" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <a @click="saveCart(product)" style="cursor: pointer"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view"><router-link v-bind:to="'/product/'+product.id">+ Quick View</router-link></li>
                                </ul>
                            </div>
                            <div class="pi-pic" v-else>
                                <img src="img/products/no-image.png" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <a @click="saveCart(product)" style="cursor: pointer"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view"><router-link v-bind:to="'/product/'+product.id">+ Quick View</router-link></li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="category-name">{{ product.type }}</div>
                                <a href="#">
                                    <h5>{{ product.name }}</h5>
                                </a>
                                <div class="product-price">
                                    Rp {{ product.price }}
                                </div>
                            </div>
                        </div>
                    </carousel>
                </div>
                <div class="col-lg-12" v-else>
                    <p>
                        Produk belum tersedia
                    </p>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
    name: 'Banner',
    components: {
        carousel,
    },
    data() {
        return {
            products: [],
            userCart: []
        }
    },
    methods: {
        saveCart(product) {
            console.log("saved")
            console.log(product)

            let productExistOnCart = this.userCart.find((p) => {
                return p.id === product.id
            })

            if(productExistOnCart)
                productExistOnCart.count++
            else
            {
                let image = (product.galleries.length > 0) 
                    ? product.galleries.find(el => el.is_default).photo
                    : image = "img/products/no-image.png"

                this.userCart.push(
                    {
                        id: product.id,
                        name: product.name,
                        price: product.price,
                        image: image,
                        count: 1
                    }
                )
            }

            let parsed = JSON.stringify(this.userCart)
            localStorage.setItem('userCart', parsed)
        }
    },
     mounted() {
        if (localStorage.getItem('userCart')) {
            try {
                this.userCart = JSON.parse(localStorage.getItem('userCart'));
            } catch(e) {
                localStorage.removeItem('userCart');
            }
        }

        axios
            .get("http://shayna-laravel-admin.localhost/api/products", {
                params: {
                    per_page: 3,
                    gallery: "true",
                    gallery_default: "true"
                }
            })
            .then(res => {
                this.products = res.data.data
            })
            .catch(err => console.log(err))
    }
}
</script>