<template>
  <div class="container">
    <label for="hexNum">Input some color using hex code</label>

    <textarea
      class="form-control"
      :class="{ 'is-invalid': $v.hexNumber.$error }"
      v-model="hexNumber"
      placeholder="Example: #fafafa"
      cols="30"
      rows="5"
      @blur="$v.hexNumber.$touch()"
    ></textarea>

    <div class="invalid-feedback" v-if="!$v.hexNumber.$required">
      Hex is required
    </div>

    <div class="invalid-feedback" v-if="!$v.hexNumber.$minLength">
      Hex code must be at least 3 letters
    </div>

    <button class="btn btn-primary" :disabled="$v.$invalid" @click="invertHex">
      Invert
    </button>

    <div class="row">
      <div class="col">
        <ul>
          <li v-for="(color, index) in resultArray" :key="index">
            <div :style="{ 'background-color': color }" class="colored"></div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import { required, minLength } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      hexNumber: [], //v-model to textarea
      hexArray: [], //pushed items from hexNumbers
      resultArray: []
    };
  },
  methods: {
    invertHex() {
      this.resultArray = [];
      this.hexArray = this.hexNumber.split("\n");
      this.hexArray.forEach(element => {
        if (this.isValidHex(element)) {
          this.resultArray.push(this.invertHexCode(element));
        }
      });
      console.log(this.resultArray);
    },
    invertHexCode(hexnum) {
      if (hexnum.length === 3) {
        hexnum = hexnum
          .split("")
          .map(function(hex) {
            return hex + hex;
          })
          .join("");
      }
      hexnum = hexnum.toUpperCase();
      let splitnum = hexnum.split("");
      let resultnum = "";
      let simplenum = "FEDCBA9876".split("");
      let complexnum = new Array();
      complexnum.A = "5";
      complexnum.B = "4";
      complexnum.C = "3";
      complexnum.D = "2";
      complexnum.E = "1";
      complexnum.F = "0";

      let index = 0;
      if (hexnum[0] == "#") index = 1;
      else index = 0;

      resultnum += "#";
      for (let i = index; i < hexnum.length; i++) {
        if (!isNaN(splitnum[i])) {
          resultnum += simplenum[splitnum[i]];
        } else if (complexnum[splitnum[i]]) {
          resultnum += complexnum[splitnum[i]];
        } else {
          return false;
        }
      }
      return resultnum;
    },
    isValidHex(color) {
      if (!color || typeof color !== "string") return false;
      // Validate hex values
      if (color.substring(0, 1) === "#") color = color.substring(1);

      switch (color.length) {
        case 3:
          return /^[0-9A-F]{3}$/i.test(color);
        case 6:
          return /^[0-9A-F]{6}$/i.test(color);
        case 8:
          return /^[0-9A-F]{8}$/i.test(color);
        default:
          return false;
      }
    }
  },
  validations: {
    hexNumber: {
      required,
      minLength: minLength(3)
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}
.row {
  align-items: center;
}
.colored {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  border: 1px solid black;
}
li {
  list-style-type: none;
  margin: 10px;
}
.btn {
  margin: 5px;
}
</style>
