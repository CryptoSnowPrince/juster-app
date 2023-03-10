<script setup>
import { ref, onMounted } from "vue"

/**
 * Local
 */
import UserCard from "@/components/local/UserCard"

/**
 * UI
 */
import Modal from "@/components/ui/Modal"

/**
 * API
 */
import { fetchEventParticipants } from "@/api/events"

const props = defineProps({ show: Boolean, event: Object })

const users = ref([])

onMounted(async () => {
	users.value = await fetchEventParticipants({ id: props.event.id })
})
</script>

<template>
	<Modal :show="show" width="500" closable @onClose="$emit('onClose')">
		<div :class="$style.title">Participants</div>

		<div :class="$style.block">
			<div :class="$style.subtitle">Creator</div>
			<div :class="$style.users">
				<UserCard
					:user="{ userId: event.creatorId, creator: true }"
					:class="$style.user"
				/>
			</div>
		</div>

		<div :class="$style.subtitle">
			Users who participate <span>{{ users.length }}</span>
		</div>
		<div v-if="users.length" :class="$style.users">
			<UserCard
				v-for="user in users"
				:key="user.id"
				:user="user"
				:class="$style.user"
			/>
		</div>

		<div v-else :class="$style.empty">
			<Icon name="help" size="16" /> Wait for the initial liquidity to be
			provided
		</div>

		<div :class="$style.hint">
			The list of participants displays users who made bets or provided
			liquidity for this event
		</div>
	</Modal>
</template>

<style module>
.wrapper {
}

.title {
	font-size: 20px;
	font-weight: 600;
	line-height: 1.2;
	color: var(--text-primary);

	margin-bottom: 24px;
}

.block {
	margin-block: 32px;
}

.subtitle {
	display: flex;
	justify-content: space-between;

	font-size: 14px;
	font-weight: 600;
	color: var(--text-tertiary);

	margin-bottom: 12px;
}

.users {
	display: flex;
	flex-direction: column;
	gap: 8px;

	max-height: 400px;
	overflow-y: auto;

	margin-bottom: 16px;
}

.empty {
	display: flex;
	align-items: center;
	gap: 8px;

	font-size: 14px;
	line-height: 1.6;
	color: var(--text-secondary);
	fill: var(--text-tertiary);
}

.hint {
	font-size: 13px;
	font-weight: 500;
	line-height: 1.6;
	color: var(--text-tertiary);
	text-align: center;
	max-width: 330px;

	margin: 0 auto;
}
</style>
