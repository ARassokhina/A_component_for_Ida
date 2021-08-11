<template>
  <div class="hello">
    <h1>Добавление товара</h1>
    <div class="wrapper">
      <div class="left">
        <p>Наименование товара <span class="star">*</span></p>
        <input type="text" v-model="productName" required />
        <div class="error" v-if="visibility1">Поле является обязательным</div>

        <p>Описание товара</p>
        <textarea name="text" v-model="productDescription"></textarea>
        <p>Ссылка на изображение товара <span class="star">*</span></p>
        <label for="file-upload" class="custom-file-upload">
          <input
            type="file"
            id="file"
            aria-label="File browser example"
            @change="onFileSelected"
            required
        /></label>
        <div class="error" v-show="visibility2">Поле является обязательным</div>
        <p>Цена товара <span class="star">*</span></p>

        <input type="text" v-model="productPrice" required />
        <div class="error" v-show="visibility3">Поле является обязательным</div>

        <button
          v-on:click="examine"
          @click="createElement"
          v-if="!visibility1 && !visibility2 && !visibility3"
        >
          Добавить товар
        </button>

        <button disabled="true" v-else>Добавить товар</button>
      </div>
      <div class="right">
        <div id="grid-container"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {},
  data() {
    return {
      vis1: false,
      vis2: false,
      vis3: false,
      url: null,
      goodsArray: [],
      productName: "",
      selection: "",
      productDescription: "",
      productPrice: "",
      productUrl: "",
    };
  },
  methods: {
    onFileSelected(event) {
      this.selection = event.target.files[0];
      this.url = URL.createObjectURL(this.selection);
    },
    examine() {
      if (this.productName.length == 0) this.vis1 = true;
      else this.vis1 = false;
      if (this.url == null) this.vis2 = true;
      else this.vis2 = false;
      if (this.productPrice.length == 0) this.vis3 = true;
      else this.vis3 = false;
    },
    showBtn() {
      console.log("Ya showed");
    },
    hideBtn() {
      console.log("Ya hided");
    },
    deleteBtn(e) {
      const ichild = e.target.closest(".buttonChild");
      ichild.remove();
    },
    createElement() {
      var grid = document.getElementById("grid-container");
      var copy = document.createElement("div");
      var btn = document.createElement("div");
      var parag1 = document.createElement("p");
      parag1.classList.add("prodName");
      var parag2 = document.createElement("p");
      var parag3 = document.createElement("p");
      parag2.classList.add("prodDescr");
      parag1.innerText = this.productName;
      parag2.innerText = this.productDescription;

      var n = this.productPrice.toString();
      this.productPrice =
        n.replace(/(\d{1,3}(?=(?:\d\d\d)+(?!\d)))/g, "$1" + " ") + " " + "руб.";

      parag3.innerText = this.productPrice;
      parag3.classList.add("prodPr");
      var img1 = document.createElement("img");
      img1.src = this.url;
      img1.width = 332;
      img1.height = 200;
      copy.appendChild(img1);
      copy.appendChild(parag1);
      copy.appendChild(parag2);
      copy.appendChild(parag3);
      btn.classList.add("buttonChild");
      var button = document.createElement("div");
   
      button.classList.add("button");

      btn.appendChild(button);
      btn.appendChild(copy);
      if (this.vis1 == false && this.vis3 == false) {
        grid.appendChild(btn);
        this.goodsArray.push("0");
      }
      this.vis1 = false;
      this.vis2 = false;
      this.vis3 = false;
      btn.addEventListener(onmouseover, this.showBtn);
      btn.addEventListener(onmouseleave, this.hideBtn);
      btn.addEventListener("click", this.deleteBtn);
      this.productName = "";

      this.productDescription = "";
      this.productPrice = "";
    },
  },
  computed: {
    visibility1: function () {
      if (this.productName.length !== 0) return false;
      else return true;
    },
    visibility2: function () {
      if (this.url !== null) return false;
      else return true;
    },
    visibility3: function () {
      if (this.productPrice.length !== 0) return false;
      else return true;
    },
  },
  filters: {
    formatPrice(price) {
      if (!parseInt(price)) {
        return "";
      }
      if (price > 999) {
        var priceArray = price.split("").reverse();
        var index = 0;
        while (priceArray.length > index + 3) {
          priceArray.splice(index + 3, 0, ",");
          index += 4;
        }
        return priceArray.reverse().join("") + "руб.";
      } else {
        return price + "руб.";
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
