<template>
<div class="donation-form">
	<template v-if="paymentStep === 'selectValue'">
		<h2>Escolha um valor para doar</h2>
		<selectValue />
	</template>
	<template v-else-if="paymentStep === 'finalMessage'">
		<finalMessage />
	</template>
	<template v-else>
		<div class="donation-form-title">
			<h2>Você escolheu doar:</h2>
			<h2 v-if="amount">R$ {{ amount | formatBRL }}</h2>
		</div>
		<a class="donation-nav donation-nav--rewind" href="#" @click.prevent="goBack()">voltar</a>
		<ul>
			<li :class="paymentStep === 'userData' ? 'active' : ''">
			passo 1
			</li>
			<li :class="paymentStep === 'cardData' ? 'active' : ''">
			passo 2
			</li>
		</ul>

		<userData v-if="paymentStep === 'userData'"/>
		<cardData v-if="paymentStep === 'cardData'"/>
		<addressData v-if="paymentStep === 'address'"/>
	</template>
</div>
</template>

<script>
// @ is an alias to /src
import selectValue from '@/components/steps/selectValue.vue';
import userData from '@/components/steps/userData.vue';
import cardData from '@/components/steps/cardData.vue';
import finalMessage from '@/components/steps/finalMessage.vue';
import addressData from '@/components/steps/addressData.vue';

export default {
  name: 'Payment',
  components: {
    selectValue,
    userData,
    cardData,
    finalMessage,
    addressData,
  },
  computed: {
    paymentStep() {
      return this.$store.state.paymentStep;
    },
    amount() {
      return this.$store.state.amount;
    },
  },
  methods: {
    goBack() {
      const step = this.paymentStep === 'userData' ? 'selectValue' : 'userData';
      this.$store.dispatch('CHANGE_PAYMENT_STEP', { step });
    },
  },
};
</script>
