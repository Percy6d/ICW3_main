<script>
export default {
    data(){
        return{
            user:{
            name:'',
            number:''
            }
        }
    },
    props:['cart'],
    methods:{
        removeFromCart(lesson){
            this.$emit('removeFromCart',lesson)
        },
        submit(name,phone){
            this.$emit('submit',name,phone)
        },
        showCheckout(){
            this.$emit('showCheckout');
        }
    }
}
</script>
<template>
     <div class="container-fluid p-5">
                        <!-- calculation-->
                        <div class="title-bar">
                            <button class="btn btn-dark" v-on:click="showCheckout"><span class="fas fa-arrow-left"></span> Go Back </button>
                            
                        </div>
                        <!-- cart-->
                       
                                    <h4 class="card-title">Cart Items</h4>
                                    <div class="lessons__item col-md-3 py-2">
                                <div class="cart">
                                    <div class="cart__item">
                                        <div class="cart__item" v-for="(item, index) in cart" :key="`cart--${index}`" >
                                            <div class="card-img-top" ><img :src="item.img" alt="" width="200" height="200"></div>
                                            <div class="card-text">Subject: {{item.subject}}</div>
                                            <div class="card-text">Location: {{item.location}}</div>
                                            <div class="card-text">Price: {{item.price}}</div>
                                            <div><button class="btn btn-danger fa fa-minus-square" v-on:click="removeFromCart(item)"
                                                        aria-hidden="true">Remove</button></div>
                                        </div>
                                    </div>
                                    <hr style="width: 100%">
                                    
                                    <div v-if="!cart.length > 0" class="cart__item"
                                        style="background-color: transparent; padding: 20px;">
                                        <div class="card-text" style="text-align: center; color: grey; font-size: 20px; font-weight: bold;">
                                        List Empty</div>
                                    </div>
                                </div>
                    </div>

                        
                        <hr>

                        

                        <div>
                            
                            <div class="float-left">
                                <div class="card-deck">
                                <div class="card">
                                    <div class="card-body">
                                            <h3 style="color: black;" class="card-title">Details</h3>
                                            <div>
                                                <label style="color: black;" for="name">Name</label>
                                                <input class="form-control" v-model="user.name" type="text" required>
                                            </div>
                                            <div>
                                                <label style="color: black;" for="name">Phone Number</label>
                                                <input class="form-control" v-model="user.number" type="phone" required>
                                            </div>
                                            <br>
                                            <input class="btn btn-success" type="submit"  v-on:click="submit(user.name,user.phone)" value="Checkout">
                                            <!-- <button v-if="letterRegex && numberRegex" v-on:click="submitCheckout"> Checkout </button> -->
                                    </div>
                                </div>
                                </div>
                            </div> 
                            
                            </div>
                        </div>
</template>