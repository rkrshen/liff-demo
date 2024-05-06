<script lang="ts" setup>
import liff from '@line/liff';
import { ref, readonly, computed } from 'vue'

const order = ref({
    name: '',
    phone: '',
    items: [],
    amount: 0
})

const meals = ref([
    { name: '漢堡', price: 50 },
    { name: '薯條', price: 30 },
    { name: '可樂', price: 20 }
])

const total = computed(() => {
    return order.value.items.reduce((acc, item) => {
        const price = meals.value.find(meal => meal.name === item).price
        return acc + price
    }, 0)
})

const itemToText = computed(() => {
    return order.value.items.join('、')
})

const sendOrder = () => {
    order.value.amount = total.value
    liff.sendMessages([
        {
            type: 'text',
            text: `您訂購的餐點為：${itemToText}，總金額為：${total.value} 元。`,
        },
    ])
}
</script>

<template>
    <div>
        <h1>點餐系統</h1>
        <table>
            <tr>
                <td>
                    <label for="name">姓名：</label>
                </td>
                <td>
                    <input type="text" v-model="order.name" placeholder="姓名">
                </td>
            </tr>
            <tr>
                <td>
                    <label for="phone">電話：</label>
                </td>
                <td>
                    <input type="text" v-model="order.phone" placeholder="電話">
                </td>
            </tr>
            <tr>
                <td>
                    <label for="items">餐點：</label>
                </td>
                <td>
                    <input type="checkbox" v-model="order.items" value="漢堡">漢堡
                    <input type="checkbox" v-model="order.items" value="薯條">薯條
                    <input type="checkbox" v-model="order.items" value="可樂">可樂
                </td>
            </tr>
            <tr>
                <td>
                    <label for="amount">總金額：</label>
                </td>
                <td>
                    <p>{{ total }} 元</p>
                </td>
            </tr>
        </table>
        <a @click="sendOrder">送出訂單</a>
    </div>
</template>