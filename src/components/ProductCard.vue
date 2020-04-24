<template>
  <div v-if="loaded" class="center">
    <div class="inline" v-for="(item, index) in items" :key="index">
      <div @click="show_modal(item)" class="card_container">
        <img
          :src="item.PhotoName + '?width=170&height=180'"
          alt="Product_Photo"
          class="photo_name"
        />
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
        <i @click="hide_modal" class="fal fa-times fa-lg close"></i>
      </div>
      <div class="modal-content">
        <div class="product_container">
          <img
            :src="modal.PhotoName"
            class="product_image"
            alt="Product"
            width="462"
            height="450"
          />
          <div class="buttons">
            <div class="inline first_group">
              <button class="count_btn plus" @click="increment">+</button>
              <button class="count">{{ count }}</button>
              <button class="count_btn minus" @click="decrement">-</button>
            </div>
            <div class="inline">
              <button class="base_price_modal">${{ modal.BasePrice }}</button>
            </div>
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
              <p class="description_text">
                {{ modal.Description }}
              </p>
            </div>
          </div>
          <div class="center footer">
            <button @click="show_contact_modal" class="contact_button inline">
              <i class="fad fa-user-headset fa-2x"></i>
              <span class="footer_btn">Contact me</span>
            </button>
            <button class="cart_button inline">
              <i class="fad fa-cart-arrow-down fa-2x"></i>
              <span class="footer_btn">Add to Cart</span>
            </button>
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
  mounted() {
    this.items = this.json.items;
    setTimeout(() => {
      this.loaded = true;
    }, 300);
    this.logo = this.items[0].ManufacturerID;
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
      this.$emit("show_modal");
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
  outline: none;
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
  border: none;
  padding: 20px;
  font-family: Poppins;
  font-weight: 200;
  font-size: 18.63px;
  color: #ffffff;
  text-align: center;
  outline: none;
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
  margin-top: 120px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 100px;
  position: relative;
  background-color: #fefefe;
  padding: 0;
  border: 1px solid #888;
  border-radius: 10px;
  width: 480px;
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
  width: 44px;
  height: 32px;
  color: #fff;
  background: #9100a3;
  z-index: 1;
  font-weight: lighter;
  border-radius: 5px;
  margin-top: 100px;
  margin-left: 170px;
  line-height: 45px !important;
  padding: 0px 5px 8px 5px;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}
/* product */
.product_container {
  width: 480px;
  background: #ffffff;
  border: 0 solid #c0c0c0;
  box-shadow: 0 3px 4px 0 rgba(0, 0, 0, 0.12);
  border-radius: 2.7px;
  padding-bottom: 50px;
  margin-right: 20px;
}

.base_price_modal {
  border-radius: 10px;
  display: inline-block;
  width: 124px;
  height: 60px;
  background: #9100a3;
  font-family: Poppins;
  font-weight: 500;
  font-size: 24.3px;
  color: #ffffff;
  text-align: center;
  outline: none;
  border: none;
}
.count {
  width: 70px;
  height: 52px;
  border-radius: 7px;
  background: #9100a3;
  font-family: Poppins;
  font-weight: 500;
  font-size: 24.3px;
  color: #ffffff;
  text-align: center;
  margin-left: -5px;
  margin-right: -5px;
  position: relative;
  outline: none;
  border: none;
  box-shadow: none !important;
}
.count_btn {
  width: 55px;
  height: 40px;
  border-radius: 5px;
  background: #ffffff;
  font-family: Poppins;
  font-weight: 500;
  font-size: 24.3px;
  color: black;
  border-color: #9100a3;
  text-align: center;
  outline: none;
}
.plus {
  border-right: none;
}
.minus {
  border-left: none;
}
.count_btn:hover {
  cursor: pointer;
}
.item_name_modal {
  text-align: left !important;
  padding-left: 40px;
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
.buttons {
  margin-top: 20px;
}
.first_group {
  margin-right: 100px;
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
  position: absolute;
  max-width: 250px;
  font-family: Poppins;
  font-weight: 500;
  margin-left: 5px;
  font-size: 17.94px;
  color: #5e5e5e;
  margin-bottom: 5px;
  overflow: wrap;
}
.description_text {
  max-width: 420px;
  font-family: Poppins;
  font-weight: 500;
  margin-left: 5px;
  font-size: 17.94px;
  color: #5e5e5e;
  margin-bottom: 5px;
  overflow: wrap;
}

.contact_button {
  display: inline-block;
  height: 60px;
  color: #9100a3;
  width: 200px;
  background: none;
  border: 1px solid #9100a3;
  border-radius: 6px;
  outline: none;
}
.contact_button:hover {
  cursor: pointer;
}
.cart_button {
  margin-left: 20px;
  display: inline-block;
  height: 60px;
  width: 200px;
  color: #5e5e5e;
  background-image: linear-gradient(0deg, #ff9b00 0%, #fcd304 100%);
  border: 0 solid #979797;
  border-radius: 6px;
}

.cart_button:hover {
  cursor: pointer;
}

.footer {
  margin-top: 100px;
}
.footer_btn {
  margin-left: 15px;
  font-family: Poppins;
  font-size: 20px;
  font-weight: 100;
}
.product_image {
  padding: 10px;
}
@media (max-width: 450px) {
  .modal-content {
    width: 320px;
  }
  .product_container {
    width: 320px;
  }
  .product_image {
    width: 310px;
    height: 310px;
  }
  .first_group {
    margin-right: 10px;
  }
  .contact_button {
    margin-left: 10px;
    margin-bottom: 10px;
  }
  .cart_button {
    margin-left: 10px;
  }
  .close {
    margin-left: 90px;
  }
}
@media (max-width: 600px) {
  .card_container {
    width: 300px;
  }
  .photo_name {
    height: 250px;
  }
}
</style>
