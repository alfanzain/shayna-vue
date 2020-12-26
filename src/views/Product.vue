<template>
  <div class="product">
    <!-- Header Section Begin -->
    <Header />
    <!-- Header End -->

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom" v-if="productDetail && productDetail.galleries.length > 0">
                                <img class="product-big-img" :src="productImageView" alt="" />
                            </div>
                            <div class="product-pic-zoom" v-else>
                                <img class="product-big-img" src="img/products/no-image.png" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetail && productDetail.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider" :loop="false" :nav="true" :dots="false" :margin="10">

                                    <div 
                                        class="pt" 
                                        v-for="image in productDetail.galleries" 
                                        :key="image.id"
                                        @click="changeImage(image.photo)" 
                                        :class="image.photo == productImageView ? 'active' : ''"
                                    >
                                        <img :src="image.photo" alt="" />
                                    </div>

                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details">
                                <div class="pd-title">
                                    <span>{{ productDetail && productDetail.type }}</span>
                                    <h3>{{ productDetail && productDetail.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <div v-html="productDetail && productDetail.description"></div>
                                    <h4>Rp {{ productDetail && productDetail.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <a @click="saveCart(productDetail)" class="primary-btn pd-cart" style="cursor: pointer">Add To Cart</a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

    <!-- Related Products Section Begin -->
    <RelatedProduct />
    <!-- Related Products Section End -->

    <!-- Footer Section Begin -->
    <Footer />
    <!-- Footer Section End -->
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'
import RelatedProduct from '@/components/RelatedProduct.vue'

import carousel from 'vue-owl-carousel'

import axios from 'axios'

export default {
    name: 'Home',
    components: {
        Header,
        Footer,
        RelatedProduct,
        carousel
    },
    data() {
        return {
            productDetail: null,
            productImageView: null,
            userCart: [],
        }
    },
    methods: {
        changeImage(urlImage) {
            this.productImageView = urlImage
        },
        saveCart(product) {
            let productExistOnCart = this.userCart.find((p) => {
                return p.id === product.id
            })

            if(productExistOnCart)
                productExistOnCart.count++
            else
                this.userCart.push(
                    {
                        id: product.id,
                        name: product.name,
                        price: product.price,
                        image: this.productImageView,
                        count: 1
                    }
                )

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
            .get("http://shayna-laravel-admin.localhost/api/products/" + this.$route.params.id, {
                params: {
                    gallery: "true"
                }
            })
            .then(res => {
                this.productDetail = res.data.data
                this.productImageView = this.productDetail.galleries.find(el => el.is_default).photo
            })
            .catch(err => console.log(err))
    }
}
</script>