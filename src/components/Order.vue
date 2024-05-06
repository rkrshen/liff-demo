<script lang="ts" setup>
import liff from '@line/liff';
import { ref, defineModel, computed } from 'vue'


const order = ref({
    name: '',
    phone: '',
    items: [],
    amount: 0
})

interface Meal {
    name: string
    price: number
}

const meals: Meal[] = [
    { name: '漢堡', price: 50 },
    { name: '薯條', price: 30 },
    { name: '可樂', price: 20 },
]

const total = computed(() => {
    return order.value.items.reduce((acc, item) => {
        const price = meals.find(meal => meal.name === item)?.price ?? 0
        return acc + price
    }, 0)
})

const itemToText = computed(() => {
    return order.value.items.join('、')
})

const sendOrder = async() => {
    order.value.amount = total.value
    await liff.sendMessages([
        {
            type: 'text',
            text: `我這次訂購的餐點為：${itemToText.value}，總金額為：${total.value} 元。`,
        },
    ])
    logout()
}

const shareOrder = async() => {
    await liff.shareTargetPicker([
        {
            type: 'text',
            text: `我這次訂購的餐點為：${itemToText.value}，總金額為：${total.value} 元。`,
        },
    ])
}

const logout = () =>{
    liff.logout();
    liff.closeWindow();
}

interface UserData {
    displayName: string
    pictureUrl: string
}

const modelValue: any = defineModel()
</script>

<template>
    <div>
        <h1>點餐系統</h1>
        <img class="profile_pic" :src="modelValue.pictureUrl" alt="profile picture">
        <table>
            <tr>
                <td>
                    <label for="name">姓名：</label>
                </td>
                <td>
                    <p>{{ modelValue.displayName }}</p>
                </td>
            </tr>
            <tr>
                <td>
                    <label for="phone">電話：</label>
                </td>
                <td>
                    <input type="tel" v-model="order.phone" placeholder="電話">
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
        <div class="footer">
            <a class="button" @click="sendOrder">送出訂單</a>
            <a class="button" @click="shareOrder">分享我的訂單</a>
        </div>
    </div>
</template>

<style scoped>
    .profile_pic {
        width: 100px;
        height: 100px;
        border-radius: 50%;
    }
    .footer {
        margin-top: 20px;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }
    .button {
        display: inline-block;
        padding: 6px 10px;
        background-color: #10B980;
        color: white;
        text-decoration: none;
        border-radius: 5px;
    }
</style>