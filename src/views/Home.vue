<template>
  <div class="container">
    <div class="nav_container">
      <div class="box">
        <div>
          <img v-if="loaded" :src="url" alt="logo" />
        </div>
        <div>
          <button @click="show_modal" class="contact_button">
            <i class="fad fa-user-headset fa-2x"></i>
            <span class="contact_me_btn"
              >Contact&nbsp;{{ info.firstname }}</span
            >
          </button>
        </div>
      </div>
    </div>
    <div class="main_image">
      <div class="cover"></div>
      <div class="info_block_new">
        <p class="title">{{ info.companyname }}</p>
        <p class="msg" v-html="info.message"></p>
      </div>
    </div>
    <div v-if="loaded" class="products_container">
      <div class="content">
        <ProductCard @show_modal="show_modal" />
      </div>
    </div>
    <div v-if="clicked" id="contact" class="modal center" @click="exit_modal">
      <div class="center">
        <i @click="hide_modal" class="fal fa-times close"></i>
      </div>
      <div class="modal-content">
        <div class="contact_container">
          <div class="header">
            <div class="contact_logo">
              <img :src="url" alt="logo" height="50" width="70" />
            </div>
            <div>
              <p class="sales_rep">
                Sales Rep
              </p>
            </div>
          </div>
          <div class="sales_div">
            <img
              src="../assets/images/salesrep.jpeg"
              alt=""
              width="500"
              height="310"
            />
          </div>
          <div class="info">
            <div class="text_name">
              {{ info.firstname }}&nbsp;{{ info.lastname }}
            </div>
            <div class="text_address">
              {{ info.city }},&nbsp;{{ info.state }}
            </div>
          </div>
          <div class="contact_info">
            <div class="contact_me">Email or Call Me Direct</div>
            <div class="email_block">
              <i class="fa fa-envelope fa-lg inline" style="color:#9100a3;"></i>
              <span class="font">Email</span>:
              <p class="inline email">{{ info.email }}</p>
            </div>
            <div class="phone_block">
              <i class="fa fa-phone fa-lg inline" style="color:#9100a3;"></i>
              <span class="font">Phone</span>:
              <p class="inline phone">{{ info.phone }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProductCard from "../components/ProductCard";
import JsonData from "../assets/db/db.json";
export default {
  name: "Home",
  components: {
    ProductCard
  },
  data() {
    return {
      json: JsonData,
      info: {
        companyname: "",
        message: "",
        firstname: "",
        lastname: "",
        city: "",
        state: "",
        email: "",
        phone: ""
      },
      items: [],
      loaded: false,
      logo: null,
      clicked: false
    };
  },
  computed: {
    url() {
      return (
        "http://images.repzio.com/productimages/" +
        this.logo +
        "/logo" +
        this.logo +
        "_lg.jpg?width=90&height=60"
      );
    }
  },
  mounted() {
    this.info.companyname = this.json.CompanyName;
    this.info.message = this.json.Message;
    this.info.firstname = this.json.SalesRep.FirstName;
    this.info.lastname = this.json.SalesRep.LastName;
    this.info.city = this.json.SalesRep.City;
    this.info.state = this.json.SalesRep.State;
    this.info.email = this.json.SalesRep.EmailAddress;
    this.info.phone = this.json.SalesRep.Phone;
    this.items = this.json.items;
    this.logo = this.items[0].ManufacturerID;
    this.loaded = true;
  },
  methods: {
    show_modal() {
      this.clicked = true;
    },
    hide_modal() {
      this.clicked = false;
    },
    exit_modal(e) {
      var modal = document.getElementById("contact");
      if (e.target == modal) {
        this.clicked = false;
      }
    }
  }
};
</script>

<style scoped>
.container {
  overflow: hidden;
}
.nav_container {
  margin-right: auto;
  margin-left: auto;
  width: 956px;
  margin-bottom: 10px;
}
.box {
  width: 956px;
  display: flex;
  justify-content: space-between;
}
.cover {
  background-image: url("https://ugcmedia.com/images/hero_1440_narrow4.png");
  background-color: #898a8b;
  height: 315px;
  background-position: right bottom;
  background-repeat: no-repeat;
  background-size: cover;
}
.main_image {
  position: relative;
  text-align: center;
}
.tile_logo {
  position: absolute;
  margin-left: -450px;
  margin-top: 130px;
  height: 150px;
  width: 150px;
}
.products_container {
  position: relative;
  min-height: auto;
  margin-left: auto;
  margin-right: auto;
  width: 956px;
  background: #f3f3f3;
  box-shadow: 0 10px 14px 0 rgba(0, 0, 0, 0.27);
  border-radius: 9px;
}
.content {
  text-align: center;
  margin-top: -20px;
  margin-bottom: 5px;
}
.inline {
  display: inline-block;
}
.info_block {
  position: absolute;
  margin-top: -250px;
  margin-left: 200px;
}
.title {
  font-size: 28px;
  color: #9100a3;
  font-family: Poppins;
  font-weight: 800;
}
.msg {
  font-size: 17px;
  font-family: Poppins;
  color: #fff;
  font-weight: 400;
}

@media (max-width: 600px) {
  .bg {
    height: 200px;
    width: auto;
  }
  .products_container {
    width: 250px;
    margin-left: auto;
    margin-right: auto;
  }
  .box {
    width: calc(100vw - 80px);
  }
}
@media (min-width: 600px) and (max-width: 800px) {
  .bg {
    height: 300px;
    width: auto;
  }
  .products_container {
    width: 550px;
    margin-left: auto;
    margin-right: auto;
  }
  .box {
    width: calc(100vw - 80px);
  }
}
@media (min-width: 800px) and (max-width: 1200px) {
  .bg {
    height: 400px;
    width: auto;
  }
  .products_container {
    width: 750px;
    margin-left: auto;
    margin-right: auto;
  }
  .box {
    width: calc(100vw - 80px);
  }
}
@media (min-width: 1200px) {
  .info_block {
    left: 30%;
    margin-top: -400px;
  }
}

/* Moda css */
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
  width: 34px;
  height: 32px;
  color: #fff;
  background: #9100a3;
  z-index: 1;
  font-size: 35px;
  font-weight: lighter;
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
.contact_button {
  margin-right: -50px;
  color: white;
  width: 250px;
  height: 60px;
  background: #9100a3;
  border: 1px solid #9100a3;
  border-radius: 6px;
}
.contact_container {
  position: relative;
  width: 480px;
  background: #ffffff;
  border: 0 solid #c0c0c0;
  box-shadow: 0 3px 4px 0 rgba(0, 0, 0, 0.12);
  border-radius: 2.7px;
  padding-bottom: 50px;
  padding-top: 20px;
}

.header {
  display: flex;
  justify-content: space-between;
  height: 50px;
  padding-bottom: 10px;
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
  margin-right: 90px;
}
.sales_div {
  text-align: center;
}
.info {
  margin-top: -80px;
  margin-bottom: 40px;
  text-align: center;
}
.text_name {
  font-size: 30px;
  font-family: Poppins;
  font-weight: 400;
  color: #fff;
}
.text_address {
  font-size: 18px;
  font-family: Poppins;
  font-weight: lighter;
  color: #fff;
}
.font {
  margin-left: 10px;
  font-family: Poppins;
  font-weight: 500;
  font-size: 20px;
}
.contact_info {
  text-align: center;
  margin: 0px;
}
.contact_me {
  /* margin: 0; */
  font-size: 22px;
  font-family: Poppins;
  font-weight: 500;
}
.inline {
  display: inline-block;
}
.email {
  font-size: 20px;
  margin-left: 4px;
  margin-top: 10px;
  margin-bottom: 0px;
  font-family: Poppins;
  font-weight: 300;
  /* font-weight: lighter; */
}
.phone {
  font-size: 20px;
  margin-left: 4px;
  margin-top: 10px;
  margin-bottom: 0px;
  font-family: Poppins;
  font-weight: 300;
}
.contact_me_btn {
  margin-left: 15px;
  font-family: Poppins;
  font-size: 25px;
  font-weight: 500;
}
.center {
  text-align: center;
}
.email_block {
  margin-top: 0px;
}
.info_block_new {
  width: 100%;
  position: absolute;
  margin-top: -280px;
}
</style>
