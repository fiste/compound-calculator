<template>
  <div class="home">
    <b-container>
      <h1 class="my-3">Compound calculator</h1>
      <b-form class="mt-5" @submit="onSubmit">
        <b-form-group
          id="input-group-1"
          label="Investment amount:"
          label-for="input-1"
        >
          <b-form-input
            id="input-1"
            v-model="form.amount"
            placeholder="Investment amount"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-1"
          label="Days to invest:"
          label-for="input-1"
        >
          <b-form-input
            id="input-1"
            v-model="form.days"
            placeholder="Days to invest"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-1"
          label="Re-deposit amount:"
          label-for="input-1"
        >
          <b-form-input
            id="input-1"
            v-model="form.reDeposit"
            placeholder="Re-deposit amount"
          ></b-form-input>
        </b-form-group>

        <b-form-group
          v-if="form.reDeposit"
          id="input-group-1"
          label="Re-deposit every (days):"
          label-for="input-1"
        >
          <b-form-input
            id="input-1"
            v-model="form.reDepositDays"
            placeholder="Re-deposit every (days)"
          ></b-form-input>
        </b-form-group>

        <p class="text-primary">1% of daily interest</p>

        <b-button type="submit" variant="primary" class="mr-3">Submit</b-button>
      </b-form>

      <b-table class="mt-5" striped hover :items="items"></b-table>
    </b-container>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class Home extends Vue {
  private items: any = [];
  private form: any = {
    amount: null,
    days: null,
    reDeposit: null,
    reDepositDays: null
  };

  private onSubmit(event: any) {
    event.preventDefault();
    this.items = [];
    for (let i = 1; i < Number(this.form.days) + 1; i++) {
      if (
        this.form.reDeposit &&
        this.form.reDepositDays &&
        i % Number(this.form.reDepositDays) == 0
      ) {
        this.form.amount =
          Number(this.form.amount) + Number(this.form.reDeposit);
        this.items.push({
          day: i,
          amount_with_interest: Number(this.form.amount).toFixed(3),
          amount_of_interest: (Number(this.form.amount) / 100).toFixed(3),
          interest: "1%",
          redeposit: this.form.reDeposit
        });
      } else {
        this.form.amount =
          Number(this.form.amount) + Number(this.form.amount) / 100;

        this.items.push({
          day: i,
          amount_with_interest: Number(this.form.amount).toFixed(3),
          amount_of_interest: (Number(this.form.amount) / 100).toFixed(3),
          interest: "1%"
        });
      }
    }

    // if (this.form.reDeposit && this.form.reDepositDays) {
    //   this.items = this.items.map((item: any, index: number) => {
    //     if (index % Number(this.form.reDepositDays) == 0) {
    //       return {
    //         ...item,
    //         amount_with_interest:
    //           Number(item.amount_with_interest) + Number(this.form.reDeposit),
    //         amount_of_interest: (
    //           (Number(item.amount_with_interest) +
    //             Number(this.form.reDeposit)) /
    //           100
    //         ).toFixed(3),
    //         redeposit: this.form.reDeposit
    //       };
    //     }

    //     return { ...item, Redeposit: null };
    //   });
    // }

    this.form.amount = null;
    this.form.days = null;
    this.form.reDeposit = null;
    this.form.reDepositDays = null;
  }
}
</script>
