<template>
  <q-page padding>
    <div class="row justify-center">
      <div class="col-12 col-md-8">
        <q-card>
          <q-card-section class="bg-primary text-white">
            <div class="text h6">CALCULADORA</div>
          </q-card-section>

          <q-card-section>
            <div class="text-h5 text-grey-5 text-right">
              {{ acumulator + actual }}
            </div>

            <div class="text-h3 text-right">
              {{ result }}
            </div>
          </q-card-section>

          <q-card-section class="bg-grey-4">
            <div class="row q-col-gutter-sm">
              <div
                class="col-3"
                v-for="(button, index) in buttons"
                :key="index"
              >
                <q-btn
                  class="full-width text-h6"
                  :color="noEsNum(button) ? 'indigo' : 'grey-2'"
                  :text-color="noEsNum(button) ? 'white' : 'grey-8'"
                  @click="btnAction(button)"
                >
                  {{ button }}
                </q-btn>
              </div>

              <div class="col-6">
                <q-btn
                  class="full-width text-h6"
                  color="orange"
                  @click="resetBtn"
                >
                  RESET
                </q-btn>
              </div>

              <div class="col-6">
                <q-btn
                  class="full-width text-h6"
                  color="green"
                  @click="resultBtn"
                >
                  =
                </q-btn>
              </div>
            </div>
          </q-card-section>
        </q-card>
        <pre>{{ resultsArray }}</pre>
      </div>
    </div>
  </q-page>
</template>

<script>
import { evaluate, round } from "mathjs";

export default {
  name: "Calculator",

  data() {
    return {
      buttons: [7, 8, 9, "%", 4, 5, 6, "+", 1, 2, 3, "-", ".", 0, "/", "*"],
      actual: "",
      acumulator: "",
      result: "",
      operatorClick: true,
      resultsArray: [],
    };
  },
  methods: {


    noEsNum(valor) {
      return isNaN(valor);
    },

    btnAction(valor) {
      if (!this.noEsNum(valor)) {
        if (this.operatorClick) {
          this.actual = "";
          this.operatorClick = false;
        }
        this.actual = `${this.actual}${valor}`;
      } else {
        this.ejectOp(valor);
      }
    },

    ejectOp(valor) {
      if (valor === ".") {
        if (this.actual.indexOf(".") === -1) {
          this.actual = `${this.actual}${valor}`;
        }
        return;
      }

      if (valor === "%") {
        if (this.actual !== "") {
          this.actual = `${parseFloat(this.actual) / 100}`;
        }
        return;
      }

      this.addOperator(valor);
    },

    addOperator(valor) {
      if (!this.operatorClick) {
        this.acumulator += `${this.actual} ${valor}`;
        this.actual = "";
        this.operatorClick = true;
      }
    },

    resetBtn() {
      this.actual = "";
      this.acumulator = "";
      this.result = "";
      this.operatorClick = true;
    },

    resultBtn() {
      if (!this.operatorClick) {
        this.result = evaluate(this.acumulator + this.actual);
        this.resultsArray.push(
          `${this.acumulator} + ${this.actual} = ${this.result}`
        );
      } else {
        this.result = "Error!";
      }
    },
  },
};
</script>

<style>
.text-h5 {
  height: 25px;
}

.text-h3 {
  height: 50px;
}
</style>
