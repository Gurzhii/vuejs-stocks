<template>
    <div class="col-sm-6 col-md-4">
        <div class="panel panel-success">
            <div class="panel-heading">
                <h3 class="panel-title">
                    {{ stock.name }}
                    <small>(Price: {{ stock.price }})</small>
                </h3>
            </div>
            <div class="panel-body">
                <div class="pull-left">
                    <input type="number" class="form-control" :class="{danger: insufficientFunds}" placeholder="Quantity" v-model="quantity">
                </div>
                <div class="pull-right">
                    <button :disabled="canBeBought" @click="buyStock" class="btn btn-success">
                        {{ (insufficientFunds) ? 'Insufficient funds' : 'Buy'}}
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['stock'],
        computed: {
            funds() {
                return this.$store.getters.funds;
            },
            insufficientFunds() {
                return this.quantity * this.stock.price > this.funds;
            },
            canBeBought() {
                return (this.quantity <= 0 || !Number.isInteger(this.quantity) || this.insufficientFunds);
            }
        },
        data() {
            return {
                quantity: 0
            }
        },
        methods: {
            buyStock() {
                const order = {
                    stockId: this.stock.id,
                    stockPrice: this.stock.price,
                    quantity: this.quantity
                };
                this.$store.dispatch('buyStock', order);
                this.resetQuantity();
            },
            resetQuantity() {
                this.quantity = 0;
            }
        }
    }
</script>

<style scoped>
    .danger {
        border: 1px solid red;
    }
</style>