<template>
  <el-dialog
    title="Your investment Simulation"
    :visible.sync="investmentDialogVisible"
    width="60%"
    :before-close="handleClose"
    center>
    <el-row :gutter="20">
      <el-col :span="12">
        <p>You invested <b>{{investmentContribution}}%</b> of your netSaving: <b>${{netSaving}}</b></p>
        <el-progress type="circle" :percentage="investmentContribution" color="green"></el-progress>
      </el-col>
      <el-col :span="12">
        <p>Your investment options and distribution</p>
        <div v-for="item in investmentOptions">
          <span><b>{{item.id}}:</b> <el-progress :percentage="item.percentage" color="green"></el-progress></span>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="24">
        <p>If you made the investment <b>1 Year ago</b> with TD, you would have made: <b>${{investmentGain.Yr1}}</b> and your saving will be: <b>${{Math.round((investmentGain.Yr1 + netSaving) * 100) / 100}}</b>!</p>
        <p>If you made the investment <b>5 Year ago</b> with TD, you would have made: <b>${{investmentGain.Yr5}}</b> and your saving will be: <b>${{Math.round((investmentGain.Yr5 + netSaving) * 100) / 100}}</b>!</p>
        <p>It's never TOO LATE to start investment, talk with TD to find the best investment options for your!</p>
        <el-button v-on:click.prevent="goTDInvest('call')" type="success">Request a Call Back</el-button>
        <p>Or you can start invest right now!</p>
        <el-button v-on:click.prevent="goTDInvest('account')" type="success">Invest Now</el-button>
        <div class="disclaimer"><p>Disclaimer: This Projected Saving displays what your portfolio income might look like based on previous historical performance.
          This is for estimation purpose ONLY, and TD <b> does NOT </b> garuantee you will make the same amount of money. </p>
        </div>
      </el-col>
    </el-row>

    <span slot="footer" class="dialog-footer">
      <el-button v-on:click.prevent="closeDialog">Done</el-button>
    </span>
  </el-dialog>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'InvestmentDialog',
  computed: {
    ...mapGetters([
      'investmentGain',
      'investmentDialogVisible',
      'netSaving',
      'investmentOptions',
      'investmentContribution'
    ])
  },
  methods: {
    closeDialog() {
      this.$store.dispatch('closeInvestmentDialog')
    },
    handleClose(done) {
      this.$store.dispatch('closeInvestmentDialog')
      done()
    },
    goTDInvest(str) {
      var url = str === 'call' ? 'https://www.forms.td.com/app/di-request-call/#/di-request-call/di-request-call-form' : 'https://wb.authentication.td.com/uap-ui/index.html?consumer=webbroker&locale=en_CA#/login/webbroker-getting-started'
      window.open(url, '_blank')
    }
  }
}
</script>
<style rel="stylesheet/scss" lang="scss" scoped>
.disclaimer{
  background-color: #FFFACD;
  font-weight: bold;
  font-size: small;
}
</style>
