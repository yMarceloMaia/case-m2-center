<template>
    <div class="main-container">
    <h2>Internet</h2>
    
    <p class="description-text">Selecione um plano de internet para continuar</p>

    <div class="card-product">   
        <div class="product" @click="addToCartInternetProduct(internetProducts)" v-for="(internetProducts) in internetProducts" :key="internetProducts.id">
            <p class="name-product"><b>{{internetProducts.name}}</b></p>
            <p><b>R${{internetProducts.price}},00</b></p>
            <p>+ Detalhes</p>
        </div>
    </div>

    <h2>TV</h2>
    <p class="description-text">Agora selecione seu pacote de telefone fixo</p>

    <div class="card-product">
        <div class="product" @click="addToCartTvProduct(tvProducts)" v-for="(tvProducts) in tvProducts" :key="tvProducts.id">
            <p class="name-product"><b>{{tvProducts.name}}</b></p>
            <p><b>R${{tvProducts.price}},00</b></p>
            <p>+ Detalhes</p>
        </div>
    </div>

    <h2>Fixo</h2>
    <p class="description-text">Agora selecione seu plano de tv</p>

    <div class="card-product">
        <div class="product" @click="addToCartFixoProduct(fixoProducts)" v-for="(fixoProducts) in fixoProducts" :key="fixoProducts.id">
            <p class="name-product"><b>{{fixoProducts.name}}</b></p> 
            <p><b>R${{fixoProducts.price}},00</b></p>
            <p>+ Detalhes</p>
        </div>
    </div>

    <hr/>
    <div class="text-install">
        <p class="description-text">Taxa de instalação</p>
        <p class="description-text">Grátis</p>
    </div>
    <div class="text-install">
        <p>Total</p>
        <!-- {{this.renderCart.length == 0? "" : this.renderCart}} -->
        <p>R${{priceToPay}},00</p>
    </div>
    <button>Continuar</button>
   </div>
</template>
<script>
import axios from "axios"

export default {
    name: "HomePage",
    data(){
        return {
            product: [],
            tvProducts: [],
            fixoProducts: [],
            internetProducts: [],
            colorTexto: "#1155ba",
            controlColor: true,
            colorBackgroundCard: this.controlColor == true ? "#000" : "#9a53c2",
            cart: [],
            renderCart: [],
            priceToPay: 0
        }
    },
     async mounted(){ 
        await axios.get("http://localhost:3000/products")
        .then((res) => {
           this.product = res.data
        })
        .catch((error) => {
            console.log("error", error)
        })
        this.fixoProducts = this.product.filter((p) => {return p.category_id === 1})
        this.tvProducts = this.product.filter((p) => {return p.category_id === 2})
        this.internetProducts = this.product.filter((p) => {return p.category_id === 3})
    },
    methods: {
        addToCartInternetProduct(prod){
            this.cart = [prod]
            console.log(this.cart)
            this.renderCartScreen()
        },
        addToCartTvProduct(prod){
            let newCart = [...this.cart]
            const index = this.cart.findIndex((i) => i.category_id === prod.category_id && i.id !== prod.id)
            if (index === -1 && this.cart.length === 1) {
                newCart = [...this.cart, prod]
                // console.log("aaaa")
                this.cart = newCart
            }else if(index === 1 && this.cart.length === 2){
                let delCart = newCart.slice(0,1)
                let cartNew = [...delCart, prod]
                console.log("bbbb", delCart)
                this.cart = cartNew
            }
            // console.log("cccc", this.cart)
            this.renderCartScreen()
        },
        addToCartFixoProduct(prod){
            const index = this.cart.findIndex((i) => i.category_id === prod.category_id && i.id !== prod.id)
            if (index === -1 && this.cart.length === 2){
                let newCart = [...this.cart, prod]
                this.cart = newCart
            }
            // console.log(this.cart)
            // console.log(this.renderCart)
            this.renderCartScreen()
            this.onChangeValue()
        },
        renderCartScreen(){
            this.renderCart = this.cart && this.cart.map((prod) => {
                 let divvv = 
                    <div class="product">
                        <p class="name-product"><b>{prod.name}</b></p>
                        <p><b>R${prod.price},00</b></p>
                        <p>+ Detalhes</p>
                    </div>
                return divvv
                
            })
            console.log("aaabvbvx", this.renderCart)
        },
        onChangeValue() {
            if(this.cart.length === 3){
                this.cart.forEach((prod) => {
                this.priceToPay += Number(prod.price);
                console.log("vvv", this.priceToPay)
                })
            }
                
        }
    }
}
</script>

<style scoped>
    .main-container{
        display: flex;
        flex-direction: column;
        justify-content: center;
        max-width: 500px;
        margin: auto;
        padding: 10px;
    }
    .description-text{
        color: #666666;
    }
    h2 {
        color: #9a53c2;
    }
    form{
        display: flex;
        flex-direction: column;
    }
    form input{
        width: 50%;
    }
    form button{
        width: 50%;
    }
    .card-product {
        display: flex;
        margin: 20px;
    }
    .card-product div{
        /* background-color: v-bind(colorBackgroundCard); */
        /* border: 5px solid v-bind(colorBackgroundCard); */
        border: 1px solid black;
        color: v-bind(colorTexto);
        margin: 5px;
        padding: 5px;
        width: 100px;
        height: 100px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }
    .card-product div:hover{
        cursor: pointer;
    }
    .name-product{
        font-size: 15px;
    }
    .text-install{
        display: flex;
        justify-content: space-between;
    }
    button{
        width: 50%;
        margin: auto;
        height: 30px;
        border-radius: 0px 10px 0px 10px;
        background-color: #9a53c2;
        color: white;
        font-size: 17px;
    }
</style>
