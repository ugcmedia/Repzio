<template>
  <div v-if="loaded" class="center">
    <div class="inline" v-for="(item, index) in items" :key="index">
      <div @click="show_modal(item)" class="card_container">
        <img :src="item.PhotoName" alt="" class="photo_name" />
        <div class="item_name">
          {{ item.ItemName }}
        </div>
        <div class="base_price">${{ item.BasePrice }}</div>
        <div class="center">
          <button class="shop_button">
            Shop Now
          </button>
        </div>
      </div>
      <div></div>
    </div>
    <div
      v-if="clicked_product"
      id="myModal"
      @click="exit_product_modal"
      class="modal"
    >
      <div>
        <span @click="hide_modal" class="close">&times;</span>
      </div>
      <div class="modal-content">
        <div class="product_container">
          <img
            :src="modal.PhotoName"
            alt="Product"
            class="product_photo_name"
          />
          <div>
            <button class="count_btn" @click="increment">+</button>
            <button class="count">{{ count }}</button>
            <button class="count_btn" @click="decrement">-</button>
            <button class="base_price_modal">${{ modal.BasePrice }}</button>
          </div>
          <div class="item_name_modal">
            <div>{{ modal.itemName }}</div>
            <div>
              <p class="label inline">ID:</p>
              <p class="description inline">{{ modal.ItemID }}</p>
            </div>
            <div>
              <p class="label inline">Dimensions:</p>
              <p class="description inline">{{ modal.Dimensions }}</p>
            </div>
            <div class="block">
              <p class="description">
                {{ modal.Description }}
              </p>
            </div>
          </div>
          <div class="center footer">
            <button @click="show_contact_modal" class="contact_button inline">
              <i class="fa fa-phone fa-2x"
                >&nbsp; <span class="footer_btn">Contact me</span>
              </i>
            </button>
            <button class="cart_button inline">
              <i class="fa fa-cart-arrow-down fa-2x"
                ><span class="footer_btn">Add to Cart</span></i
              >
            </button>
          </div>
        </div>
      </div>
    </div>
    <div
      v-if="clicked_contact"
      id="contact"
      @click="exit_contact_modal"
      class="modal"
    >
      <div>
        <span @click="hide_contact_modal" class="close">&times;</span>
      </div>
      <div class="modal-content">
        <div class="contact_container">
          <div class="header">
            <div class="contact_logo">
              <img
                :src="
                  'http://images.repzio.com/productimages/' +
                    logo +
                    '/logo' +
                    logo +
                    '_lg.jpg'
                "
                alt="logo"
                height="50"
                width="70"
              />
            </div>
            <div>
              <p class="sales_rep">
                Sales Rep
              </p>
            </div>
          </div>
          <div class="sales_div">
            <img
              src="../assets/images/salesrep.jpg"
              alt=""
              class="sales_rep_image"
            />
          </div>
          <div class="info">
            <div class="text">{{ info.firstname }},{{ info.lastname }}</div>
            <div class="text">{{ info.city }},{{ info.state }}</div>
          </div>
          <div class="contact_info">
            <div class="contact_me">Email or call me direct</div>
            <div>
              <i class="fa fa-envelope fa-lg inline"
                >&nbsp;<span class="font">Email</span>:</i
              >
              <p class="inline email">{{ info.email }}</p>
            </div>
            <div>
              <i class="fa fa-phone fa-lg inline"
                >&nbsp;<span class="font">Phone</span>:</i
              >
              <p class="inline phone">{{ info.phone }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import JsonData from "../assets/db/db.json";
export default {
  name: "ProductCard",
  data() {
    return {
      json: JsonData,
      count: 1,
      logo: null,
      itmes: [],
      loaded: false,
      info: {
        firstname: "",
        lastname: "",
        city: "",
        state: "",
        email: "",
        phone: ""
      },
      clicked_product: false,
      clicked_contact: false,
      modal: {}
    };
  },
  // props: {
  //   photoName: {
  //     type: String,
  //     default: ""
  //   },
  //   itemName: {
  //     type: String,
  //     default: ""
  //   },
  //   basePrice: {
  //     type: Number,
  //     default: null
  //   },
  //   itemId: {
  //     type: String,
  //     default: ""
  //   },
  //   itemDimensions: {
  //     type: String,
  //     default: ""
  //   },
  //   itemDescription: {
  //     type: String,
  //     default: ""
  //   },
  //   contactInfo: {
  //     type: Object,
  //     default: () => ({})
  //   }
  // },
  mounted() {
    this.info.firstname = this.json.SalesRep.FirstName;
    this.info.lastname = this.json.SalesRep.LastName;
    this.info.city = this.json.SalesRep.City;
    this.info.state = this.json.SalesRep.State;
    this.info.email = this.json.SalesRep.EmailAddress;
    this.info.phone = this.json.SalesRep.Phone;
    this.items = this.json.items;
    setTimeout(() => {
      this.loaded = true;
    }, 300);
    this.logo = this.items[0].ManufacturerID;
  },
  computed: {
    url(name) {
      return (
        "http://images.repzio.com/productimages/" +
        name +
        "/logo" +
        name +
        "_lg.jpg"
      );
    }
  },
  methods: {
    show_modal(item) {
      this.modal.ItemID = item.ItemID;
      this.modal.PhotoName = item.PhotoName;
      this.modal.ItemName = item.ItemName;
      this.modal.BasePrice = item.BasePrice;
      this.modal.Dimensions = item.Dimensions;
      this.modal.Description = item.Description;
      this.count = 1;
      this.clicked_product = true;
      console.log(this.photoName);
    },
    hide_modal() {
      this.clicked_product = false;
    },
    show_contact_modal() {
      this.clicked_contact = true;
    },
    hide_contact_modal() {
      this.clicked_contact = false;
    },
    exit_contact_modal(e) {
      var modal = document.getElementById("contact");
      if (e.target == modal) {
        this.clicked_contact = false;
      }
    },
    exit_product_modal(e) {
      var modal = document.getElementById("myModal");
      if (e.target == modal) {
        this.clicked_product = false;
      }
    },
    increment() {
      if (this.count >= 0) {
        this.count++;
      }
    },
    decrement() {
      if (this.count > 0) {
        this.count--;
      }
    }
  }
};
</script>

<style scoped>
.card_container {
  display: inline-block;
  vertical-align: top;
  /* height: 380px; */
  width: 210px;
  background: #ffffff;
  border: 0 solid #c0c0c0;
  box-shadow: 0 3px 4px 0 rgba(0, 0, 0, 0.12);
  border-radius: 2.7px;
  margin: 0px 10px;
  margin-top: 60px;
  margin-bottom: 20px;
}

.photo_name {
  width: 170.1px;
  height: 180px;
  margin: 9px;
}
.item_name {
  padding: 0px 10px;
  height: 30px;
  font-family: Poppins;
  font-weight: 500;
  font-size: 17.1px;
  color: #5e5e5e;
  text-align: center;
  margin-bottom: 20px;
}
.base_price {
  font-weight: bolder;
  font-family: Poppins;
  font-weight: 500;
  font-size: 24.3px;
  color: #9100a3;
  text-align: center;
}
.shop_button {
  width: 90%;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 20px;
  background-image: linear-gradient(
    180deg,
    #fdb413 0%,
    #9100a3 1%,
    #3e0045 100%
  );
  border-radius: 10px;
  padding: 20px;
  font-family: Poppins;
  font-weight: 200;
  font-size: 18.63px;
  color: #ffffff;
  text-align: center;
}
.center {
  text-align: center;
}
/* Modal css */

.modal {
  display: block; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 20px;
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
  position: relative;
  background-color: #fefefe;
  margin: auto;
  padding: 0;
  border: 1px solid #888;
  width: 500px;
  height: auto;
  overflow: hidden;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  -webkit-animation-name: animatetop;
  -webkit-animation-duration: 0.4s;
  animation-name: animatetop;
  animation-duration: 0.4s;
}

/* Add Animation */
@-webkit-keyframes animatetop {
  from {
    top: -300px;
    opacity: 0;
  }
  to {
    top: 0;
    opacity: 1;
  }
}

@keyframes animatetop {
  from {
    top: -300px;
    opacity: 0;
  }
  to {
    top: 0;
    opacity: 1;
  }
}

/* The Close Button */
.close {
  position: absolute;
  width: 40px;
  height: 35px;
  color: #fff;
  background: #9100a3;
  z-index: 1;
  font-size: 35px;
  font-weight: bold;
  border-radius: 5px;
  margin-top: -10px;
  margin-left: 220px;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}
/* product */
.product_container {
  width: 500px;
  background: #ffffff;
  border: 0 solid #c0c0c0;
  box-shadow: 0 3px 4px 0 rgba(0, 0, 0, 0.12);
  border-radius: 2.7px;
  padding-bottom: 50px;
  margin-right: 20px;
}

.product_photo_name {
  width: 462px;
  height: 450px;
  /* margin: 23.3px; */
}
.base_price_modal {
  border-radius: 5%;
  display: inline-block;
  width: 124px;
  height: 60px;
  background: #9100a3;
  font-family: Poppins;
  font-weight: 500;
  font-size: 24.3px;
  color: #ffffff;
  text-align: center;
  margin-left: 150px;
}
.count {
  width: 70px;
  height: 50px;
  border-radius: 5%;
  background: #9100a3;
  font-family: Poppins;
  font-weight: 500;
  font-size: 24.3px;
  color: #ffffff;
  text-align: center;
  margin-left: -2px;
  margin-right: -2px;
}
.count_btn {
  width: 54px;
  height: 40px;
  border-radius: 5%;
  background: #ffffff;
  font-family: Poppins;
  font-weight: 500;
  font-size: 24.3px;
  color: black;
  border-color: #9100a3;
  text-align: center;
}
.count_btn:hover {
  cursor: pointer;
}
.item_name_modal {
  text-align: left !important;
  margin-left: 50px;
  margin-top: 20px;
  font-family: Poppins;
  font-weight: 500;
  font-size: 21.1px;
  color: #5e5e5e;
  margin-bottom: 15px;
}

.inline {
  display: inline-block;
}

.label {
  font-family: Poppins;
  font-weight: 500;
  font-size: 17.94px;
  color: #373737;
  margin-bottom: 0px;
}
.block {
  margin-bottom: -20px;
  margin-left: -5px;
  margin-right: 30px;
}
.description {
  font-family: Poppins;
  font-weight: 500;
  margin-left: 5px;
  font-size: 17.94px;
  color: #5e5e5e;
  margin-bottom: 5px;
}

.contact_button {
  display: inline-block;
  height: 60px;
  color: #9100a3;
  width: 200px;
  background: none;
  border: 1px solid #9100a3;
  border-radius: 6px;
}
.contact_button:hover {
  cursor: pointer;
}
.cart_button {
  background: yellow;
  margin-left: 20px;
  display: inline-block;
  height: 60px;
  width: 200px;
  color: #5e5e5e;
  background-image: lineargradient(0deg, #ff9b00 0%, #fcd304 100%);
  border: 0 solid #979797;
  border-radius: 6px;
}

.cart_button:hover {
  cursor: pointer;
}

.footer {
  margin-top: 100px;
}

/* Contact Modal css */
.contact_container {
  width: 480px;
  background: #ffffff;
  border: 0 solid #c0c0c0;
  box-shadow: 0 3px 4px 0 rgba(0, 0, 0, 0.12);
  border-radius: 2.7px;
  padding-bottom: 50px;
  padding-top: 30px;
  padding-left: 10px;
  padding-right: 10px;
}

.header {
  display: flex;
  justify-content: space-between;
  height: 50px;
}
.contact_logo {
  margin-left: 20px;
}
.sales_rep {
  font-family: Poppins;
  font-weight: 800;
  color: #9100a3;
  font-size: 25px;
  margin-top: 15px;
  margin-right: 10px;
}
.sales_div {
  text-align: center;
}
.sales_rep_image {
  width: 462px;
  height: 300px;
}
.info {
  margin-top: -80px;
  margin-bottom: 70px;
}
.text {
  font-size: 25px;
  font-family: Poppins;
  font-weight: 500;
}
.contact_info {
  text-align: center;
}
.contact_me {
  font-size: 22px;
  font-family: Poppins;
  font-weight: 800;
}
.inline {
  display: inline-block;
}
.email {
  font-size: 20px;
  margin-left: 4px;
  font-family: Poppins;
  font-weight: 500;
}
.phone {
  font-size: 20px;
  margin-left: 4px;
  font-family: Poppins;
  font-weight: 500;
}
.font {
  font-family: Poppins;
  font-weight: 800;
}
.footer_btn {
  font-family: Poppins;
  font-size: 26px;
  font-weight: 800;
}
</style>
