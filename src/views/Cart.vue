
<template>
    <div class="cart">
        <!-- Header Section Begin -->
        <Header />
        <!-- Header End -->
        
        <section class="shopping-cart spad">
            <div class="container">
                <div class="row" v-if="userCart && userCart.length > 0">
                    <div class="col-lg-8">
                        <div class="row">
                            <div class="col-lg-12">
                                <div class="cart-table">
                                    <table>
                                        <thead>
                                            <tr>
                                                <th>Image</th>
                                                <th class="p-name text-center">Product Name</th>
                                                <th>Price</th>
                                                <th>Action</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr v-for="cart in userCart" :key="cart.id">
                                                <td class="cart-pic first-row">
                                                    <img :src="cart.image" alt="" />
                                                </td>
                                                <td class="cart-title first-row text-center">
                                                    <h5>{{ cart.name }}</h5>
                                                </td>
                                                <td class="p-price first-row">Rp {{ cart.price }} x {{ cart.count }}</td>
                                                <td class="delete-item">
                                                    <a href="#"><i @click="removeItem(cart.id)" class="material-icons">close</i></a></td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                            <div class="col-lg-8">
                                <h4 class="mb-4">
                                    Informasi Pembeli:
                                </h4>
                                <div class="user-checkout">
                                    <form>
                                        <div class="form-group">
                                            <label for="namaLengkap">Nama lengkap</label>
                                            <input 
                                                type="text" 
                                                class="form-control" 
                                                id="namaLengkap" 
                                                aria-describedby="namaHelp" 
                                                placeholder="Masukan Nama"
                                                v-model="customer.name"
                                            />
                                        </div>
                                        <div class="form-group">
                                            <label for="namaLengkap">Email Address</label>
                                            <input type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email"
                                            v-model="customer.email"
                                        />
                                        </div>
                                        <div class="form-group">
                                            <label for="namaLengkap">No. HP</label>
                                            <input type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP"
                                            v-model="customer.number"
                                        />
                                        </div>
                                        <div class="form-group">
                                            <label for="alamatLengkap">Alamat Lengkap</label>
                                            <textarea class="form-control" id="alamatLengkap" rows="3" v-model="customer.address"></textarea>
                                        </div>
                                    </form>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-lg-4">
                        <div class="row">
                            <div class="col-lg-12">
                                <div class="proceed-checkout">
                                    <ul>
                                        <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                        <li class="subtotal mt-3">Subtotal <span>Rp {{ totalPrice }}</span></li>
                                        <li class="subtotal mt-3">Pajak <span>{{ tax }} ({{ totalTax }})</span></li>
                                        <li class="subtotal mt-3">Total Biaya <span>Rp {{ totalPriceAfterTax }}</span></li>
                                        <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                        <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                        <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                    </ul>
                                    <a href="success.html" class="proceed-btn">I ALREADY PAID</a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row" v-else>
                    Oops. Keranjang masih kosong nich~
                </div>
            </div>
        </section>

        <!-- Footer Section Begin -->
        <Footer />
        <!-- Footer Section End -->
    </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'

// import axios from 'axios'

export default {
    name: 'Home',
    components: {
        Header,
        Footer
    },
    data() {
        return {
            userCart: [],
            customer: {
                name: '',
                email: '',
                number: '',
                address: ''
            },
            tax: 10
        }
    },
    methods: {
        removeItem(id) {
               this.userCart = this.userCart.filter((p) => {                          
                return p.id !== id
            })

            const parsed = JSON.stringify(this.userCart)
            localStorage.setItem('userCart', parsed)
        },
        // checkout() {
        //     let productIds = this.userCart.map((product) => product.id)

        //     let checkoutData = {
        //         name: this.customer.name,
        //         email: this.customer.email,
        //         number: this.customer.number,
        //         address: this.customer.address,
        //         transaction_total: this.totalPriceAfterTax,
        //         transaction_status: "PENDING",
        //         transaction_details: productIds
        //     }
        // }
    },
    mounted() {
        if (localStorage.getItem('userCart')) {
            try {
                this.userCart = JSON.parse(localStorage.getItem('userCart'));
            } catch(e) {
                localStorage.removeItem('userCart');
            }
        }
    },
    computed: {
        totalPrice: function () {
            return this.userCart.reduce((total, product) => product.price * product.count + total, 0)
        },
        totalTax: function () {
            return this.totalPrice * this.tax / 100
        },
        totalPriceAfterTax: function () {
            return this.totalPrice - this.totalTax
        }
    }
}
</script>