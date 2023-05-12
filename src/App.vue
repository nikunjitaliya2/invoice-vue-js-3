<script setup>
import Home from './components/Home.vue'
</script>

<template>
  <div class='container-fluid border-bottom py-2'>
    <div class='d-flex justify-content-between align-items-center pt-2'>
      <div>
        <h5 class='' style='color: #0F2744;'>Create Invoice</h5>
        <div class='d-flex'>
          <h6>cash</h6>
          <div class='px-2'>
            <label class='switch'>
              <input type='checkbox'>
              <span class='slider round'></span>
            </label>
          </div>
          <h6>Credit</h6>
        </div>
      </div>
      <div class='d-flex align-items-baseline'>
        <h5 class='common-text'>Balance Due</h5>
        <h3 class='px-2 text-style-common' style='color: #020C60'>0.00</h3>
        <p class='pe-2 pe-4'>PRs</p>
        <i class="fa-solid fa-gear fs-3"></i>
      </div>
    </div>
  </div>
  <Home/>
</template>

<style scoped>
body {
  background: #E0E0E0;
}

.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}

.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 20px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 12px;
  width: 12px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}
</style>
