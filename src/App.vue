<template>
  <div id="app">
    <div class="container" style="width: 1500px">
			<!-- <section class="navbar main-menu" >
				<div class="navbar-inner main-menu"style="width: 1500px">
					<a href="/" class="logo pull-left"><img src="./assets/oln logo.png" class="site_logo" alt="" style="margin: 15px auto 5px auto"></a>
					<nav  class="pull-right" style="margin: 5px auto 0px auto">
						<h1>Welcome</h1>
					</nav>
				</div>
			</section> -->
			<div class="" style="margin-top:20px">
			<img class="pageBanner" src="https://cdn.shopify.com/s/files/1/0214/5470/files/OLN-SLIDER-1.png?8247432977261988465"  >
			</div>
      <div class="" style="margin-top:20px">
        <h1>Welcome</h1> <br>
			<!-- <img class="imgEdit" src="https://scontent.fbkk21-1.fna.fbcdn.net/v/t1.0-9/27459244_2155848374701581_4375726229725598849_n.jpg?_nc_cat=0&oh=4517b2c5d3326fce2338f435696b7ac4&oe=5B8802CE"  > -->

      <hr>
      </div>
				<div class="row" style="margin-top:30px">
					<div class="col-9" >
						<ul >
							<li class="col-3" v-for="product in productFire" :key="product['.key']">
									<a ><img :src="product.imgLink" width="300" height="255"></a><br/>
                    {{product.productName}}<br/>
  									<p class="price"><h5>{{product.price}} ฿</h5> </p>
                    {{product.amount}} left <br>
                    <button type="button" class="btn btn-outline-primary" v-if="adminF == false && product.amount !==0 && loginState== true"
                    @click="addToCart(product.productName,product.price,product['.key'],product.amount,product.imgLink)">Add to cart</button>
                    <button type="button" class="btn btn-outline-warning" v-if="adminF && loginState== true"
                    @click="editProduct(product.productName,product.price,product.amount,product.imgLink,product['.key'])">Edit</button>
                    <button type="button" class="btn btn-outline-danger" v-if="adminF && loginState== true"
                    @click="deleteProduct(product['.key'])">Delete</button>
              </li>
						</ul>
						<hr>
					</div>
          <!-- login box -->
          <div class="col-3" style="text-align: center" v-if="loginState == false && registerState == false" >
            <h2>Login</h2>
            <div class="col">
              <input type="text" class="form-control" placeholder="Email" v-model="email" >
              <br>
            </div>
            <div class="col">
              <input type="password" class="form-control" placeholder="Password" v-model="passEmail" >
            </div>
            <br>
            <button type="button" class="btn btn-outline-success" @click="checkUserLogin()">Login</button>
            <button type="button" class="btn btn-outline-warning" @click="changeStateRe()">Register</button>
                <!-- <button @click="removeTodoFire(todo['.key'])">X</button> -->
          </div>
          <!-- login box -->
          <!-- register box -->
          <div class="col-3" style="text-align: center" v-if="registerState == true" >
            <h2>Register</h2>
            <div class="col">
              <input type="text" class="form-control" placeholder="Username" v-model="userNameRe" >
            </div>
            <br>
            <div class="col">
              <input type="text" class="form-control" placeholder="Email" v-model="emailRe" >
              <br>
            </div>
            <div class="col">
              <input type="password" class="form-control" placeholder="Password" v-model="passEmailRe" >
            </div>
            <br>

            <button type="button" class="btn btn-outline-primary" @click="registerUser">Register</button>
            <button type="button" class="btn btn-outline-danger" @click="changeStateRe">Cancel</button>
                <!-- <button @click="removeTodoFire(todo['.key'])">X</button> -->
          </div>
          <!-- register box -->
          <!-- Your cart -->
          <div class="col-3" style="text-align: center" v-if="loginState== true && adminF == false" >
            <h2>{{userName}}</h2>
            Your Cart <br><br>
             <div v-for="(item , index) in items">
               <ul>
                 <li class="list-group-item" style="width:350px; margin-bottom: 10px; " ><img :src="item.imgLink" width="50" height="50"> {{item.productName}} : {{item.price}} ฿ <br> {{item.amount}}<br>
                   <button class="btn btn-danger" style="margin-bottom: 20px; margin-top: 20px" @click="pickOff(item.productName,item.price,item.amount,item.imgLink,item.key,index,item.amountf)">Bring this off</button></li>
               </ul>
             </div>

            All of these : <strong>{{cost}} ฿ </strong><br>
            <button type="button" class="btn btn-outline-success" @click="checkOut">Check out</button>
            <button type="button" class="btn btn-outline-danger" @click="logout">Logout</button>
          </div>
          <!-- Your cart -->
          <!-- Admin -->
          <div class="col-3" style="text-align: center" v-if="adminF" >
            <h2>{{userName}}</h2>

            <button type="button" class="btn btn-outline-success" @click="addProductStateChange">Add product</button>
            <button type="button" class="btn btn-outline-danger" @click="logout">Logout</button>
            <div class="" v-if="addProductState" >
              <div>
                <br> <strong>Add product</strong> <br>
              </div>

                <div class="col">
                  <input type="text" class="form-control" placeholder="Name" v-model="n" >
                </div><br>
                <div class="col">
                  <input type="number" class="form-control" min="1" placeholder="Price" v-model="p" >
                </div><br>
                <div class="col">
                  <input type="number" class="form-control" min="1" placeholder="Amount" v-model="a" >
                </div><br>
                <div class="col">
                  <input type="text" class="form-control" placeholder="Image link" v-model="l" >
                </div><br>
                <button class="btn btn-success" @click="addProduct"> Add </button>
                <button class="btn btn-danger" @click="addProductState = false"> Cancel </button>

            </div>
            <div class="" v-if="editState" >
              <div>
                <br> <strong>Edit product</strong> <br>
              </div>

                <div class="col">
                  <input type="text" class="form-control" placeholder="Name" v-model="nEdit" >
                </div><br>
                <div class="col">
                  <input type="number" class="form-control" min="1" placeholder="Price" v-model="pEdit" >
                </div><br>
                <div class="col">
                  <input type="number" class="form-control" min="1" placeholder="Amount" v-model="aEdit" >
                </div><br>
                <div class="col">
                  <input type="text" class="form-control" placeholder="Image link" v-model="lEdit" >
                </div><br>
                <button class="btn btn-warning" @click="submitEditProduct"> Edit </button>
                <button class="btn btn-danger" @click="editState = false"> Cancel </button>

            </div>
                <!-- <button @click="removeTodoFire(todo['.key'])">X</button> -->
          </div>
          <!-- Admin -->
				 </div>
		</div>
		</div>
    <!-- firebase -->
    <!-- <ul>
      <li v-for="todo in todosFire" :key="todo['.key']">
        <input type="text" :value="todo.text" @input="updateTodoFire(todo['.key'], $event.target.value)">
        <button @click="removeTodoFire(todo['.key'])">X</button>
      </li>
    </ul> -->
