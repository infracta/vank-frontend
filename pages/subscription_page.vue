<template>
  <div>
    <header-nav />
    <div class="subscription_wrap">
      <div class="withdraw_wrap">
        <h5>
          <nuxt-link to="/user_dashboard/dashboard"
            ><font-awesome-icon
              class="fa-1x text-white bg-primary"
              :icon="['fas', 'arrow-left']"
            />
          </nuxt-link>

          <span>Buy Now</span>
        </h5>
      </div>
      <div class="subscription_content">
        <div class="d-flex justify-content-center">
          <div>
            <div class="question_wrap">
              <h6>What do you want to buy?</h6>
            </div>
            <p>
              We buy into a monthly index of Crypto Assets for you. This index
              changes from month <br />
              to month. You must set a savings target of at least N5,000 to
              join.
            </p>
            <h5 class="text-center">Choose your Plan</h5>
            <div class="plan_wrap">
              <div class="mt-2">
                <form action="" @submit.prevent="request_asset()">
                  <b-form-group v-slot="{ ariaDescribedby }">
                    <b-form-radio
                      v-model="subscription_plan.btc"
                      :aria-describedby="ariaDescribedby"
                      name="some-radios"
                      value="Usd"
                      class="label"
                      >Basic Plan (USD)</b-form-radio
                    >
                    <span>US Dollar Equivalent</span>
                    <b-form-radio
                      v-model="subscription_plan.btc"
                      :aria-describedby="ariaDescribedby"
                      name="some-radios"
                      value="Bitcoin"
                      class="label"
                      >BTC Plan</b-form-radio
                    >
                    <span>Bitcoin to your Wallet</span>

                    <b-form-radio
                      v-model="subscription_plan.vank_basket"
                      :aria-describedby="ariaDescribedby"
                      name="some-radios"
                      value="Basket"
                      class="label"
                      >VANK Basket Plan (5,000 + 5%)</b-form-radio
                    >
                    <span>Curated Crypto</span>
                  </b-form-group>

                  <!-- <div class="form-check ml-5 py-2">
                    <input
                      class="form-check-input"
                      type="radio"
                      value=""
                      id="defaultCheck2"
                      v-model="subscription_plan.vank_basket"
                    />
                    <label class="form-check-label" for="defaultCheck2">
                      VANK Basket Plan (5,000 + 5%)
                    </label>
                    <br />
                    <span>Curated Crypto</span>
                  </div> -->
                  <div class="form-group mt-2">
                    <label class="" for="">Amount </label>
                    <input
                      type="number"
                      class="form-control px-2"
                      v-model="subscription_plan.amount"
                      placeholder="Enter Amount in Naira"
                    />
                  </div>
                  <div class="mt-3 text-center">
                    <v-btn
                      class=""
                      :loading="loading"
                      value="Withdraw"
                      type="submit"
                      >Buy Now</v-btn
                    >

                    <v-btn
                      class="buy_later_button"
                      value="Withdraw"
                      type="button"
                      >Buy Later</v-btn
                    >
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <footer-section />
  </div>
</template>

<script>
export default {
  // middleware: "auth",
  data() {
    return {
      loading: false,
      subscription_plan: {
        usdt: "",
        btc: "",
        vank_basket: "",
        amount: "",
      },
      mm: {},
    };
  },
  methods: {
    async request_asset() {
      try {
        this.loading = true;
        const response = await this.$axios.post(
          "/requestSubscription",
          this.subscription_plan
        );
        console.log(response);
        this.mm = response.data;
        localStorage.setItem("marketMakerKey", JSON.stringify(this.mm));
        console.log(this.mm);
        this.loading = false;
        this.$router.push("/list_market_makers");
        this.subscription_plan = {};
      } catch (error) {
        console.log(error.response);
        this.loading = false;
      }
    },
  },
};
</script>

<style >
.subscription_wrap {
  background-color: #fff;
  margin-top: 100px;
  padding: 80px 100px;
}
.subscription_wrap .subscription_content {
  background-color: #fff;
  /* width: 40%; */
  padding: 50px;
}
.subscription_wrap .subscription_content .question_wrap h6 {
  color: #162051;
  /* font-family: Titillium Web; */
  letter-spacing: 2px;
  font-weight: 600;
  text-align: center;
  padding: 10px 0;
  font-size: 20px;
}
.subscription_wrap .subscription_content p {
  font-size: 13px;
  text-align: center;
  margin-top: 15px;
}
.subscription_wrap .subscription_content h5 {
  margin-top: 20px;
}
.subscription_wrap .subscription_content .plan_wrap {
  width: 70%;
  margin: 0 auto;
}
.subscription_wrap .subscription_content .plan_wrap .form-check-input {
  position: unset;
}
.subscription_wrap .subscription_content .plan_wrap .form-check-label {
  font-size: 14px !important;
}
.subscription_wrap .subscription_content .plan_wrap .label {
  font-size: 15px;
  font-weight: 600;
}
.subscription_wrap .subscription_content .plan_wrap span {
  color: #1d83c5;
  font-size: 11px;
  margin-bottom: 0 !important;
}
.subscription_wrap .subscription_content .plan_wrap .v-btn {
  background-color: #00e8fe;
  font-size: 15px;
  padding: 5px;
  box-shadow: none !important;
  text-transform: none;
  /* margin-top: 1px; */
}
.subscription_wrap .subscription_content .plan_wrap .buy_later_button {
  border: 1px solid #00e8fe;
  color: #00e8fe;
  background-color: #fff;
}

@media (max-width: 768px) {
  .subscription_wrap {
    padding: 10px;
  }
  .subscription_wrap .subscription_content {
    width: 90%;
    padding: 10px;
  }
  .subscription_wrap .subscription_content h1 {
    font-size: 30px;
  }
  .subscription_wrap .subscription_content p {
    font-size: 10px;
  }
  .subscription_wrap .subscription_content .plan_wrap {
    width: 100%;
    margin: 0 auto;
  }
  .subscription_wrap .subscription_content .plan_wrap .v-btn {
    background-color: #00e8fe;
    font-size: 12px;
    padding: 0 2px !important;
    box-shadow: none !important;
    text-transform: none;
    /* margin-top: 1px; */
  }
  .subscription_wrap .subscription_content .plan_wrap .form-check-label {
    font-size: 12px !important;
  }
  .subscription_wrap .subscription_content .plan_wrap .buy_later_button {
    border: 1px solid #00e8fe;
    color: #00e8fe;
    background-color: #fff;
  }
}
</style>