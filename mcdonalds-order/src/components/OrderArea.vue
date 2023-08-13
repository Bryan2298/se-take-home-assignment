<template>
	<div class="order-sections">
		<div>
			<h2>PENDING</h2>
			<ul>
				<li v-for="order in pendingOrders" :key="order.number">{{ order.type }} Order #{{ order.number }}</li>
			</ul>
		</div>
		<div>
			<h2>PROCESSING</h2>
			<ul>
				<li v-for="order in processingOrders" :key="order.number">
					{{ order.type }} Order #{{ order.number }} (Bot {{ order.bot.id }}) - Processing...
				</li>
			</ul>
		</div>
		<div>
			<h2>COMPLETE</h2>
			<ul>
				<li v-for="order in completedOrders" :key="order.number">{{ order.type }} Order #{{ order.number }}</li>
			</ul>
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			pendingOrders: Array,
			processingOrders: Array,
			completedOrders: Array,
		},
	};
</script>

<style scoped>
.order-sections {
	display: flex;
	justify-content: center;
}

.order-sections h2{
	width: 300px;
}


</style>