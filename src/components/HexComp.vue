<template>
  <div class="container">
    <p>Input some hex code to invert</p>

    <textarea
      class="form-control"
      :class="{ 'is-invalid': $v.hexNumber.$error }"
      v-model="hexNumber"
      placeholder="Example: #fafafa"
      cols="30"
      rows="5"
      @input="$v.hexNumber.$touch()"
    ></textarea>

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
      if (hexnum[0] == "#") hexnum = hexnum.substring(1);
      console.log(hexnum);
      if (hexnum.length === 3) {
        hexnum = hexnum
          .split("")
          .map(function(hex) {
            return hex + hex;
          })
          .join("");
      } else if (hexnum.length === 4) {
        hexnum = hexnum
          .split("")
          .map(function(hex) {
            return hex + hex;
          })
          .join("");
      }
      console.log(hexnum);
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
      if (hexnum[0] == "#") {
        index = 1;
      } else {
        resultnum += "#";
      }

      for (let i = index; i < hexnum.length; i++) {
        if (i > 5) {
          resultnum += splitnum[i];
        } else {
          if (!isNaN(splitnum[i])) {
            resultnum += simplenum[splitnum[i]];
          } else if (complexnum[splitnum[i]]) {
            resultnum += complexnum[splitnum[i]];
          } else {
            return false;
          }
        }
      }
      return resultnum;
    },
    isValidHex(code) {
      if (!code || typeof code !== "string") return false;
      // Validate hex values
      if (code.substring(0, 1) === "#") code = code.substring(1);

      switch (code.length) {
        case 3:
          return /^[0-9A-F]{3}$/i.test(code);
        case 4:
          return /^[0-9A-F]{4}$/i.test(code);
        case 6:
          return /^[0-9A-F]{6}$/i.test(code);
        case 8:
          return /^[0-9A-F]{8}$/i.test(code);
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
  text-align: center;
  align-self: center;
  font-size: 14px;
}
li {
  list-style-type: none;
  margin: 10px;
}
.btn {
  margin: 5px;
}
</style>
