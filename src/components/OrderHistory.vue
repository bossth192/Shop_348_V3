<script setup>
import HeaderList from './HeaderList.vue';
import { orders } from '../stores/History';
import { computed } from 'vue';

const totalPrice = (item) => computed(() => item.quantity * item.price);

const totalItemsInOrder = (order) => {
  return order.items.reduce((accum, item) => accum + item.quantity, 0);
};

const totalOrderPrice = (order) => {
  return order.items.reduce((accum, item) => accum + item.quantity * item.price, 0);
};
</script>

<template>
  <HeaderList></HeaderList>
  <div class="container_all">
    <h2>ประวัติการสั่งซื้อ</h2>

    <div v-if="orders.list.length">
      <div class="card rounded-3 mb-4" v-for="(order) in orders.list" :key="order.id">
        <h3 class="m-3">คำสั่งซื้อที่ : {{ order.id }} </h3>
        <div class="card-body p-4" v-for="(item, index) in order.items" :key="item.name">
          <div class="row d-flex justify-content-between align-items-center">
            <div class="col-md-2 col-lg-2 col-xl-2">
              <img :src="item.img" class="img-fluid rounded-3" alt="Cotton T-shirt">
            </div>
            <div class="col-md-3 col-lg-3 col-xl-3">
              <span>รายการ {{ index + 1 }} </span>
              <p class="lead fw-normal mb-2">{{ item.name }}</p>
              <p><span class="text-muted">ราคา : {{ item.price }} บาท</span></p>
            </div>
            <div class="col-md-3 col-lg-3 col-xl-2 d-flex">
              <input id="form1" min="0" name="quantity" type="number" v-model="item.quantity" class="form-control form-control-sm" />
            </div>
            <div class="col-md-3 col-lg-2 col-xl-2 offset-lg-1">
              <h5 class="mb-0">{{ totalPrice(item) }} บาท</h5>
            </div>
          </div>
        </div>
        <div class="card-body p-4">
          <div class="float-end">
            <p class="mb-0 me-5 d-flex align-items-center">
              <h4><span class="small text-muted me-2"> รายการทั้งหมด {{ totalItemsInOrder(order) }} จำนวน </span></h4>
            </p>
            <p class="mb-0 me-5 d-flex align-items-center">
              <strong><h3><span class="small text-muted me-2"> ยอดรวมทั้งหมด {{ totalOrderPrice(order) }} บาท</span></h3></strong>
            </p>
          </div>
        </div>
      </div>
    </div>

    <div v-else>
      <p style="color: red;">**ไม่มีรายการคำสั่งซื้อ**</p>
    </div>
  </div>
</template>