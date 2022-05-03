<template>
  <div class="wrapper">
    <div id="book">
      <div class="title">
        <h3>Each book is limited.</h3>
      </div>
      <div class="md-layout">
        <div
          v-for="n in bookList"
          :key="n.img"
          class="md-layout-item md-size-66 mx-auto"
        >
          <img :src="require(`@/assets/img/${n.img}`)" class="img-harry" />
          <div class="md-checkbox-list">
            <md-checkbox v-model="n.check" @change="onCheckedChange($event, n)"
              >Checked</md-checkbox
            >
            <div class="md-input-list">
              <div
                style="margin-top: -1.5rem; margin-left: 2rem"
                class="
                  md-layout-item
                  md-size-80
                  md-xsmall-size-100
                  md-small-size-50
                  md-medium-size-80
                "
              >
                <md-field>
                  <md-input
                    v-model="n.amount"
                    placeholder="Amount"
                    :disabled="!n.check"
                    type="number"
                    @keypress="isNumber($event, n)"
                    min="0"
                    oninput="javascript: if (this.value.length > 9) this.value = this.value.slice(0, 9);"
                  ></md-input>
                </md-field>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div id="calculate">
      <div class="title">
        <h3>Summary</h3>
      </div>
      <div class="md-layout">
        <div class="md-layout-item md-size-66 mx-auto">
          <md-button class="md-info md-sm" @click="onClickCalculate()"
            >Calculate</md-button
          >
        </div>
        <div
          v-for="n in calculateList"
          :key="n.header"
          class="md-layout-item md-size-66 mx-auto"
        >
          <span class="header-calculate">{{ n.header }}</span>
          <span class="header-calculate result-calculate">{{ n.result }}</span>
          <span class="baht">Baht.</span>
        </div>
      </div>
    </div>
    <!-- end buttons -->
    <div class="space-50"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      defaultPagination: 3,
      infoPagination: 3,
      initial: null,
      floatingLabel: null,
      success: null,
      error: null,
      withMIcon: null,
      withFaIcon: null,
      checkbox1: true,
      checkbox2: null,
      checkbox3: true,
      checkbox4: null,
      radio1: true,
      radio2: false,
      switch1: true,
      switch2: null,
      amount: 30,
      amount2: 60,
      buffer: 40,
      sliders: {
        simple: 40,
        rangeSlider: [20, 60],
      },
      bookList: [
        {
          img: "harry-1.jpg",
          check: false,
          amount: null,
        },
        {
          img: "harry-2.jpg",
          check: false,
          amount: null,
        },
        {
          img: "harry-3.jpg",
          check: false,
          amount: null,
        },
        {
          img: "harry-4.jpg",
          check: false,
          amount: null,
        },
        {
          img: "harry-5.jpg",
          check: false,
          amount: null,
        },
        {
          img: "harry-6.jpg",
          check: false,
          amount: null,
        },
        {
          img: "harry-7.jpg",
          check: false,
          amount: null,
        },
      ],
      calculateList: [
        {
          header: "Total :",
          result: this.toFixed(0),
        },
        {
          header: "Discount :",
          result: this.toFixed(0),
        },
        {
          header: "Net Price :",
          result: this.toFixed(0),
        },
      ],
    };
  },
  methods: {
    onCheckedChange(e, n) {
      if (!e) n.amount = null;
    },
    isNumber(evt) {
      const keysAllowed = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"];
      const keyPressed =
        !evt.target.value && evt.key === "0"
          ? evt.key.replace(/^0+/, "")
          : evt.key;

      if (!keysAllowed.includes(keyPressed)) {
        evt.preventDefault();
      }
    },
    clearRef(e) {
      return JSON.parse(JSON.stringify(e || ""));
    },
    onClickCalculate() {
      if (this.bookList.every((x) => !x.amount)) {
        this.calculateList.forEach((x) => (x.result = this.toFixed(0)));
      } else {
        let book = this.clearRef(this.bookList);
        book = book.filter((x) => !!x.amount);

        if (book.length > 0) {
          let discount = 0;
          let amountList = book.map((x) => +x.amount);
          let total = amountList.reduce((a, b) => a + b, 0) * 100;
          while (amountList?.length > 0) {
            let min = Math.min.apply(Math, amountList);
            let lengthDistinctList = amountList.length;
            discount +=
              (lengthDistinctList - 1) * 0.1 * 100 * min * lengthDistinctList;
            amountList = amountList.filter((x) => +x !== min);
            amountList.forEach((x) => (x -= min));
          }

          discount = parseInt(discount);

          this.calculateList[0].result = this.toFixed(total);
          this.calculateList[1].result = this.toFixed(discount);
          this.calculateList[2].result = this.toFixed(total - discount);
        }
      }
    },
    toFixed(e, fraction) {
      return e?.toFixed(fraction || 2).replace(/\d(?=(\d{3})+\.)/g, "$&,");
    },
  },
};
</script>

<style lang="scss" scoped>
.vertical-center {
  display: flex;
  align-items: center;
}
.flex-column {
  display: flex;
  flex-direction: column;
}
.md-checkbox,
.md-radio {
  display: flex;
  margin: 0;
  margin-bottom: 0.5rem;
}
.header-calculate {
  width: 12rem;
  display: inline-block;
  margin-left: 30%;
}
.result-calculate {
  margin-left: -10rem;
  text-align: right;
}
.baht {
  margin-left: 4rem;
}
</style>
