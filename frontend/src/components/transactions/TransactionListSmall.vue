<!--
  - TransactionListSmall.vue
  - Copyright (c) 2020 james@firefly-iii.org
  -
  - This file is part of Firefly III (https://github.com/firefly-iii).
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program.  If not, see <https://www.gnu.org/licenses/>.
  -->

<template>
    <table class="table table-striped table-sm">
        <caption style="display:none;">{{ $t('firefly.transaction_table_description') }}</caption>
        <thead>
        <tr>
            <th scope="col" class="text-left">{{ $t('firefly.description') }}</th>
            <th scope="col" class="text-right">{{ $t('firefly.amount') }}</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="transaction in this.transactions">
            <td>
                <a :href="'transactions/show/' + transaction.id " :title="transaction.date">
                    <span v-if="transaction.attributes.transactions.length > 1">{{ transaction.attributes.group_title }}</span>
                    <span v-if="1===transaction.attributes.transactions.length">{{ transaction.attributes.transactions[0].description }}</span>
                </a>
            </td>
            <td style="text-align:right;">
                <span v-for="tr in transaction.attributes.transactions">
                     <span v-if="'withdrawal' === tr.type" class="text-danger">
                        {{ Intl.NumberFormat('en-US', {style: 'currency', currency: tr.currency_code}).format(tr.amount * -1)}}<br>
                     </span>
                    <span v-if="'deposit' === tr.type" class="text-success">
                        {{ Intl.NumberFormat('en-US', {style: 'currency', currency: tr.currency_code}).format(tr.amount)}}<br>
                     </span>
                    <span v-if="'transfer' === tr.type && tr.source_id === account_id" class="text-info">
                        {{ Intl.NumberFormat('en-US', {style: 'currency', currency: tr.currency_code}).format(tr.amount * -1)}}<br>
                    </span>
                    <span v-if="'transfer' === tr.type && tr.destination_id === account_id" class="text-info">
                        {{ Intl.NumberFormat('en-US', {style: 'currency', currency: tr.currency_code}).format(tr.amount)}}<br>
                    </span>
                </span>
            </td>
        </tr>
        </tbody>
    </table>
</template>

<script>
    export default {
        name: "TransactionListSmall",
        props: {
            transactions: {
                type: Array,
                default: function () {
                    return [];
                }
            },
            account_id: {
                type: Number,
                default: function() {
                    return 0;
                }
            },
        }
    }
</script>

<style scoped>

</style>
