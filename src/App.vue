<template>
	<div id="shopping-list">
		<div class="header">
			<h1>{{ header.toLocaleUpperCase() }}</h1>
			<button class="btn btn-primary" v-if="state === 'default'" @click="state = 'edit'">Add Item</button>
			<button class="btn btn-cancel" v-else @click="state = 'default'">Cancel</button>
		</div>
		<transition name="slide" appear>
			<div v-if="state === 'edit'" class="add-item-form">
				<input v-model="newItem" type="text" placeholder="Add an item" @keyup.enter="saveItem">
				<button class="btn btn-primary" :disabled="newItem.length === 0" @click="saveItem">Save Item</button>
			</div>
		</transition>

		<transition name="slide" mode="out-in">
			<p v-if="items.length === 0">
				You have purchased all the items
			</p>
			<ul v-else>
				<transition-group appear name="multiple">
					<li 
						v-for="(item, index) in items" 
						:key="index"
						@click="onPurchase(item, index)">
						{{ item.label }}
					</li>
				</transition-group>
			</ul>
		</transition>
		<hr/>
		<ul>
			<transition-group appear name="multiple">
				<li 
					v-for="(item, index) in purchasedItems" 
					:key="index"
					:class="{strikeout: item.purchased}" 
					@click="onUnPurchase(item, index)">
					{{ item.label }}
				</li>
			</transition-group>
		</ul>
	</div>
</template>

<script>

    export default {
        data() {
            return {
				newItem: '',
				state: 'default',		// can be default or edit
				header: 'Shopping Cart',
				items: [
					{
						label: '10 party hats',
						purchased: false
					},
					{
						label: '2 board games',
						purchased: true
					}
				],
				purchasedItems: [
					{
						label: '20 cups',
						purchased: true
					}
				]
            }
		},
		computed: {
			reversedArray() {
				return this.items.slice(0).reverse()
			}
		},
		methods: {
			saveItem() {
				this.items.push({
					label: this.newItem,
					purchased: false
				})
				this.newItem = ''
			},
			onPurchase(item, index) {
				this.items.splice(index, 1)
				this.purchasedItems.push({
					label: item.label,
					purchased: true
				})
			},
			onUnPurchase(item, index) {
				this.purchasedItems.splice(index, 1)
				this.items.push({
					label: item.label,
					purchased: false
				})
			}
		}
    }
</script>


<style>
	.fade-enter {
		opacity: 0;
	}

	.fade-enter-active {
		transition: opacity 0.3s;
	}

	.fader-leave {

	}

	.fade-leave-active {
		transition: opacity 0.3s;
		opacity: 0;
	}

	.multiple-enter {
	opacity: 0;
	transform: translateY(20px);
	}

	.multiple-enter-active {
	transition: all 0.5s;
	}

	.multiple-leave {
	}

	.multiple-leave-active {
	transition: all 0.3s;
	transform: translateY(20px);
	opacity: 0;
	position: absolute;
	}

	.multiple-move {
	transition: transform .5s;
	}

	.slide-enter {
		transform: translateY(30px);
		opacity: 0;
	}

	.slide-enter-active {
		transition: all 0.3s;
	}

	.slide-leave {
	}

	.slide-leave-active {
		transition: all 0.3s;
		transform: translateY(30px);
		opacity: 0;
	}
</style>
