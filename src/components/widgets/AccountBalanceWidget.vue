<template>
	<Card :loading="loading">
		<template #title>{{getNameByKey(title)}}</template>

		<template #body>
			<div class="body-wrapper">
				<div class="body">
					<div class="section">
						<svg
							class="account-icon"
							viewBox="15 15 115 115"
							:width="55"
							:height="55"
						>
							<AccountIcon
								:width="128"
								:height="128"
								:address="address"
								hideCaption
							/>
						</svg>
						<div>
							<img :src="ConnectorIcon" class="icon noselect" />
							<div class="address">{{address}}</div>
						</div>
					</div>
					<div class="mosaic">{{mosaicName}}</div>
					<Decimal class="balance" :value="balance" />	
				</div>
			</div>
		</template>
	</Card>
</template>

<script>
import Card from '@/components/containers/Card.vue';
import Decimal from '@/components/fields/Decimal.vue';
import AccountIcon from '../graphics/AccountIcon.vue';
import ConnectorIcon from '../../styles/img/connector_bg_1.png';

export default {
	props: {
		managerGetter: String,
		dataGetter: String,
		title: {
			type: String,
			default: 'accountBalanceTitle'
		}
	},

	components: {
		Card,
		AccountIcon,
		Decimal
	},

	data() {
		return {
			ConnectorIcon
		};
	},

	computed: {
		data() {
			return this.dataGetter
				? this.$store.getters[this.dataGetter]
				: this.$store.getters[this.managerGetter].data;
		},

		balance() {
			return this.data.balance || 0;
		},

		address() {
			return this.data.address;
		},

		mosaicName() {
			return this.data.mosaicName || 'XYM';
		},

		loading() {
			return this.$store.getters[this.managerGetter].loading;
		},

		error() {
			return this.$store.getters[this.managerGetter].error;
		}
	},

	methods: {
		getNameByKey(e) {
			return this.$store.getters['ui/getNameByKey'](e);
		}
	}
};
</script>

<style lang="scss" scoped>
.body-wrapper {
	display: flex;
	justify-content: center;
	align-items: center;

	.body {
		background: linear-gradient(120deg, var(--primary) 0%, var(--secondary) 100%);
		color: #fff;
		border-radius: 5px;
		position: relative;
		display: inline-block;
		padding: 40px;

		.section {
			display: flex;
			flex-direction: row;
			justify-content: space-between;

			.account-icon {
				margin-right: 20px;
				background: #fff;
				border-radius: 20px;
				padding: 10px;
			}

			.address {
				font-size: 1rem;
				line-height: 1.75rem;
				margin: 0 0 33.2px;
				max-width: 60%;
			}

			.icon {
				position: absolute;
				bottom: 0;
				right: 0;
				height: 100%;
				pointer-events: none;
			}
		}

		.mosaic {
			font-size: 2.5rem;
			line-height: 3.25rem;
		}

		.balance {
			font-size: 2.5rem;
			line-height: 3.25rem;
		}
	}
}
</style>