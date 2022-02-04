<script setup>
// import HelloWorld from './components/HelloWorld.vue'
import { ref, watch, computed } from 'vue'

const tax = ref(6)
const receive = ref(0)
const bill = ref(0)

const getMultiplier = (theTax) => Number(1 - (theTax / 100)).toPrecision(2)

let isReceiveChanged = false

watch([tax, receive, bill], (newValue, oldValue) => {
	const [oldTax, oldReceive, oldBill] = oldValue
	const [newTax, newReceive, newBill] = newValue

	const multiplier = getMultiplier(newTax)

	if (oldTax !== newTax) {
		isReceiveChanged = true
	}

	if (oldBill !== newBill) {
		if (isReceiveChanged) {
			isReceiveChanged = false
			return
		}
		receive.value = Number(newBill * multiplier).toFixed(2)
		return
	}

	if (typeof newReceive === 'string' && !isReceiveChanged) {
		return
	}

	isReceiveChanged = true

	const newBillValue = Number(newReceive / multiplier).toFixed(2)
	bill.value = newBillValue
	console.log(isReceiveChanged)
})


</script>

<template>
	<form class="form">
		<div>
			<div>tax</div>
			<input
				v-model="tax"
				type="number"
				step="1"
				min="0"
				placeholder="tax"
			/>
		</div>
		<div>
                        <div>receive</div>
                        <input
                                v-model="receive"
                                type="number"
                                min="0"
                                placeholder="0,00"
                        />
                </div>
		<div>
                        <div>bill</div>
                        <input
                                v-model="bill"
                                type="number"
                                min="0"
                                placeholder="0,00"
                        />
                </div>
	</form>
</template>

<style>
* {
	margin: 0;
	padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
	display: flex;
	justify-content: center;
	align-items: center;
	height: 100vh;
}

.form {
	display: flex;
}

.form > * {
	padding: 6px;
}

.form input {
	font-size: 20px;
	margin-top: 3px;
}

.form > div > div {
	font-size: 20px;
	font-weight: bold;
}

@media (max-width: 420px) {
	.form {
		flex-wrap: wrap;
	}
}
</style>
