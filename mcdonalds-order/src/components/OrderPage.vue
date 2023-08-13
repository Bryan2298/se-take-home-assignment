<template>
	<div class="order-container">
		<button @click="addNormalOrder">New Normal Order</button>
		<button @click="addVIPOrder">New VIP Order</button>
		<button @click="addBot">+ Bot</button>
		<button @click="removeBot">- Bot</button>
		<OrderArea
			:pendingOrders="pendingOrders"
			:processingOrders="processingOrders"
			:completedOrders="completedOrders"
		/>
		<BotArea :bots="bots" />
	</div>
</template>

<script>
import { ref, computed } from "vue";
import OrderArea from "@/components/OrderArea.vue";
import BotArea from "@/components/BotArea.vue";

export default {

	components: {
		OrderArea,
		BotArea,
	},
	setup() {
		const orderCounter = ref(1);
		const pendingOrders = ref([]);
		const processingOrders = ref([]);
		const completedOrders = ref([]);
		const bots = ref([]);

		const addNormalOrder = () => {
			const order = {
				number: orderCounter.value,
				type: "NORMAL",
			};
			orderCounter.value++;
			pendingOrders.value.push(order);
		};

		const addVIPOrder = () => {
			const order = {
				number: orderCounter.value,
				type: "VIP",
			};
			orderCounter.value++;
			const vipIndex = pendingOrders.value.findIndex((order) => order.type === "NORMAL");
			pendingOrders.value.splice(vipIndex, 0, order);
		};

		const addBot = () => {
			const bot = {
				id: bots.value.length + 1,
				status: "IDLE",
			};
			bots.value.push(bot);
			processOrders();
		};

		const removeBot = () => {
			const removedBot = bots.value.pop();
			if (bots.value.length) {
				if (removedBot.status === "PROCESSING") {
					const processingIndex = processingOrders.value.findIndex(order => order.bot.id === removedBot.id);
					if (processingIndex !== -1) {
						const order = processingOrders.value[processingIndex];
						order.bot = null;
						pendingOrders.value.push(order);
						processingOrders.value.splice(processingIndex, 1);
					}
				}
			} else {
				alert("No bots to remove");
			}
		};

		const processOrders = () => {
			for (const bot of bots.value) {
				if (bot.status === "IDLE" && pendingOrders.value.length > 0) {
					const order = pendingOrders.value.shift();
					order.bot = bot;
					processingOrders.value.push(order);
					bot.status = "PROCESSING";

					setTimeout(() => {
						bot.status = "IDLE";
						const processingIndex = processingOrders.value.findIndex(o => o.number === order.number);
						if (processingIndex !== -1) {
							processingOrders.value.splice(processingIndex, 1);
							completedOrders.value.push(order);
						}
						processOrders();
					}, 10000);
				}
			}

			// If there are no available bots, cancel processing for pending orders
			if (bots.value.every(bot => bot.status === "PROCESSING")) {
				for (const order of pendingOrders.value) {
					cancelOrderProcessing(order);
				}
			}
		};

		const cancelOrderProcessing = (order) => {
			const processingIndex = processingOrders.value.findIndex(o => o.number === order.number);
			if (processingIndex !== -1) {
				const removedOrder = processingOrders.value.splice(processingIndex, 1)[0];
				removedOrder.bot = null;
				pendingOrders.value.push(removedOrder);
				processOrders();
			}
		};

		const idleBots = computed(() => {
			return bots.value.filter(bot => bot.status === "IDLE");
		});

		return {
			pendingOrders,
			processingOrders,
			completedOrders,
			bots,
			addNormalOrder,
			addVIPOrder,
			addBot,
			removeBot,
			idleBots,
		};
	},
};
</script>

<style scoped>
.order-container button {
	margin: 10px;
	padding: 5px;
}

</style>
