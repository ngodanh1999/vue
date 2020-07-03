<template>
  <div>
    <b-container>
      <b-row>
        <b-col>
          <h2>Products</h2>
        </b-col>
      </b-row>
      <b-row>
        <b-col v-for="product in products" :key="product.id">
          <!--Tag :img-src="" fllow of path index.js-->
          <b-card
          :title= 'product.name'
          :img-src="'/static/assets/fruit'+product.id+'.jpeg'"
          img-alt="Image"
          img-top
          tag="article"
          style="max-width:20rem"
          class="mb-2"
          >
          <b-card-text>
            Price: {{product.price}}
          </b-card-text>

          <b-button v-if="!checkIsAddCart(product.id)" @click="add(product)" href="#" variant="primary"> Add to Shopping Cart </b-button>
          <b-button v-else @click="add(product)" :disabled="product.cart" href="#" variant="warning">Sản phẩm đã được thêm vào giỏ hàng</b-button>
          </b-card>
        
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <h2>Giỏ Hàng</h2>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
         <table style="width:100%">
           <tr>
             <th>STT</th>
             <th>Name</th>
             <th>quantity</th>
             <th>Price</th>
             <th>Remove</th>
           </tr>
           <tr v-for="(item,index) in cart" :key="index">
             <td>{{index+1}}</td>
             <td>{{item.name}}</td>
             <td><button @click="increment(item.id)">+</button> {{item.quantity}} </td>
             <td>{{item.price}}</td>
             <td><a href="javascript:;" @click="remove(item.id)">Remove</a></td>
           </tr>
         </table>
        </b-col>
      </b-row>

      <b-row v-if="cart.length >0">
        <b-col></b-col>
        <b-col cols="4">Youtube Channel</b-col>
        <b-col></b-col>
        <b-col>
          <b-button @click="buy" variant="success" block class="mr-2">
            Mua
          </b-button>

        </b-col>
      </b-row>
      <b-modal hide-header-close no-close-on-esc no-close-on-backdrop ref="modal-1" certered title="Purchase Completed"/>
        <template slot="modal-footer">
          <b-button class="mt-3" variant="info" block @click="clean"> Đóng </b-button>

        </template>
      
        <ul v-for="productFinal in ticket.products" :key="productFinal.id">
          <li> 
            Sản phẩm :{{productFinal.name}}
          </li>
          <li>
            quantity: {{productFinal.quantity}}
          </li>
          <li>
            Price: {{productFinal.price}}
          </li>
          <li>
            Total:{{productFinal.price = productFinal.quantity}}
          </li>
          <hr>
        
        </ul>
        <h2 class="my-4"> Total: ${{total}}.00</h2>

    </b-container>
  </div>
</template>
<script>
export default {
  name:'Cart',
  data(){
    return{
      ticket:{
        products: null,
        total:0
      },
      
      counter:0,
      products:[
      {
        id:1,
        img:'@/assets/fruit1.jpeg',
        name:'Apple',
        price:3,
        cart:false,
        quantity:1
      },
      {
        id:2,
        img:'@/assets/fruit2.jpeg',
        name:'Tao',
        price:4,
        cart:false,
        quantity:1
      }
      ,{
        id:3,
        img:'@/assets/fruit3.jpeg',
        name:'Chuoi',
        price:3,
        cart:false,
        quantity:1
      }
      ],
      cart:[],
      fields:['#','remove', 'image', 'name', 'quantity','price']
    }
  },
  methods:{
    add(product){
      product.quantity =1;
      this.cart.push(product)
    },
    clean(){
      this.cart=[];
      for(const key in this.products)
      {
        this.products[key].cart=false
        this.products[key].quantity=1
      }
      this.$refs['modal-1'].hide()
    },
    remove(id){
      this.cart = this.cart.filter(item => item.id != id);
    },
    buy(){
      this.ticket={
        product:this.cart,
        total:this.total
      }
      this.$refs['modal-1'].show()
    },
    increment(id){
     this.cart = this.cart.map(item=>{
       if(item.id == id){
         item.quantity ++;
       }
       return item;
     })
    },
    decrement(id){
      for(let index=0; index < this.cart.length; index++){
        if(this.cart[index].id == id){
          this.cart[index].quantity--
        }
      }
    },
    checkIsAddCart(id){
      return this.cart.find(item=>item.id == id) != null
    }
  },
  computed:{
    total(){
      let t=0;
      for(let item of this.cart){
        t += item.price*item.quantity
        // console.log(t)
      }
      return t
    }
  },
  watch:{
    counter(value){

    }
  }
}
</script>
<style scoped>

</style>