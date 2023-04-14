<script>
import Lesson from './components/Lesson.vue'
import Checkout from './components/Checkout.vue'

export default {
  data(){
    return {

                search: '',
                sitename: 'LessonHub',
                lessons: [],
                cart: [],
                showProduct: false,
                order: {
                    firstName: '',
                    lastName: '',
                },
                sort: 'ascending',
                type: '',
                user: {
                    name: '',
                    number: ''
                }
    }
            },
              async created() {
                let course = await fetch("http://joshuacw3-env.eba-i9ghq3bb.eu-west-2.elasticbeanstalk.com/collection/lesson")
                let result = await course.json()
                this.lessons = result
                console.log(result)
            },
            methods: {
                filteredLessons(){
                    fetch(`http://joshuacw3-env.eba-i9ghq3bb.eu-west-2.elasticbeanstalk.com/collection/lesson/search?key_word=${this.search}`)
                    .then(response => {
                        return response.json()
                    })
                    .then(data => {
                        this.lessons = data
                    })
                },
                canAddToCart(lesson) {
                    this.lessons.find(item => item.id == lesson.id).space -= 1;
                    this.cart.push({ cartId: (this.cart.length + 1), ...lesson });
                    // console.log('adding to cart', lesson.id)
                },
                removeFromCart(lesson) {
                    if (confirm('Remove from cart?')) {
                        this.cart = [...this.cart].filter(item => item.cartId != lesson.cartId)
                    }
                },
                showCheckout() {
                    this.showProduct = !this.showProduct;
                },
                submit(name,number) {
                    console.log("Order Submitted")
                    let orders = {
                        checkoutName: name,
                        checkoutPhone: number,
                        cartProduct: this.cart,
                    }
                    let order_details = (JSON.stringify(orders))
                    fetch('http://joshuacw3-env.eba-i9ghq3bb.eu-west-2.elasticbeanstalk.com/collection/order', {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json; charset=UTF-8',
                        },
                        mode: "cors",
                        cache: "no-store",
                        body: order_details,
                    })
                    .then(response => response.json())
                    .then(json_response => {
                        console.log(json_response)
                        this.submitForm()
                    })
                    .catch((error) => {
                        console.log(error);
                    });
                },
                phonenumber(number) {
                    if ((number.match(phoneno))) {
                        return true;
                    } 
                },
                submitForm() {
                        alert("Submitted")
                    },
            },
            computed: {
                total() {
                    return this.cart.length > 0 ? this.cart.map(item => item.price).reduce((acc, cur) => acc + cur) : 0;
                },

                //Everything here is for sorting lessons
                sortedLessons() {
                    switch (this.type) {
                        case 'subject':
                            return this.lessons.sort((a, b) => {
                                switch (this.sort) {
                                    case 'ascending':
                                        return a.subject.toUpperCase() > b.subject.toUpperCase() ? 1 : -1;
                                    default:
                                        return a.subject.toUpperCase() < b.subject.toUpperCase() ? 1 : -1;
                                }
                            })
                        case 'location':
                            return this.lessons.sort((a, b) => {
                                switch (this.sort) {
                                    case 'ascending':
                                        return a.location.toUpperCase() > b.location.toUpperCase() ? 1 : -1;
                                    default:
                                        return a.location.toUpperCase() < b.location.toUpperCase() ? 1 : -1;
                                }
                            })
                        case 'price':
                            return this.lessons.sort((a, b) => {
                                return this.sort == 'ascending' ? a.price - b.price : b.price - a.price;
                            })

                        case 'space':
                            return this.lessons.sort((a, b) => {
                                return this.sort == 'ascending' ? a.space - b.space : b.space - a.space;
                            })
                            
                        default:
                            return this.lessons.sort((a, b) => {
                                return this.sort == 'ascending' ? a.id - b.id : b.id - a.id;
                            })
                    }
                }
            },
  components: {
    Lesson,Checkout
  }
}
</script>

<template>
  
  <main>
     <header>
            
            
            <nav class="navbar navbar-expand-sm navbar-light bg-light">
                <h1 style="color: black;" class="navbar-brand" v-text="sitename"></h1>
                <button class="navbar-toggler d-lg-none" type="button" data-toggle="collapse" data-target="#collapsibleNavId" aria-controls="collapsibleNavId"
                aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
                <button class="btn btn-dark" v-if="cart.length > 0" v-on:click='showCheckout'> {{cart.length}} <span
                    class="fas fa-cart-plus"></span>Cart
                </button> <br>
                <div class="collapse navbar-collapse" id="collapsibleNavId">
                    <ul class="navbar-nav mr-auto mt-2 mt-lg-0">
                        <li class="nav-item">
                            <button class="btn btn-primary" v-on:click="type = 'subject'">Subject</button>
                        </li>
                        <br>
                        <li class="nav-item">
                            <button class="btn btn-primary" v-on:click="type = 'price'">Price</button>
                        </li>
                        <br>
                        <li class="nav-item">
                            <button class="btn btn-primary" v-on:click="type = 'Location'">Location</button>
                        </li>
                        <br>
                        <li class="nav-item dropdown">
                            <select class="form-select form-select-lg mb-3" aria-label=".form-select-lg example" v-model="sort" id="sortby">
                                <option value="ascending">Ascending</option>

                                <option value="descending">Descending</option>
                            </select>
                        </li>
                    </ul>
                    <form class="form-inline my-2 my-lg-0">
                        <div class="search-wrapper">
                            <input v-on:input="filteredLessons" class="form-control mr-sm-2" type="text" v-model="search" placeholder="Search Subject or Location.."/>
                            </div>
                    </form>
                </div>
            </nav>

                
        </header>
    

        <div class="root">
 
            <main>
                <div v-if='!showProduct' class="row">
                    
                    <Lesson :sortedLessons="sortedLessons" @canAddToCart="canAddToCart" />
                </div>
                
                <div v-else class="row">
                   <Checkout :cart="cart" @removeFromCart="removeFromCart" @submit="submit" @showCheckout="showCheckout"/>
                </div>
            </main>
        </div>
  </main>
</template>