</template>
<script>
import firebase from 'firebase'
import swal from 'sweetalert2'

var config = {
  apiKey: 'AIzaSyBPDmTmUaMfH0l666pnCk3BElbpdVh47o0',
  authDomain: 'onlineshop-best.firebaseapp.com',
  databaseURL: 'https://onlineshop-best.firebaseio.com',
  projectId: 'onlineshop-best',
  storageBucket: 'onlineshop-best.appspot.com',
  messagingSenderId: '307885241189'
}
var firebaseApp = firebase.initializeApp(config)
var db = firebaseApp.database()
var todosRef = db.ref('todos')
var UserRef = db.ref('users')
var productRef = db.ref('products')
export default {
  name: 'App',
  firebase: {
    todosFire: todosRef,
    productFire: productRef,
    userFire: UserRef
  },
  data () {
    return {
      text: '',
      productName: '',
      price: '',
      addProductButtonState: false,
      inputPassword: '',
      authorized: false,
      onAdd: false,
      p: null,
      n: null,
      l: null,
      a: null,
      pEdit: null,
      nEdit: null,
      lEdit: null,
      aEdit: null,
      keyEdit: null,
      emailRe: '',
      passEmailRe: '',
      userNameRe: '',
      email: '',
      passEmail: '',
      userName: '',
      adminF: false,
      cost: 0,
      startA: 1,
      loginState: false,
      registerState: false,
      addProductState: false,
      editState: false,
      items: []
    }
  },
  methods: {
    addProductStateChange () {
      this.addProductState = true
      this.editState = false
    },
    submitEditProduct () {
      swal({
        title: 'Are you sure?',
        text: "After this you can edit anytime",
        type: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#ff9900',
        cancelButtonColor: '#adad85',
        confirmButtonText: 'Yes, edit it!'
      }).then((result) => {
        if (result.value) {
          db.ref('products/' + this.keyEdit).set({
            productName: this.nEdit,
            price: this.pEdit,
            imgLink: this.lEdit,
            amount: this.aEdit
          })
          this.nEdit = null
          this.pEdit = null
          this.aEdit = null
          this.lEdit = null
          this.keyEdit = null
          this.editState = false
          swal(
            'Edited!',
            'Your product has been edited.',
            'success'
          )
        }
      })
    },
    editProduct (name,price,amount,link,key) {
      this.editState = true
      this.addProductState = false
      this.nEdit = name
      this.pEdit = price
      this.aEdit = amount
      this.lEdit = link
      this.keyEdit = key
      window.scrollTo(450, 450)
    },
    deleteProduct (key) {
      swal({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        type: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#d33',
        cancelButtonColor: '#adad85',
        confirmButtonText: 'Yes, delete it!'
      }).then((result) => {
        if (result.value) {
          db.ref('products/' + key).remove()
          swal(
            'Deleted!',
            'Your product has been deleted.',
            'success'
          )
        }
      })
    },
    logout () {
      this.loginState = false
      this.email = ''
      this.userName = ''
      this.passEmail = ''
      this.adminFunction = false
    },
    checkUserLogin () {
      for (var i = 0; i < this.userFire.length; i++) {
        if (this.email == this.userFire[i].email && this.passEmail == this.userFire[i].password) {
          this.loginState = true
          this.userName = this.userFire[i].username
          this.adminF = this.userFire[i].adminFunction
          if (this.userFire[i].adminFunction) {
            swal({
              position: 'center',
              type: 'success',
              title: 'Welcome back Admin',
              showConfirmButton: false,
              timer: 1500
            })
          } else {
            swal({
              position: 'center',
              type: 'success',
              title: 'Welcome :)',
              showConfirmButton: false,
              timer: 1500
            })
          }
        }
      }
      if(this.loginState == false) {
        swal(
          'Warning!',
          'Invalid email or password.',
          'warning'
          )
      }
    },
    registerUser () {
      var pattMail = /.+@\w+\..+/g
      var pattPass = /.{6}/g
      var mailMatch = pattMail.test(this.emailRe)
      var passMatch = pattPass.test(this.passEmailRe)
      if (mailMatch) {
        if (passMatch) {
          UserRef.push({
            email: this.emailRe,
            password: this.passEmailRe,
            username: this.userNameRe,
            adminFunction: false
          })
          swal(
            'Thank you!',
            'Registeration successed',
            'success'
            )
          this.emailRe = ''
          this.passEmailRe = ''
          this.registerState = false
          } else {
            swal(
              'Warning!',
              'Please enter at least 6 characters for password.',
              'warning'
              )
            }

        } else {
          swal(
            'Warning!',
            'Please enter the correct email!',
            'warning'
            )
        }
    },
    changeStateRe () {
      if (this.registerState == true) {
        this.registerState = false
      } else {
        this.registerState = true
      }
    },
    changePassword () {
      this.inputPassword = ''
    },
    pickOff (name,price,amount,link,key,index,amountf) {
      console.log(amountf)
      db.ref('products/' + key).set({
        productName: name,
        price: price,
        imgLink: link,
        amount: amountf
      })
      this.cost-=price * amount
      this.items.splice(index, 1)
    },
    checkOut () {
      swal({
        title: 'Are you sure?',
        text: "You won't be able to refund!",
        type: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#33cc33',
        cancelButtonColor: '#adad85',
        confirmButtonText: 'Yes, I buy all of these!'
      }).then((result) => {
        if (result.value) {
          this.items = []
          this.cost = 0
          swal(
            'Thank you!',
            'come back again :)',
            'success'
            )
        }
      })
    },
    addToCart (name,price,key,amount,link) {
      console.log(amount)
      if (this.items[0] == null) {
        this.cost += price * 1
        this.items.push({
          productName: name,
          price: price,
          key: key,
          amount: 1,
          imgLink: link,
          amountf: amount})

          db.ref('products/' + key).set({
            productName: name,
            price: price,
            imgLink: link,
            amount: amount - 1
          })
      } else {
        for (var i = 0; i < this.items.length; i++) {
          if (name==this.items[i].productName) {
            this.cost += price * 1
            db.ref('products/' + key).set({
              productName: name,
              price: price,
              imgLink: link,
              amount: amount - 1
            })
            this.items[i].push({
              productName: name,
              price: price,
              key: key,
              amount: this.items[i].amount += 1,
              imgLink: link,
              amountf: amount})


          } else if (i == this.items.length - 1 && name !== this.items[i].productName) {
            this.cost += price * 1
            db.ref('products/' + key).set({
              productName: name,
              price: price,
              imgLink: link,
              amount: amount - 1
            })
            this.items.push({
              productName: name,
              price: price,
              key: key,
              amount: 1,
              imgLink: link,
              amountf: amount})
              break
          }
        }
      }
    },
    addProduct () {
      if (this.n!==null||this.p!==null||this.a!==null||this.l!==null) {
        if (this.n!==null) {
          if (this.p!==null&&this.p>=0) {
            if (this.a!==null&&this.a>0) {
              if (this.l!==null) {
                productRef.push({
                  productName: this.n,
                  price: this.p,
                  imgLink: this.l,
                  amount: this.a
                })
                this.n = null
                this.l = null
                this.p = null
                this.a = null
              }else {
                swal(
                  'Warning!',
                  'Please fill in image\'s link of product!',
                  'warning'
                  )
              }
            }else {
              swal(
                'Warning!',
                'Please fill in the correct amount of product!',
                'warning'
                )
            }
          }else {
            swal(
              'Warning!',
              'Please fill in the correct price of product!',
              'warning'
              )
          }
        }else {
          swal(
            'Warning!',
            'Please fill in name of product!',
            'warning'
            )
        }
      }else {
        swal(
          'Warning!',
          'Please fill up the information!',
          'warning'
          )
      }
    },
    addTodoFire () {
      todosRef.push({
        text: 'wsdsd',
        productName: 'Coca col',
        price: 125

      })
    },
    updateTodoFire (key, text) {
      // todosRef.child(key).child('text').set(text)
      db.ref('todos/' + key).set({
        text: text
      })
    },
    removeTodoFire (key) {
      // todosRef.child(key).remove()
      db.ref('todos/' + key).remove()
    },
    changeStateButton () {
      this.addProductButtonState = true
    },
    changeStatePassword () {
      this.onAdd = true
    },
    updateTodo (key, text) {
      // const index = this.todos.findIndex((todo) => todo.key === key)
      // if (index > -1) {
      //   this.todos[index].text = text
      // }
      const todo = this.todos.find((todo) => todo.key === key)
      if (todo) {
        todo.text = text
      }
    },
    removeTodo (key) {
      // const index = this.todos.findIndex(function (todo) {
      //   return todo.key === key
      // })
      const index = this.todos.findIndex((todo) => todo.key === key)
      if (index > -1) {
        this.todos.splice(index, 1)
      }
    }
  }
}
</script>

<style>
  #app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: -5px;
  }
  ul {
    margin: 0 auto;
    text-align: center;
  }

  li {
    display: inline-block;
    vertical-align: top;
    margin-bottom: 10px
  }
  .imgEdit {
    border-radius: 50%;
    width: 300px;
    height: 300px;
  }
  h1 {
     font-size: 50px;
     color: black;
     text-align: center;
     text-shadow: 0px 1px 0px #f2f2f2;
     border: 1px solid transparent;
   }
  h2 {
     font-size: 35px;
     color: black;
     text-align: center;
     margin: 0 0 35px 0;
     text-shadow: 0px 1px 0px #f2f2f2;
   }
 h5 {
      font-size: 20px;
      color: black;
      text-align: center;
      font-weight: bold;
      text-shadow: 0px 1px 0px #f2f2f2;
    }
  .borderlist {
    list-style-position:inside;
    border: 1px solid black;
  }
</style>
