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

          <b-button v-if="!checkIsAddCart(product.id)" @click="add(product)" href="#"  variant="primary"> Thêm vào giỏ hàng </b-button>
          <b-button v-else @click="add(product)"  disabled href="#" block variant="warning">Sản phẩm đã được thêm vào giỏ hàng</b-button>
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
             <td style="display:flex;margin-left: 30%;">
               
                <button @click="decrement(item.id)" class="btn btn-info">-</button> 
                 <h4 style="width:20%;">{{item.quantity}}</h4> 
                <button @click="increment(item.id)" class="btn btn-info">+</button>
              
              </td>
             <td>{{item.price}}</td>
             <td><a href="javascript:;" @click="remove(item.id)" class="btn btn-danger"> X </a></td>
           </tr>
         </table>
        </b-col>
      </b-row>

      <b-row v-if="cart.length >0">
        <b-col></b-col>
        <b-col cols="4"></b-col>
        <b-col></b-col>
        <b-col>
          <b-button @click="buy" variant="success" block class="mr-2" style="margin: 5%;">
            Mua Hàng
          </b-button>

        </b-col>
      </b-row>
      <b-modal  ref="modal-1" id="modal-1" certered title="Purchase Completed" hide-footer>
        <p>Sản phẩm: </p>
        <ul v-for="productFinal in cart" :key="productFinal.id">
          <hr>
          <li> 
            Tên Sản phẩm :{{productFinal.name}}
          </li>
          <li>
            quantity: {{productFinal.quantity}}
          </li>
          <li>
            Price: {{productFinal.price}}
          </li>
          <li>
            Total:{{productFinal.price * productFinal.quantity}}
          </li>
        </ul>
         <hr>
        <h2>Total: ${{total}}.00</h2>
        <b-button class="mt-2" variant="outline-warning" block @click="toggleModal">Thanh toán</b-button>
        <hr>
        <b-button style="width:auto; margin-left: 90%; " block @click="$bvModal.hide('modal-1')">Hủy</b-button>
       
      </b-modal>
      <b-modal ref="modal-3" certered title="Login" hide-footer>
        
      <div class="form-group" @submit="onSubmit" @reset="onReset" >
        <label style="font-size: 12px;height: 15px;line-height: 15px;overflow: hidden;color: #424242;display: block;margin-bottom: 5px;">Phone Number or Email</label>
        <input
          id="input-1"
          v-model="form.text"
          type="text"
          required
          placeholder="UserName"
          style="margin-bottom: 2%;"
          class="form-control"
        />
        <label style="font-size: 12px;height: 15px;line-height: 15px;overflow: hidden;color: #424242;display: block;margin-bottom: 5px;">Password</label>
        <input
          id="input-1"
          v-model="form.password"
          type="password"
          required
          style="margin-bottom:5%"
          placeholder="Password"
          class="form-control"
        />
        <b-button type="submit" variant="primary" style="margin-left: 60%;">Đăng Nhập</b-button>
        <b-button class="ml-2" @click="exit" >Đóng</b-button>
      </div>
      </b-modal>
        <template >
          <a  href="javascript:;"  class="btn btn-danger" style="margin-right: 85%;" variant="info" block @click="clean" v-if="cart.length > 0"> Xóa tất cả </a>
          
        </template>
        <h2 class="my-4"> Total: ${{total}}.00</h2>
    </b-container>
  </div>
</template>
<script>
export default {
  name:'Cart',
  data(){
    return{
      form: {
          username:"",
          password:"",
        },
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
    exit(){
      this.$refs['modal-3'].hide(),
        this.$refs['modal-1'].hide()
    },
    toggleModal() {
        // We pass the ID of the button that we want to return focus to
        // when the modal has hidden
        this.$refs['modal-3'].show(),
        this.$refs['modal-1'].hide()
      },
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
      
      // 
    },
    buy(){
      this.ticket={
        product : this.cart,
        total : this.total
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
      this.cart = this.cart.map(item=>{
       if(item.id == id){
         if(item.quantity != 1)
         item.quantity --;
       }
       return item;
     })
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
  onSubmit(evt) {
        evt.preventDefault()
        alert(JSON.stringify(this.form))
      },
  watch:{
    counter(value){

    }
  }
}
</script>
<style scoped>

</style>